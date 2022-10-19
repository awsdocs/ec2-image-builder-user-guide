# What is EC2 Image Builder?<a name="what-is-image-builder"></a>

EC2 Image Builder is a fully managed AWS service that helps you to automate the creation, management, and deployment of customized, secure, and up\-to\-date server images\. You can use the AWS Management Console, AWS Command Line Interface, or APIs to create custom images in your AWS account\.

You own the customized images that Image Builder creates in your account\. You can configure pipelines to automate updates and system patching for the images that you own\. You can also run a stand\-alone command to create an image with the configuration resources that you've defined\.

The Image Builder pipeline wizard can guide you through the steps to create a custom image, as follows:

1. Choose a base image for your customizations\.

1. Add to or remove software from your base image\.

1. Customize settings and scripts with build components\.

1. Run selected tests or create custom test components\.

1. Distribute AMIs to AWS Regions and AWS accounts\.

1. If your Image Builder pipeline creates a custom Amazon Machine Image \(AMI\) for distribution, you can authorize other AWS accounts, organizations, and OUs to launch it from your account\. Your account is billed for charges that are associated with the AMI\.

Image Builder integrates with the following AWS services to provide detailed event metrics, logging, and monitoring\. This information helps you track your activity, troubleshoot image build issues, and create automations based on event notifications\.
+ **Amazon CloudWatch Logs** – Monitor, store, and access your Image Builder log files\. Optionally, you can save your logs to an S3 bucket\. For more information about CloudWatch Logs, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*\.
+ **Amazon EventBridge** – Connect to a stream of real\-time event data from Image Builder activities in your account\. For more information about EventBridge, see [What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) in the *Amazon EventBridge User Guide*\.
+ **AWS CloudTrail** – Monitor Image Builder events that are sent to CloudTrail\. For more information about CloudTrail, see [What Is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) in the *AWS CloudTrail User Guide*\.
+ **Amazon Simple Notification Service \(Amazon SNS\)** – If configured, publish detailed messages about your image status to an SNS topic that you subscribe to\. For more information about Amazon SNS, see [What is Amazon SNS?](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Topics**
+ [Features of EC2 Image Builder](#image-builder-features)
+ [Supported operating systems](#image-builder-os)
+ [Supported image formats](#image-builder-image-formats)
+ [Concepts](#image-builder-concepts)
+ [Pricing](#image-builder-pricing)
+ [Related AWS services](#image-builder-related-services)

## Features of EC2 Image Builder<a name="image-builder-features"></a>

EC2 Image Builder provides the following features:

**Increase productivity and reduce operations for building compliant and up\-to\-date images**

Image Builder reduces the amount of work involved in creating and managing images at scale by automating your build pipelines\. You can automate your builds by providing your build execution schedule preference\. Automation reduces the operational cost of maintaining your software with the latest operating system patches\.

**Increase service uptime**

Image Builder provides access to test components that you can use to test your images before deployment\. You can also create custom test components with AWS Task Orchestrator and Executor \(AWSTOE\), and use those\. Image Builder distributes your image only if all of the configured tests have succeeded\.

**Raise the security bar for deployments**

Image Builder allows you to create images that remove unnecessary exposure to component security vulnerabilities\. You can apply AWS security settings to create secure, out\-of\-the\-box images that meet industry and internal security criteria\. Image Builder also provides collections of settings for companies in regulated industries\. You can use these settings to help you quickly and easily build compliant images for STIG standards\. For a complete list of STIG components available through Image Builder, see [EC2 Image Builder STIG components](toe-stig.md)\.

**Centralized enforcement and lineage tracking**

Using built\-in integrations with AWS Organizations, Image Builder enables you to enforce policies that restrict accounts to run instances only from approved AMIs\.

**Simplified sharing of resources across AWS accounts**

EC2 Image Builder integrates with AWS Resource Access Manager \(AWS RAM\) to allow you to share certain resources with any AWS account or through AWS Organizations\. EC2 Image Builder resources that can be shared are:
+ Components
+ Images
+ Image recipes
+ Container recipes

For more information, see [Share EC2 Image Builder resources](manage-shared-resources.md)\.

## Supported operating systems<a name="image-builder-os"></a>

Image Builder supports the following operating systems:
+ Amazon Linux 2
+ Windows Server 2022/2019/2016/2012 R2
+ Windows Server version 2004, and 20H2
+ Red Hat Enterprise Linux \(RHEL\) 8 and 7
+ CentOS 8 and 7
+ Ubuntu 20, 18, and 16
+ SUSE Linux Enterprise Server \(SUSE\) 15

## Supported image formats<a name="image-builder-image-formats"></a>

For your custom AMI images, you can choose an existing AMI as a starting point\. For Docker container images, you can choose from public images hosted on DockerHub, existing container images in Amazon ECR, or Amazon\-managed container images\.

## Concepts<a name="image-builder-concepts"></a>

The following terms and concepts are central to your understanding and use of EC2 Image Builder\.

**AMI**  
An Amazon Machine Image \(AMI\) is the basic unit of deployment in Amazon EC2, and is one of the types of images you can create with Image Builder\. An AMI is a pre\-configured virtual machine image that contains the operating system \(OS\) and preinstalled software to deploy EC2 instances\. For more information, see [Amazon Machine Images \(AMI\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)\.

**Image pipeline**  
An image pipeline provides an automation framework for building secure AMIs and container images on AWS\. The Image Builder image pipeline is associated with an image recipe or container recipe that defines the build, validation, and test phases for an image build lifecycle\.

An image pipeline can be associated with an infrastructure configuration that defines where your image is built\. You can define attributes, such as instance type, subnets, security groups, logging, and other infrastructure\-related configurations\. You can also associate your image pipeline with a distribution configuration to define how you would like to deploy your image\. 

**Managed image**  
A managed image is a resource in Image Builder that consists of an AMI or container image, plus metadata, such as version and platform\. The managed image is used by Image Builder pipelines to determine which base image to use for the build\. In this guide, managed images are sometimes referred to as "images," however, an image is not the same as an AMI\.

**Image recipe**  
An Image Builder image recipe is a document that defines the base image and the components that are applied to the base image to produce the desired configuration for the output AMI image\. You can use an image recipe to duplicate builds\. Image Builder image recipes can be shared, branched, and edited using the console wizard, the AWS CLI, or the API\. You can use image recipes with your version control software to maintain shareable, versioned image recipes\.

**Container recipe**  
An Image Builder container recipe is a document that defines the base image and the components that are applied to the base image to produce the desired configuration for the output container image\. You can use a container recipe to duplicate builds\. You can share, branch, and edit Image Builder image recipes by using the console wizard, the AWS CLI, or the API\. You can use container recipes with your version control software to maintain shareable, versioned container recipes\.

**Base image**  
The base image is the selected image and operating system used in your image or container recipe document, along with the components\. The base image and the component definitions combined produce the desired configuration for the output image\.

**Components**  
A component defines the sequence of steps required to either customize an instance prior to image creation \(a **build component**\), or to test an instance that was launched from the created image \(a **test component**\)\.

A component is created from a declarative, plain\-text YAML or JSON document that describes the runtime configuration for building and validating, or testing an instance that is produced by your pipeline\. Components run on the instance using a component management application\. The component management application parses the documents and runs the desired steps\.

After they are created, one or more components are grouped together using an image recipe or container recipe to define the plan for building and testing a virtual machine or container image\. You can use public components that are owned and managed by AWS, or you can create your own\. For more information about components, see [AWS Task Orchestrator and Executor component manager](toe-component-manager.md)\.

**Component document**  
A declarative, plain\-text YAML or JSON document that describes configuration for a customization you can apply to your image\. The document is used to create a build or test component\.

**Runtime stages**  
EC2 Image Builder has two runtime stages: **build** and **test**\. Each runtime stage has one or more phases with configuration defined by the component document\.

**Configuration phases**  
The following list shows the phases that run during the **build** and **test** stages:*Build stage:*

Build phase  
An image pipeline begins with the build phase of the build stage when it runs\. The base image is downloaded, and configuration that is specified for the build phase of the component is applied to build and launch an instance\.

Validate phase  
After Image Builder launches the instance and applies all of the build phase customizations, the validation phase begins\. During this phase, Image Builder ensures that all of the customizations work as expected, based on the configuration that the component specifies for the validate phase\. If the instance validation succeeds, Image Builder stops the instance, creates an image, and then continues to the test stage\.*Test stage:*

Test phase  
During this phase, Image Builder launches an instance from the image that it created after the validation phase completed successfully\. Image Builder runs test components during this phase to verify that the instance is healthy and functions as expected\.

Container host test phase  
After Image Builder runs the test phase for all of the components that you selected in the container recipe, Image Builder runs this phase for container workflows\. The container host test phase can run additional tests that validate container management and custom runtime configurations\.

## Pricing<a name="image-builder-pricing"></a>

There is no cost to use EC2 Image Builder to create custom AMI or container images\. However, standard pricing applies for other services that are used in the process\. The following list includes the usage of some AWS services that can incur costs when you create, build, store, and distribute your custom AMI or container images, depending on your configuration\.
+ Launching an EC2 instance
+ Storing logs on Amazon S3
+ Validating images with Amazon Inspector
+ Storing Amazon EBS Snapshots for your AMIs
+ Storing container images in Amazon ECR
+ Pushing and pulling container images into and out of Amazon ECR
+ If Systems Manager Advanced Tier is turned on, and Amazon EC2 instances run with on\-premises activation, you might be charged for resources through Systems Manager

## Related AWS services<a name="image-builder-related-services"></a>

EC2 Image Builder uses other AWS services to build images\. Depending on your Image Builder image recipe or container recipe configuration, the following services might be used\.

**AWS License Manager**  
AWS License Manager allows you to create and apply license configurations from an account license configuration store\. For each AMI, you can use Image Builder to attach to a preexisting license configuration that your AWS account has access to as part of the Image Builder workflow\. License configurations can be applied only to AMIs\. Image Builder can use only preexisting license configurations and cannot directly create or modify license configurations\. License Manager settings will not replicate across AWS Regions that must be enabled in your account, for example, between the `ap-east-1` \(Asia Pacific: Hong Kong\) and the `me-south-1` \(Middle East: Bahrain\) Regions\. 

**AWS Organizations**  
AWS Organizations allows you to apply Service Control Policies \(SCP\) on accounts in your organization\. You can create, manage, enable, and disable individual policies\. Similar to all other AWS artifacts and services, Image Builder honors the policies defined in AWS Organizations\. AWS provides template SCPs for common scenarios, such as enforcing constraints on member accounts to launch instances with only approved AMIs\.

**Amazon Inspector**  
Image Builder uses Amazon Inspector as the default vulnerability scanning agent to establish security baselines for Amazon Linux 2, Windows Server 2012, and Windows Server 2016\. For more information, see [What is Amazon Inspector?](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html)

**AWS Systems Manager Automation**  
An AWS Systems Manager automation document defines the actions that AWS Systems Manager performs on your managed instances and AWS resources\. Systems Manager documents use JSON or YAML and include steps and parameters that you specify\. The steps you specify run in sequential order\. Automation documents are AWS Systems Manager documents of type Automation, as opposed to Command and Policy documents\. For more information, see [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html)\.

**AWS Resource Access Manager**  
AWS Resource Access Manager \(AWS RAM\) lets you share your resources with any AWS account or through AWS Organizations\. If you have multiple AWS accounts, you can create resources centrally and use AWS RAM to share those resources with other accounts\. EC2 Image Builder allows sharing for the following resources: components, images, and image recipes\. For more information about AWS RAM, see the [AWS Resource Access Manager User Guide](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html)\. For information about sharing Image Builder resources, see [Share EC2 Image Builder resources](manage-shared-resources.md)\.

**Amazon CloudWatch Logs**  
You can use Amazon CloudWatch Logs to monitor, store, and access your log files from EC2 instances, AWS CloudTrail, Amazon Route 53, and other sources\.

**Amazon Elastic Container Registry \(Amazon ECR\)**  
Amazon ECR is a managed AWS container image registry service that is secure, scalable, and reliable\. Container images you create with Image Builder are stored in Amazon ECR in your default Region, and in any Regions where you distribute the container image\. For more information about Amazon ECR, see the [Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)\.