# simple-aws-cloudformation-s3

This repository has a simple AWS CloudFormation Template that creates an AWS S3 Bucket adds more security to it. This repository is for use with the [Cloud Security Bootcamp](https://www.cloudsecuritybootcamp.com/) run by [Cloud Security Podcast](https://cloudsecuritypodcast.tv/). Please reach out

The full Playlist for the FREE Cloud Security Bootcamp can be found [here](https://youtube.com/playlist?list=PLrU93JvkXVl5TamdCdCwzqFs6vt5vySFi).

### Pre-Requisite

Please ensure you have AWS Cli Installed and configured on your machine before you follow the comamands below:

#### **AWS Cli Commands to be in the following stages from 1-4 to complete the tasks**. 

* Please rename <NAME-OF-THE-BUCKET> with a valid AWS S3 bucket name & <NAME-OF-THE-STACK> with the name of the AWS Cloudformation Stack before running the following scripts.

* Please rename <OPTIONAL-Only-if-AWS-Profile-Exists> with your AWS Cli Profile only if you have a local AWS Cli profile on your machine.

* Please rename <REGION-FOR-AWS-S3-BUCKET> with the AWS Region where you want the AWS S3 bucket to be created.

#### Stage 1

At this stage, we are creating the AWS Cloudformation Template. This action once completed will create the AWS S3 bucket in your selected AWS Account.

```
aws cloudformation create-stack \
    --stack-name <stack-name> \
    --template-body file://1-create-aws-s3-with-no-security.yml \
    --parameters ParameterKey=BucketName,ParameterValue=<NAME-OF-THE-BUCKET>
    --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
    --region <REGION-FOR-AWS-S3-BUCKET>
 ``` 

#### Stage 2

```     
aws cloudformation deploy \
     --stack-name <NAME-OF-THE-STACK> \
     --template-file file://2-update-cfn-stack-aws-s3-accesscontrol.yml \
     --parameter-overrides BucketVersioning=Enabled \
     --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
     --region <REGION-FOR-AWS-S3-BUCKET>
 ``` 

#### Stage 3
``` 
aws cloudformation deploy 
     --stack-name <NAME-OF-THE-STACK> \
     --template-file file://3-update-cfn-stack-add-s3-bucket-versioning.yml  \
     --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
     --region <REGION-FOR-AWS-S3-BUCKET> 
```
#### Stage 4
``` 
aws cloudformation deploy 
      --stack-name <NAME-OF-THE-STACK> \
      --template-file file://4-final-step-update-cfn-to-show-output.yml  \
     --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
     --region <REGION-FOR-AWS-S3-BUCKET> 
```      
 
#### Final Stage - 
 
##### Cleanup the AWS S3 Bucket so you don't get charged by AWS
 
Check the Versioning status of your AWS S3 bucket
```
aws s3api get-bucket-versioning --bucket <NAME-OF-THE-BUCKET>
 --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
 --region <REGION-FOR-AWS-S3-BUCKET> 
```  
 
If you have Disabled AWS S3 Bucket versioning, then use the following 

```
aws s3 rm s3://<NAME-OF-THE-BUCKET> --recursive
 --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
 --region <REGION-FOR-AWS-S3-BUCKET> 
``` 
 
If you have Enabled AWS S3 Bucket versioning, then use the following 
```
aws s3api delete-objects --bucket <NAME-OF-THE-BUCKET> \ 
  --delete "$(aws s3api list-object-versions \
  --bucket "<NAME-OF-THE-BUCKET>" \
  --output=json \
  --query='{Objects: Versions[].{Key:Key,VersionId:VersionId}}')" \
 --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
 --region <REGION-FOR-AWS-S3-BUCKET> 
``` 

Empty the AWS S3 bucket before deletion, incase you have something uploaded there. 
```
aws s3 rb s3://<NAME-OF-THE-BUCKET> \
 --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
 --region <REGION-FOR-AWS-S3-BUCKET> 
```

Cleanup the AWS Cloudformation Template to reset your AWS Account.
``` 
aws cloudformation delete-stack \
 --stack-name <NAME-OF-THE-STACK> \
 --profile <OPTIONAL-Only-if-AWS-Profile-Exists> \
 --region <REGION-FOR-AWS-S3-BUCKET> 
```
