# Integrate products and services in EC2 Image Builder<a name="integrate-products-services"></a>

EC2 Image Builder integrates with AWS Marketplace and other AWS services and applications to help you create robust, secure custom machine images\.

**Products**

Image Builder recipes can incorporate image products from AWS Marketplace and AWS Task Orchestrator and Executor \(AWSTOE\) components to provide specialized build and test functionality, as follows\.
+ **AWS Marketplace image products** – Use an image product from AWS Marketplace as the base image in your recipe to meet organizational standards, such as CIS Hardening\. When you create a recipe from the Image Builder console, you can choose from your existing subscriptions, or search for a specific product from AWS Marketplace\. When you create a recipe from the Image Builder API, CLI, or SDK, you can specify an image product Amazon Resource Name \(ARN\) to use as your base image\.
+ **AWSTOE components** – Components that you specify in your recipes can perform build and test actions, for example, to install software or perform compliance validation\. Some image products that you subscribe to from AWS Marketplace might include a companion component that you can use in your recipes\. The CIS Hardened images include a matching AWSTOE component that you can use in your recipe to enforce CIS Benchmarks Level 1 guidelines for your configuration\.

**Note**  
For more information about compliance\-related products, see [Compliance products for your Image Builder images](integ-compliance-products.md)\.

**Services**

Image Builder integrates with the following AWS services to provide detailed event metrics, logging, and monitoring\. This information helps you track your activity, troubleshoot image build issues, and create automations based on event notifications\.
+ **AWS CloudTrail** – Monitor Image Builder events that are sent to CloudTrail\. For more information about CloudTrail, see [What Is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) in the *AWS CloudTrail User Guide*\.
+ **Amazon CloudWatch Logs** – Monitor, store, and access your Image Builder log files\. Optionally, you can save your logs to an S3 bucket\. For more information about CloudWatch Logs, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*\.
+ **Amazon EventBridge** – Connect to a stream of real\-time event data from Image Builder activities in your account\. For more information about EventBridge, see [What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) in the *Amazon EventBridge User Guide*\.
+ **Amazon Inspector** – Discover vulnerabilities in your software and network settings with automatic scans for the EC2 instances that Image Builder launches when you build and test a new image\. Image Builder saves findings with the build version information for your output image resource so that you can investigate and remediate after the build completes\. For more information about scans and pricing, see [What Is Amazon Inspector?](https://docs.aws.amazon.com/inspector/latest/user/what-is-inspector.html) in the *Amazon Inspector User Guide*\.

  Amazon Inspector can also scan your ECR repositories if you configure enhanced scanning\. For more information, see [Scanning Amazon ECR container images](https://docs.aws.amazon.com/inspector/latest/user/enable-disable-scanning-ecr.html) in the *Amazon Inspector User Guide*\.
**Note**  
Amazon Inspector is a paid feature\.
+ **AWS Marketplace** – See a list of your current AWS Marketplace product subscriptions, and search for image products directly from Image Builder\. You can also use an image product that you’ve subscribed to as the base image for an Image Builder recipe\. For more information about managing AWS Marketplace subscriptions, see the [AWS Marketplace Buyer Guide](https://docs.aws.amazon.com/marketplace/latest/buyerguide)\.
+ **Amazon Simple Notification Service \(Amazon SNS\)** – If configured, publish detailed messages about your image status to an SNS topic that you subscribe to\. For more information about Amazon SNS, see [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Topics**
+ [AWS CloudTrail integration in Image Builder](integ-cloudtrail.md)
+ [Amazon CloudWatch Logs integration in Image Builder](integ-cwlogs.md)
+ [Amazon EventBridge integration in Image Builder](integ-eventbridge.md)
+ [Amazon Inspector integration in Image Builder](integ-inspector.md)
+ [AWS Marketplace integration in Image Builder](integ-marketplace.md)
+ [Amazon SNS integration in Image Builder](integ-sns.md)
+ [Compliance products for your Image Builder images](integ-compliance-products.md)