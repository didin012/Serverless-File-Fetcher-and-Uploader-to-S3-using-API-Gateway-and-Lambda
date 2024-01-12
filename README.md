# Serverless File Uploader to S3 using API Gateway and Lambda
## CREATING IAM ROLE
1.	In the search bar on Management Console search for **AWS Identity and Access Management (IAM)**. Then click IAM.
2.	Click **Roles** on the sidebar of the IAM console.
3.	Click on **Create Role**
4.	Choose **Lambda** for the Service or use case then click **Next**

![image](https://github.com/didin012/AWS-Cloud-Projects/assets/104528282/84173351-2173-4e39-bcf5-431ce9c8eb96)

5.	On Add Permissions, search up for **AWSLambdaExecute** then click the checkbox and proceed to next.
6.	Give it a Role name of **lambda-api-gateway-role** then click **Create Role**.
