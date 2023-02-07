# DocumentDB Patch Auto Notification
**Automatically query available DocumentDB Patch**
## Architecture Diagram：

<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/216918607-dc639069-21c5-488e-b647-4c8d6e2bf31a.png">

## Create an IAM Policy for Lambda
**Policy name: query-pending-maintenance**

**Policy  Json definition：**
```
{
    "Version": "2012-10-17", 
    "Statement": [
        {
            "Sid": "VisualEditor0", 
            "Effect": "Allow", 
            "Action": [
            "sns:Publish", 
            "ec2:DescribeRegions", 
            "rds:DescribeDBInstances", 
            "rds:DescribePendingMaintenanceActions", 
            "rds:DescribeDBClusters"
        ],
        "Resource": "*"
        }
    ]
}
```
## Create Role for Lambda
**Role Name: lambda-query-maintenance**

**Policy: query-pending-maintenance**
## Configure SNS service
1. **Create sns topic**

**docdb-patch-notification**

<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/217126642-d9cdab87-c21b-4763-8cae-c0acc9b17bac.png">

2. **Create topic subscription**
<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/217127581-30602287-8b0e-4e89-a314-f3924c7c0240.png">


<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/217127664-effd86cb-2d86-44a7-8914-57159a5715e1.png">

3. **Confirm subscription in the email：**

<img width="468" alt="image" src="https://user-images.githubusercontent.com/50776512/217141783-97d1714f-8d86-4480-8bb4-1133065893c6.png">


## Create Lambda
**Lambda Configuration：**

**Lambda name:** 
**query_docdb_maintenance**

**Lambda role:** 
**lambda-query-maintenance**

**Lambda Timeout:  7 Minutes（Change default timeout:  from 3 seconds to 7 minutes）：**

<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/217133071-05cf3be8-4b3b-4aba-a33f-ecbb509c98c3.png">

**Lambda Runtime: Python 3.7**

**Lambda Code:**
```

import boto3

import json

notification = "This is notification for DocumentDB patch \n"

def query_docdb():

    global notification

    EC2 = boto3.client('ec2')

    regions = EC2.describe_regions()

    for REGION in regions['Regions']:

        docDB = boto3.client('docdb',REGION['RegionName'])

        pma = docDB.describe_pending_maintenance_actions()

        if len(pma['PendingMaintenanceActions']) > 0:

            num = len(pma['PendingMaintenanceActions'])

            for inst in pma['PendingMaintenanceActions']:

                mw_type=inst['PendingMaintenanceActionDetails'][0]['Description']

                if mw_type != 'Bug Fixes':

                    num = num - 1

                    continue

                temp_string = "Region: " + REGION['RegionName'] +  " has docDB cluster needed to be upgrade.\n"

                notification += temp_string

                temp_string = "The Cluster has pending maintenance action: " + inst['ResourceIdentifier'] + "\n"

                notification += temp_string

                cls_identifier=inst['ResourceIdentifier'].split(':')[-1]

                try:

                    cls = docDB.describe_db_clusters(DBClusterIdentifier=cls_identifier)

                except Exception as e:

                        temp_string = "Failed to get cluster information " + cls_identifier + " Exception: " + e + "\n"

                        notification += temp_string

                        continue

                dbclus=cls['DBClusters']

                mw = dbclus[0]['PreferredMaintenanceWindow']

                temp_string = "The Cluster maintenance window UTC time: " + mw + "\n"

                notification += temp_string

            temp_string = "Region: " + REGION['RegionName'] + " has total " + str(num) +  " cluster found.\n"   

            notification += temp_string

        else:

            temp_string = "Region: " + REGION['RegionName'] + " No Cluster found.\n"

            notification += temp_string

def lambda_handler(event, context):

    global notification

    sns_client = boto3.client('sns')

    query_docdb()

    print (notification)

    sns_response = sns_client.publish (

        TargetArn = "arn:aws:sns:us-east-1:02818****:docdb-patch-notification",

        Subject = "DocumentDB Patch Notification",

        Message = json.dumps({'default': notification}),

        MessageStructure = 'json'

     )
     
  ``` 
  **You could copy Lambda Python Code from Deploy Lamda.py**
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
