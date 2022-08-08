# Image Builder service integrations<a name="ibhow-integrations"></a>

EC2 Image Builder integrates with the following AWS services to provide detailed event metrics, logging, and monitoring\. This information helps you track your activity, troubleshoot image build issues, and create automations based on event notifications\.
+ **Amazon CloudWatch Logs** – Monitor, store, and access your Image Builder log files\. Optionally, you can save your logs to an S3 bucket\. For more information about CloudWatch Logs, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*\.
+ **Amazon EventBridge** – Connect to a stream of real\-time event data from Image Builder activities in your account\. For more information about EventBridge, see [What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) in the *Amazon EventBridge User Guide*\.
+ **AWS CloudTrail** – Monitor Image Builder events that are sent to CloudTrail\. For more information about CloudTrail, see [What Is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) in the *AWS CloudTrail User Guide*\.
+ **Amazon Simple Notification Service \(Amazon SNS\)** – If configured, publish detailed messages about your image status to an SNS topic that you subscribe to\. For more information about Amazon SNS, see [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Topics**
+ [Amazon CloudWatch Logs](#integ-cwlogs)
+ [Amazon EventBridge](#integ-eventbridge)
+ [AWS CloudTrail](#integ-cloudtrail)
+ [Amazon Simple Notification Service](#integ-sns)

## Amazon CloudWatch Logs<a name="integ-cwlogs"></a>

CloudWatch Logs support is enabled by default\. Logs are retained on the instance during the build process, and streamed to CloudWatch Logs\. The instance logs are removed from the instance before image creation\.

Build logs are streamed to following the Image Builder CloudWatch Logs group and stream:
+ LogGroup: `"/aws/imagebuilder/<ImageName>`
+ LogStream: `<ImageVersion>/<ImageBuildVersion>["x.x.x/x"]`

You can opt out of CloudWatch Logs streaming by removing the following permissions associated with the instance profile\.

```
"Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "logs:CreateLogStream",
            "logs:CreateLogGroup",
            "logs:PutLogEvents"
        ],
        "Resource": "arn:aws:logs:*:*:log-group:/aws/imagebuilder/*"
    }
]
```

For advanced troubleshooting, you can run predefined commands and scripts using [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\. For more information, see [Troubleshoot EC2 Image Builder](troubleshooting.md)\.

## Amazon EventBridge<a name="integ-eventbridge"></a>

Amazon EventBridge is a serverless event bus service that you can use to connect your Image Builder application with related data from other AWS services\. In EventBridge, a rule matches incoming events and sends them to targets for processing\. A single rule can send an event to multiple targets, which then run in parallel\.

EventBridge enables you to automate your AWS services and respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to EventBridge in near real time\. You can set up rules that react to incoming events to initiate actions; for example, sending an event to a Lambda function when the status of an EC2 instance changes from pending to running\. These are called *patterns*\. To create a rule based on an event pattern, see [Creating Amazon EventBridge rules that react to events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule.html) in the *Amazon EventBridge User Guide*\.

Actions that can be automatically initiated include the following:
+ Invoke an AWS Lambda function
+ Invoke Amazon EC2 Run Command
+ Relay the event to Amazon Kinesis Data Streams
+ Activate an AWS Step Functions state machine
+ Notify an Amazon SNS topic or an AWS SMS queue

You can also set up scheduling rules for the default event bus to perform an action at regular intervals, such as running an Image Builder pipeline to refresh an image on a quarterly basis\. There are two types of schedule expressions:
+ **cron expressions** – The following example of a cron expression schedules a task to run every day at noon UTC\+0:

  `cron(0 12 * * ? *)`

  For more information about using cron expressions with EventBridge, see [Cron expressions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html#eb-cron-expressions) in the *Amazon EventBridge User Guide*\.
+ **rate expressions** – The following example of a rate expression schedules a task to run every 12 hours:

  `rate(12 hour)`

  For more information about using rate expressions with EventBridge, see [Rate expressions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html#eb-rate-expressions) in the *Amazon EventBridge User Guide*\.

For more information about how EventBridge integrates with Image Builder image pipelines, see [Use EventBridge rules with Image Builder pipelines](ev-rules-for-pipeline.md)\.

## AWS CloudTrail<a name="integ-cloudtrail"></a>

This service supports AWS CloudTrail, which is a service that records AWS calls for your AWS account and delivers log files to an Amazon S3 bucket\. By using information collected by CloudTrail, you can determine what requests were successfully made to AWS services, who made the request, when it was made, and so on\. For more information about CloudTrail integration with Image Builder, see [Logging EC2 Image Builder API calls using AWS CloudTrail](log-cloudtrail.md)\.

To learn more about CloudTrail, including how to turn it on and find your log files, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon Simple Notification Service<a name="integ-sns"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a managed service that provides asynchronous message delivery from publishers to subscribers \(also known as producers and consumers\)\. You can specify an SNS topic in your infrastructure configuration\. When you create an image or run a pipeline, Image Builder can publish detailed messages about your image status to this topic\. When the image status reaches one of the following states, Image Builder publishes a message:
+ `AVAILABLE`
+ `FAILED`

For an example SNS message from Image Builder, see [SNS message format](#integ-sns-message)\. If you want to create a new SNS topic, see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon Simple Notification Service Developer Guide*\.

### Encrypted SNS Topics<a name="integ-sns-encrypted"></a>

If your SNS topic is encrypted, you must grant permission in the KMS key policy for the Image Builder service role to perform the following actions:
+ `kms:Decrypt`
+ `kms:GenerateDataKey`

**Note**  
If your SNS topic is encrypted, the key that encrypts this topic must reside in the account where the Image Builder service runs\. Image Builder can't send notifications to SNS topics that are encrypted with keys from other accounts\.

**Example KMS key policy addition**  
The following example shows the additional section that you add to the KMS key policy\. Use the Amazon Resource Name \(ARN\) for the IAM service\-linked role that Image Builder created under your account when you first created an Image Builder image\. To learn more about the Image Builder service\-linked role, see [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)\.

```
{
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::123456789012:role/aws-service-role/imagebuilder.amazonaws.com/AWSServiceRoleForImageBuilder"
    },
    "Action": [
      "kms:GenerateDataKey*",
      "kms:Decrypt"
    ],
    "Resource": "*"
  }]
}
```

You can use one of the following methods to get the ARN\.

------
#### [ AWS Management Console ]

To get the ARN for the service\-linked role that Image Builder created under your account from the AWS Management Console, follow these steps:

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. Search for `ImageBuilder`, and choose the following **Role name** from the results: `AWSServiceRoleForImageBuilder`\. This displays the role detail page\.

1. To copy the ARN to your clipboard, choose the icon next to the ARN name\.

------
#### [ AWS CLI ]

To get the ARN for the service\-linked role that Image Builder created under your account from the AWS CLI, use the IAM [get\-role](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/get-role.html) command, as follows\.

```
aws iam get-role --role-name AWSServiceRoleForImageBuilder
```

**Partial sample output:**

```
{
    "Role": {
        "Path": "/aws-service-role/imagebuilder.amazonaws.com/",
        "RoleName": "AWSServiceRoleForImageBuilder",
        ...
        "Arn": "arn:aws:iam::123456789012:role/aws-service-role/imagebuilder.amazonaws.com/AWSServiceRoleForImageBuilder",
        ...
}
```

------

### SNS message format<a name="integ-sns-message"></a>

After Image Builder publishes a message to your Amazon SNS topic, other services that subscribe to the topic can filter on the message format and determine if it meets criteria for further action\. For example, a success message might initiate a task to update an AWS Systems Manager parameter store, or to launch an external compliance testing workflow for the output AMI\.

The following example shows the JSON payload for a typical message that Image Builder publishes when a pipeline build runs to completion, and creates a Linux image\.

```
{
  "versionlessArn": "arn:aws:imagebuilder:us-west-1:123456789012:image/example-linux-image",
  "semver": 1237940039285380274899124227,
  "arn": "arn:aws:imagebuilder:us-west-1:123456789012:image/example-linux-image/1.0.0/3",
  "name": "example-linux-image",
  "version": "1.0.0",
  "type": "AMI",
  "buildVersion": 3,
  "state": {
    "status": "AVAILABLE"
  },
  "platform": "Linux",
  "imageRecipe": {
    "arn": "arn:aws:imagebuilder:us-west-1:123456789012:image-recipe/example-linux-image/1.0.0",
    "name": "amjule-barebones-linux",
    "version": "1.0.0",
    "components": [
      {
        "componentArn": "arn:aws:imagebuilder:us-west-1:123456789012:component/update-linux/1.0.2/1"
      }
    ],
    "platform": "Linux",
    "parentImage": "arn:aws:imagebuilder:us-west-1:987654321098:image/amazon-linux-2-x86/2022.6.14/1",
    "blockDeviceMappings": [
      {
        "deviceName": "/dev/xvda",
        "ebs": {
          "encrypted": false,
          "deleteOnTermination": true,
          "volumeSize": 8,
          "volumeType": "gp2"
        }
      }
    ],
    "dateCreated": "Feb 24, 2021 12:31:54 AM",
    "tags": {
      "internalId": "1a234567-8901-2345-bcd6-ef7890123456",
      "resourceArn": "arn:aws:imagebuilder:us-west-1:123456789012:image-recipe/example-linux-image/1.0.0"
    },
    "workingDirectory": "/tmp",
    "accountId": "462045008730"
  },
  "sourcePipelineArn": "arn:aws:imagebuilder:us-west-1:123456789012:image-pipeline/example-linux-pipeline",
  "infrastructureConfiguration": {
    "arn": "arn:aws:imagebuilder:us-west-1:123456789012:infrastructure-configuration/example-linux-infra-config-uswest1",
    "name": "example-linux-infra-config-uswest1",
    "instanceProfileName": "example-linux-ib-baseline-admin",
    "tags": {
      "internalId": "234abc56-d789-0123-a4e5-6b789d012c34",
      "resourceArn": "arn:aws:imagebuilder:us-west-1:123456789012:infrastructure-configuration/example-linux-infra-config-uswest1"
    },
    "logging": {
      "s3Logs": {
        "s3BucketName": "12345-example-linux-testbucket-uswest1"
      }
    },
    "keyPair": "example-linux-key-pair-uswest1",
    "terminateInstanceOnFailure": true,
    "snsTopicArn": "arn:aws:sns:us-west-1:123456789012:example-linux-ibnotices-uswest1",
    "dateCreated": "Feb 24, 2021 12:31:55 AM",
    "accountId": "123456789012"
  },
  "imageTestsConfigurationDocument": {
    "imageTestsEnabled": true,
    "timeoutMinutes": 720
  },
  "distributionConfiguration": {
    "arn": "arn:aws:imagebuilder:us-west-1:123456789012:distribution-configuration/example-linux-distribution",
    "name": "example-linux-distribution",
    "dateCreated": "Feb 24, 2021 12:31:56 AM",
    "distributions": [
      {
        "region": "us-west-1",
        "amiDistributionConfiguration": {}
      }
    ],
    "tags": {
      "internalId": "345abc67-8910-12d3-4ef5-67a8b90c12de",
      "resourceArn": "arn:aws:imagebuilder:us-west-1:123456789012:distribution-configuration/example-linux-distribution"
    },
    "accountId": "123456789012"
  },
  "dateCreated": "Jul 28, 2022 1:13:45 AM",
  "outputResources": {
    "amis": [
      {
        "region": "us-west-1",
        "image": "ami-01a23bc4def5a6789",
        "name": "example-linux-image 2022-07-28T01-14-17.416Z",
        "accountId": "123456789012"
      }
    ]
  },
  "buildExecutionId": "ab0cd12e-34fa-5678-b901-2c3456d789e0",
  "testExecutionId": "6a7b8901-cdef-234a-56b7-8cd89ef01234",
  "distributionJobId": "1f234567-8abc-9d0e-1234-fa56b7c890de",
  "integrationJobId": "432109b8-afe7-6dc5-4321-0ba98f7654e3",
  "accountId": "123456789012",
  "osVersion": "Amazon Linux 2",
  "enhancedImageMetadataEnabled": true,
  "buildType": "USER_INITIATED",
  "tags": {
    "internalId": "901e234f-a567-89bc-0123-d4e567f89a01",
    "resourceArn": "arn:aws:imagebuilder:us-west-1:123456789012:image/example-linux-image/1.0.0/3"
  }
}
```

The following example shows the JSON payload for a typical message that Image Builder publishes for a pipeline build failure for a Linux image\.

```
{
  "versionlessArn": "arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image",
  "semver": 1237940039285380274899124231,
  "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/1.0.0/7",
  "name": "My Example Image",
  "version": "1.0.0",
  "type": "AMI",
  "buildVersion": 7,
  "state": {
    "status": "FAILED",
    "reason": "Image Failure reason."
  },
  "platform": "Linux",
  "imageRecipe": {
    "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image/1.0.0",
    "name": "My Example Image",
    "version": "1.0.0",
    "description": "Testing Image recipe",
    "components": [
      {
        "componentArn": "arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-image-component/1.0.0/1"
      }
    ],
    "platform": "Linux",
    "parentImage": "ami-0cd12345db678d90f",
    "dateCreated": "Jun 21, 2022 11:36:14 PM",
    "tags": {
      "internalId": "1a234567-8901-2345-bcd6-ef7890123456",
      "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image/1.0.0"
    },
    "accountId": "123456789012"
  },
  "sourcePipelineArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-image-pipeline",
  "infrastructureConfiguration": {
    "arn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infra-config",
    "name": "SNS topic Infra config",
    "description": "An example that will retain instances of failed builds",
    "instanceTypes": [
      "t2.micro"
    ],
    "instanceProfileName": "EC2InstanceProfileForImageBuilder",
    "tags": {
      "internalId": "234abc56-d789-0123-a4e5-6b789d012c34",
      "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infra-config"
    },
    "terminateInstanceOnFailure": true,
    "snsTopicArn": "arn:aws:sns:us-west-2:123456789012:example-pipeline-notification-topic",
    "dateCreated": "Jul 5, 2022 7:31:53 PM",
    "accountId": "123456789012"
  },
  "imageTestsConfigurationDocument": {
    "imageTestsEnabled": true,
    "timeoutMinutes": 720
  },
  "distributionConfiguration": {
    "arn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-config",
    "name": "New distribution config",
    "dateCreated": "Dec 3, 2021 9:24:22 PM",
    "distributions": [
      {
        "region": "us-west-2",
        "amiDistributionConfiguration": {},
        "fastLaunchConfigurations": [
          {
            "enabled": true,
            "snapshotConfiguration": {
              "targetResourceCount": 2
            },
            "maxParallelLaunches": 2,
            "launchTemplate": {
              "launchTemplateId": "lt-01234567890"
            },
            "accountId": "123456789012"
          }
        ]
      }
    ],
    "tags": {
      "internalId": "1fecd23a-4f56-7f89-01e2-345678abbe90",
      "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-config"
    },
    "accountId": "123456789012"
  },
  "dateCreated": "Jul 5, 2022 7:40:15 PM",
  "outputResources": {
    "amis": []
  },
  "accountId": "123456789012",
  "enhancedImageMetadataEnabled": true,
  "buildType": "SCHEDULED",
  "tags": {
    "internalId": "456c78b9-0e12-3f45-afb6-7e89b0f1a23b",
    "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/1.0.0/7"
  }
}
```