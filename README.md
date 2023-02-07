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

### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
