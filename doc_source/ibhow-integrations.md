# Image Builder service integrations<a name="ibhow-integrations"></a>

EC2 Image Builder integrates with the following AWS services to provide detailed event metrics, logging, and monitoring to help you track your activity and troubleshoot image build issues\.
+ **Amazon CloudWatch Logs** – Monitor, store, and access your Image Builder log files\. You can optionally save your logs to an S3 bucket\. For more information about CloudWatch Logs, see [What Is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*\.
+ **Amazon EventBridge** – Connect to a stream of real\-time event data from Image Builder activities in your account\. For more information about EventBridge, see [What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) in the *Amazon EventBridge User Guide*\.
+ **AWS CloudTrail** – Monitor Image Builder events that are sent to CloudTrail\. For more information about CloudTrail, see [What Is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) in the *AWS CloudTrail User Guide*\.

**Topics**
+ [Amazon CloudWatch Logs](#integ-cwlogs)
+ [Amazon EventBridge](#integ-eventbridge)
+ [AWS CloudTrail](#integ-cloudtrail)

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

This service supports AWS CloudTrail, which is a service that records AWS calls for your AWS account and delivers log files to an Amazon S3 bucket\. By using information collected by CloudTrail, you can determine what requests were successfully made to AWS services, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to turn it on and find your log files, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.