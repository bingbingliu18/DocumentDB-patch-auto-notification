# DocumentDB Patch Auto Notification
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

<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/217128118-1e570ab8-5310-42ca-8130-21a07a4c30aa.png">

## Create Lambda
**Lambda Configuration：**

**Lambda name:** 
**query_docdb_maintenance**

**Lambda role:** 
**lambda-query-maintenance**

### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
