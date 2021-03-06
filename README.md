
## Issue
It seems when using the package command on and AWS::CloudFormation::Stack generates s3 short hand rather than a url. 

```aws cloudformation package --template-file master.yaml --s3-bucket my bucket```

Generates
s3://lambda-build-store/aws-deploy/f94558b8c73c07eacc3ec32cd7328113.template

Needs to generate
https: //s3.amazonaws.com/lambda-build-store/aws-deploy/f94558b8c73c07eacc3ec32cd7328113.template

### Test Cases

```aws cloudformation package --template-file master.yaml --s3-bucket lambda-build-store --s3-prefix AwsDeploy --output-template-file serverless-output.yaml```

```aws cloudformation deploy --template-file serverless-output.yaml --stack-name AwsDeploy --capabilities CAPABILITY_IAM```


CREATE_FAILED on 
"ResourceStatusReason": "TemplateURL must be an Amazon S3 URL.", 
"ResourceStatus": "CREATE_FAILED", 
"ResourceType": "AWS::CloudFormation::Stack", 
"Timestamp": "2017-01-13T17:22:56.757Z", 
"ResourceStatusReason": "TemplateURL must be an Amazon S3 URL.", 
"StackName": "AwsDeploy", 

Generated Stack
```AWSTemplateFormatVersion: '2010-09-09'
Description: Pipeline CFN
Resources:
  Security:
    Properties:
      Parameters:
        EnvironmentName:
          Ref: AWS::StackName
      TemplateURL: s3://lambda-build-store/aws-deploy/f94558b8c73c07eacc3ec32cd7328113.template
    Type: AWS::CloudFormation::Stack
Transform: AWS::Serverless-2016-10-31```