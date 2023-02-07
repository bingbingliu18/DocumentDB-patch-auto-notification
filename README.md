# DocumentDB Patch Auto Notification
**Automatically query available DocumentDB Patch**
## Architecture Diagram：

<img width="422" alt="image" src="https://user-images.githubusercontent.com/50776512/216918607-dc639069-21c5-488e-b647-4c8d6e2bf31a.png">

## Create an IAM Policy for Lambda
**Policy name: query-pending-maintenance**

**Policy  Json definition：**

**Please refer to Policy Json defefinition from the file deploy/policy.json**
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

**Please refer to Lambda Python Code from the file deploy/lamda.py**

**Change the python code：TargetArn = "arn:aws:sns:us-east-1:02818****:docdb-patch-notification" **change Account id to your Account id**

## Create event bridge


### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
