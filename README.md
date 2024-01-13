# Serverless File Uploader to S3 using API Gateway and Lambda
## Infrastructure

        **Still working on this**

## Create IAM Role
1.	In the search bar on Management Console search for **AWS Identity and Access Management (IAM)**. Then click IAM.
2.	Click **Roles** on the sidebar of the IAM console.
3.	Click on **Create Role**
4.	Choose **Lambda** for the Service or use case then click **Next**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/6569bbb6-cbec-40df-9867-43054bcf48ae)

5.	On Add Permissions, search up for ```AWSLambdaExecute``` then click the checkbox and proceed to next.
6.	Give it a Role name of ```lambda-api-gateway-role``` then click **Create Role**.

## Uploading the pdf file to S3 using AWS Lambda and API Gateway

## Amazon S3
1.	Go to **S3** then click for **Create Bucket**
2.	Give it a **Bucket name** whatever you want. //**Please remember your bucket name**// since we will use it for later. On my example, i used ```aldrin.bucket```
3.	Then Click for **Create Bucket** on the bottom of the page.

## AWS Lambda
1.	Open **AWS Lambda** then Create a Function
2.	Name the function as ```uploadAPI``` then select the Runtime to Python 3.12
3.	For Permissions choose Change default execution role then click **Use an existing role** on Execution role
4.	Under existing role, choose the role that we created earlier, **lambda-api-gateway-role**
5.	Then Click **Create Function**
6.  Go to **Code Source** tab then copy the code below.

```
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
    }
```

3.	In the code above please change the ```<bucketname>``` to the name of your bucket you created earlier.
4.	Click **Deploy**

## API Gateway

1.	Go to API Gateway then click for **Create API**
2.	Click **Build** on REST API

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/617df307-0aa3-4882-be55-a38f80ca02ac)

4. 	On Create Rest API page, Click on **New API** then name the API with **apiEndpoint**.
5.	Click **Create Resource** then put on the Resource name, upload. Then click Create,
6.	Go on /upload then click for **Create Method**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/2f6e721d-5876-4880-a1b9-332cda83a4dc)

6.	Under the Create Method window choose the Method Type to **POST** then **Lambda Function** for Integration type, choose the **uploadAPI** Lambda function we created earlier. Then **Create Method**
7.	On POST Click for **Integration Request** then **Edit**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/17360d83-5c4b-443f-835a-1916f34b4f5a)

8. Inside the **Integration Request** page, copy the following details below

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/3124fb39-0197-453c-9118-75a2ca7bb7ca)

9.	Click **Save**.
10.	On the left panel of the API Gateway you will see **API Settings**, click on that to be able to access the settings.
11.	Click **Manage Media Types** on Binary media types then input ```application/pdf``` as a media type. After that, **Save Changes**.
12.	Now that we have completed the configurations in our API, lets start to deploy our API.
13.	Click on **Resources** on the left pane then click **Deploy API**.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/17cb5738-ba90-43e1-9743-bb565bf429c1)

14.	Under the Deploy API, select ***New Stage*** then name it into **v1**. Then proceed to **Deploy**.
15.	After that go to the **POST** of ```v1/upload```, you will see there the Invoke URL. **Copy the URL**.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/8e6e089d-5b8f-40db-ae5f-ab2ccb247cce)

16.	Use **POSTMAN** to be able to upload the file.
17.	Under POSTMAN **paste the link** on the search bar then change the method into **POST**. Refer on the image below.
18.	Go for **Headers** then enter the **Key** of ```Content-Type``` with a Value of ```application/pdf```.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/c427467b-8c7a-464b-a1e9-0def712240d5)

19.	Then go on the **Body** tab and select **Binary**, this way POSTMAN will let you upload your pdf file. **Select your pdf file** you wish to upload on S3.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/10118916-ebdf-43e6-994e-c8bb7e39276c)

20.	After uploading the file click on **Send**. You should have received a response like this below. This means the Lambda code and API has executed the upload.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/c6830af0-f30e-4f13-a6b5-14ea30d51b65)

###  You have uploaded your file into a bucket!
22.	You can check if your file is in the bucket, so go to your bucket to see if its there.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/a7bb8502-4440-4d76-bb84-c53bdb424334)

## End of First Part

# Serverless File Fetcher from S3 using AWS Lambda and API Gateway

## AWS Lambda

1.	Open AWS Lambda then **Create a Function**
2.	Name your Function as ```accessFileS3API``` (on the image below I already have the same name so your name should work)
3.	Choose Runtime as **Python 3.12**
4.	For Permissions choose Change default execution role then click **Use an existing role** on Execution role
5.	Under existing role, choose the role that we created earlier, ```lambda-api-gateway-role```
6.	Then Click **Create Function**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/3cb65049-0da6-4cb7-9534-d13e3eca48b3)

7.	Before you copy the code, go to Configurations then click **Edit** on **General configuration**
8.	Change the Timeout from **3 sec to 10 sec** then click **Save**
9.	You may now start copying this code in the **Code source**

```
import json
import base64
import boto3

def lambda_handler(event, context):
    
    s3 = boto3.client("s3")
    print(event)
    
    #Legacy Method
    bucket_name = event["params"]['path']['bucket']
    file_name = event['params']['querystring']['file']
    
    #Fetching the object on S3
    fileObj = s3.get_object(Bucket = <bucket_name>, Key = file_name)
    file_content = fileObj["Body"].read()

    #converting the file0bj back into pdf
    return base64.b64encode(file_content)
```
10.	Make sure the you have changed the ```<bucket_name>``` to the name of bucket you created earlier.
11.	Hit Ctrl+S then click **Deploy**.

## API Gateway

12.	Create new API click **BUILD** on REST API
13.	Create Resource then name it ```{bucket}```
14.	Click on ```/{bucket}``` then click **Create Method** then choose **GET** on Method Type, Integration Type is **Lambda Function** and choose the function we created on Lambda beside ```us-west-2```. After that click **Create Method **. The image below is for reference.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/4b412a4c-8e2d-4456-b48f-aa412eaad613)

15.	Click Method Request then **Edit**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/197c0613-2aba-435e-a8a8-f68e83e88935)

16.	Under the Editing section, **copy the following inputs** based on the image below. Then click **Save**

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/fa4aaeac-da43-4345-9794-58d338a8a690)

17.	Click **Edit** on Integration Request then once again **copy the information** below. **Take note**: Under Mapping templates, **Generating template**, the Method request passthrough should be able to generate its own Template Body. After this click **Save**.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/e7986607-ad83-408a-8f4d-6f7be6be763a)

18.	Click **Edit** on Method Response then **input the details below**. Then click **Save**.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/8e5ea570-32a9-43de-b621-c67b28818968)

19.	Click **Edit** on Integration Response then **input again the details** on the image below. Click **Save** after this.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/35e10ea9-d13b-45f2-a41f-bdd67a601d96)

20.	We can now deploy our API, now click **Deploy API**. Then select ```*No Stage*``` then type on the Stage Name, ```v1```.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/dafbb0ca-fc11-4850-b1dc-03164c49f9f0)

21.	Go on **Stages** then find the Invoke URL for the GET function then copy the link.

![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/5bfec957-8234-4a86-83e7-48b5728ff2cd)

22.	You can either use any **browser** or **POSTMAN** to be able to access the pdf file.

## If you use browser: type the following below on search bar

### From:
```https://x5szddsyrd.execute-api.us-west-2.amazonaws.com/v2/{bucket}```
**Note: this link only works for me, you should have different link.**

### To this:
```https://x5szddsyrd.execute-api.us-west-2.amazonaws.com/v2/aldrin.bucket?file=content1.pdf```

**Note 1:** You should change ```{bucket}``` because that will depend on the name of the bucket where the pdf file stored.
**Note 2:** The ```content1.pdf``` is the name of the file we have converted and uploaded earlier in the process.

### There you go, you have your file back to you!

## If you use POSTMAN

1. From the Invoke URL link you copied, paste down that link to the search bar then add ```/{bucket}?file=content1.pdf``` at the end of the link. Note: {bucket} should have the name of the bucket you created earlier. For reference use the picture down below.
2. Make sure the Method type is set to **GET**.
3. For the Params, **follow** the format below
   
![image](https://github.com/didin012/Serverless-File-Uploader-to-S3-using-API-Gateway-and-Lambda/assets/104528282/920a012d-0647-4064-a801-d36067d51290)

4. Click **Send**

### There you go, you have your file back to you!

**Side Note:** you can save the pdf file by clicking the 3 … button beside **Save as example** then Save response file. New window will pop-up, find the directory you want to save the file then name it what you want and make sure to have ```.pdf``` on the end to be able to save it in a pdf format.

