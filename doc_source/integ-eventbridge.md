# Amazon EventBridge integration in Image Builder<a name="integ-eventbridge"></a>

Amazon EventBridge is a serverless event bus service that you can use to connect your Image Builder application with related data from other AWS services\. In EventBridge, a rule matches incoming events and sends them to targets for processing\. A single rule can send an event to multiple targets, and these events then run in parallel\.

With EventBridge, you can automate your AWS services and respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to EventBridge in near real time\. You can set up rules that react to incoming events to initiate actions for example, sending an event to a Lambda function when the status of an EC2 instance changes from pending to running\. These are called *patterns*\. To create a rule based on an event pattern, see [Creating Amazon EventBridge rules that react to events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule.html) in the *Amazon EventBridge User Guide*\.

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