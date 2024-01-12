# Serverless File Uploader to S3 using API Gateway and Lambda
## CREATING IAM ROLE
1.	In the search bar on Management Console search for **AWS Identity and Access Management (IAM)**. Then click IAM.
2.	Click **Roles** on the sidebar of the IAM console.
3.	Click on **Create Role**
4.	Choose **Lambda** for the Service or use case then click **Next**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/6569bbb6-cbec-40df-9867-43054bcf48ae)

5.	On Add Permissions, search up for **AWSLambdaExecute** then click the checkbox and proceed to next.
6.	Give it a Role name of **lambda-api-gateway-role** then click **Create Role**.

## Uploading the pdf file to S3 using AWS Lambda and API Gateway

## Amazon S3
1.	Go to **S3** then click for **Create Bucket**
2.	Give it a **Bucket name** whatever you want. //**Please remember your bucket name**// since we will use it for later.
3.	Then Click for **Create Bucket** on the bottom of the page.

## AWS Lambda
1.	Open **AWS Lambda** then Create a Function
2.	Name the function as **uploadAPI** then select the Runtime to Python 3.12
3.	For Permissions choose Change default execution role then click **Use an existing role** on Execution role
4.	Under existing role, choose the role that we created earlier, **lambda-api-gateway-role**
5.	Then Click **Create Function**
6.  Go to **Code Source** tab then copy the code below.

'
import json
import base64
import boto3

def lambda_handler(event, context):
    
    s3 = boto3.client("s3")
    
    decode_content = base64.b64decode(event["content"])
        
    s3_upload = s3.put_object( Bucket="<bucketname>", Key='content1.pdf', Body = decode_content)
    
    return {
        'statusCode': 200,
        'body': json.dumps("Hello Lambda!")
    }'

3.	In the code above please change the //**<bucketname>**// to the name of your bucket you created earlier.
4.	Click **Deploy**

