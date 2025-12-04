
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

![architecture](docs/architecture.png)

## Security Prerequisites

### AWS CloudTrail
This application requires AWS CloudTrail to be enabled for comprehensive security logging and audit trails. CloudTrail should be configured at the account or organization level:

1. Create a CloudTrail trail with management events enabled
2. Configure S3 bucket for CloudTrail log storage with appropriate retention
3. Optionally enable CloudTrail data events for S3 and Lambda for detailed activity logging
4. Consider using CloudTrail Lake for long-term retention and SQL-based querying

For setup instructions, see: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html

## Logging Architecture

This stack implements comprehensive logging aligned with AWS Well-Architected Framework SEC04-BP01:

- **VPC Flow Logs**: Network traffic monitoring with 1-year retention in CloudWatch Logs
- **API Gateway Access Logs**: Detailed API request logging with 1-year retention
- **Lambda Function Logs**: Application logs with 1-year retention
- **DynamoDB Point-in-Time Recovery**: Continuous backups for data protection and audit trails
- **Centralized Log Archive**: S3 bucket with lifecycle policies for 7-year log retention

### Querying Logs

- **CloudWatch Logs Insights**: Query VPC Flow Logs, API Gateway logs, and Lambda logs
- **Amazon Athena**: Query archived logs in S3 for long-term analysis

## Setup

The `cdk.json` file tells the CDK Toolkit how to execute your app.

This project is set up like a standard Python project.  The initialization
process also creates a virtualenv within this project, stored under the `.venv`
directory.  To create the virtualenv it assumes that there is a `python3`
(or `python` for Windows) executable in your path with access to the `venv`
package. If for any reason the automatic creation of the virtualenv fails,
you can create the virtualenv manually.

To manually create a virtualenv on MacOS and Linux:

```
$ python3 -m venv .venv
```

After the init process completes and the virtualenv is created, you can use the following
step to activate your virtualenv.

```
$ source .venv/bin/activate
```

If you are a Windows platform, you would activate the virtualenv like this:

```
% .venv\Scripts\activate.bat
```

Once the virtualenv is activated, you can install the required dependencies.

```
$ pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
$ cdk synth
```

To add additional dependencies, for example other CDK libraries, just add
them to your `setup.py` file and rerun the `pip install -r requirements.txt`
command.

## Deploy
At this point you can deploy the stack. 

Using the default profile

```
$ cdk deploy
```

With specific profile

```
$ cdk deploy --profile test
```

## After Deploy
Navigate to AWS API Gateway console and test the API with below sample data 
```json
{
    "year":"2023", 
    "title":"kkkg",
    "id":"12"
}
```

You should get below response 

```json
{"message": "Successfully inserted data!"}
```

## Cleanup 
Run below script to delete AWS resources created by this sample stack.
```
cdk destroy
```

## Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

Enjoy!
