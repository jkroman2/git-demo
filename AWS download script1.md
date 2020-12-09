## AWS Data Download

-   The API uses database credentials stored in AWS Secrets Manager, so you don't need to pass credentials in the API calls.

-   A user must be authorized to access the Data API. You can authorize a user to access the Data API by adding the AmazonRDSDataFullAccess policy, a predefined AWS Identity and Access Management (IAM) policy, to that user.

-   You can also create an IAM policy that grants access to the Data API. After you create the policy, add it to each user that requires access to the Data     API.

-   Generate an aws access key id and aws secret access key from IAM user created in first step

-   Store database credentials in AWS Secrets Manager

-   Make sure the Data API is enabled

-   RDS Data API has a hard response limit of 1MB, if your data exceeds 1MB you     can use LIMIT and OFFSET in your SQL Query or paginators

-   Key thing to note is that Aurora serverless cannot read from S3, as of April 2019.

import boto3

import pandas as pd

    cluster_arn = \<\#*Unique Amazon Resource Name: ARN associated with DB Instance*\>

    secret_arn = \<\#*Unique Amazon Resource Name: DB credentials saved in secret manager*\>

-   Setting up the connection to the Aurora database
```
rdsData = boto3.client(*'rds-data',
            region_name = '\<region cluster/instance is located\>',
            aws_access_key_id='\<access key id\>',
            aws_secret_access_key='\<secret access key\>')
```
-   Connecting to database and execute commands
```
query_response = rdsData.execute_statement(
                    resourceArn = cluster_arn,
                    secretArn = secret_arn,
                    database='\<database name\>',
                    sql = '\<query to extract data\>;')
```
-   Connecting to database and execute commands
```
print(query_response['records']) \<*Returns query_response output in xml format\>*
```
References:

<https://news.ycombinator.com/item?id=23520597>

<https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/data-api.html>

<https://www.mysqltutorial.org/mysql-limit.aspx>

<https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html>

<https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/#:~:text=Expand%20the%20Access%20Keys%20section,on%20the%20Security%20Credentials%20tab>.

<https://www.sqlshack.com/learn-mysql-what-is-pagination/>

<https://stackoverflow.com/questions/58306523/export-from-aurora-serverless-into-s3>

<https://stackoverflow.com/questions/35468372/using-boto3-to-interact-with-amazon-aurora-on-rds>

<https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html>

<https://boto3.amazonaws.com/v1/documentation/api/latest/guide/paginators.html>
