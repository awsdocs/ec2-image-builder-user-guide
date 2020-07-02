# What is EC2 Image Builder?<a name="what-is-image-builder"></a>

EC2 Image Builder is a fully managed AWS service that makes it easier to automate the creation, management, and deployment of customized, secure, and up\-to\-date “golden” server images that are pre\-installed and pre\-configured with software and settings to meet specific IT standards\. 

You can use the AWS Management Console, AWS CLI, or APIs to create “golden” images in your AWS account\. When you use the AWS Management Console, the Image Builder wizard guides you through steps to:
+ Provide starting artifacts
+ Add and remove software
+ Customize settings and scripts
+ Run selected tests
+ Distribute images to AWS Regions

The images you build are created in your account and you can configure them for operating system patches on an ongoing basis\. 

For troubleshooting and debugging your image deployment, you can configure build logs to be added to your Amazon Simple Storage Service \(Amazon S3\) bucket\. You can also configure the instance\-building application to send logs to CloudWatch\. To receive notifications of image build status, and associate an Amazon Elastic Compute Cloud \(Amazon EC2\) key pair with your instance to perform manual debugging and inspection, you can configure an SNS topic\.

Along with a final image, Image Builder creates an image recipe, which is a combination of the source image and components for a build\. You can use the image recipe with existing source code version control systems and continuous integration/continuous deployment pipelines for repeatable automation\. 

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

Image Builder allows you to test your images before deployment with both AWS\-provided and customized tests\. AWS will distribute your image only if all of the configured tests have succeeded\.

**Raise the security bar for deployments**

Image Builder allows you to create images that remove unnecessary exposure to component security vulnerabilities\. You can apply AWS security settings to create secure, out\-of\-the\-box images that meet industry and internal security criteria\. Image Builder also provides collections of settings for companies in regulated industries\. You can use these settings to help you quickly and easily build compliant images for STIG standards\. For a complete list of STIG components available through Image Builder, see [EC2 Image Builder STIG Components](image-builder-stig.md)\.

**Centralized enforcement and lineage tracking**

Using built\-in integrations with AWS Organizations, Image Builder enables you to enforce policies that restrict accounts to run instances only from approved AMIs\.

**Simplified sharing of images across AWS accounts**

EC2 Image Builder integrates with AWS Resource Access Manager \(AWS RAM\) to allow you to share certain resources with any AWS account or through AWS Organizations\. EC2 Image Builder resources that can be shared are:
+ Components
+ Images
+ Image recipes

For more information, see [Resource Sharing in EC2 Image Builder](image-builder-resource-sharing.md)\.

## Supported operating systems<a name="image-builder-os"></a>

Image Builder supports the following operating systems:
+ Amazon Linux 2
+ Windows Server 2019/2016/2012 R2
+ Windows Server version 1909
+ Red Hat Enterprise Linux \(RHEL\) 8 and 7
+ CentOS 8 and 7 \(CentOS 8 is not available as a public AMI from the AWS Marketplace\. You can bring your own CentOS 8 AMI using VMIE\)
+ Ubuntu 18 and 16
+ SUSE Linux Enterprise Server \(SUSE\) 15

## Supported image formats<a name="image-builder-image-formats"></a>

You can choose an existing AMI as a starting point to build your images\.

## Concepts<a name="image-builder-concepts"></a>

The following terminology and concepts are central to your understanding and use of EC2 Image Builder\.

**AMI**  
An Amazon Machine Image \(AMI\) is the basic unit of deployment in Amazon EC2\. An AMI is a pre\-configured VM image that contains the OS and preinstalled software to deploy EC2 instances\. For more information, see [Amazon Machine Images \(AMI\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)\. 

**Image pipeline**  
An image pipeline is the automation configuration for building secure OS images on AWS\. The Image Builder image pipeline is associated with an image recipe that defines the build, validation, and test phases for an image build lifecycle\. An image pipeline can be associated with an infrastructure configuration that defines where your image is built\. You can define attributes, such as instance type, subnets, security groups, logging, and other infrastructure\-related configurations\. You can also associate your image pipeline with a distribution configuration to define how you would like to deploy your image\. 

**Image recipe**  
An Image Builder image recipe is a document that defines the source image and the components to be applied to the source image to produce the desired configuration for the output image\. You can use an image recipe to duplicate builds\. Image Builder image recipes can be shared, branched, and edited using the console wizard, the AWS CLI, or the API\. You can use image recipes with your version control software to maintain shareable versioned image recipes\.

**Source image**  
The source image is the selected image and OS used in your image recipe document along with the components\. The source image and the component definitions combined produce the desired configuration for the output image\. 

**Components**  
A component defines the sequence of steps required to either mutate an instance prior to image creation \(a **build component**\), or to test an instance that was launched from the created image \(a **test component**\)\. A component is a declarative, plain\-text YAML document that describes a series of steps to perform\. Components are executed on the instance using a component management application\. The component management application parses the documents and executes the desired steps\. After they are created, one or more components are grouped together using an image recipe to define the execution plan for building and testing a virtual machine image\. You can use public components that are owned and managed by AWS, or you can create your own\. For more details about components, see [EC2 Image Builder Component Manager](image-builder-component-manager.md)\.

**Document**  
A declarative document that uses the YAML format to list the execution steps for build, validation, and test of an AMI on an instance\. The document is input to a configuration management application, which runs locally on an Amazon EC2 instance to execute the document steps\. 

**Execution phases**  
EC2 Image Builder has two execution stages: **build** and **test**\. A component has three possible phases: **build**, **validate**, and **test**\. When an image pipeline starts, Image Builder is in the build stage\. During this stage, an instance is launched and all of the components defined in the image recipe are downloaded to the instance\. The components are executed sequentially, in the order specified by the image recipe\. For each component, the component management application parses the document and executes the build and validate phases\. If successful, the instance is turned off, an image is created, and Image Builder continues to the test stage\. During the test stage, an instance is launched from the previously created image and the components are downloaded to the instance\. The component management application parses the documents and executes the test phase\. The build stage in Image Builder maps to the build and validate phases in your components, while the test stage maps to the test phase in your components\. 

## Pricing<a name="image-builder-pricing"></a>

There is no cost to use EC2 Image Builder\. There may be costs associated with launching an Amazon EC2 instance and storing logs on Amazon S3, for validating images with Amazon Inspector, and for AMI storage for Amazon EBS Snapshots, depending on the configuration of your image\. If you enable Systems Manager Advanced Tier and run EC2 instances with on\-premises activation, you may be charged for resources through Systems Manager\. 

## Related AWS services<a name="image-builder-related-services"></a>

EC2 Image Builder uses other AWS services to build images\. Depending on your Image Builder image recipe configuration, the following services may be used\.

**AWS License Manager**  
AWS License Manager allows you to create and apply license configurations from an account license configuration store\. For each AMI, you can use Image Builder to attach to a preexisting license configuration that your AWS account has access to as part of the Image Builder workflow\. License configurations can be applied only to AMIs\. Image Builder can use only preexisting license configurations and cannot directly create or modify license configurations\. License Manager settings will not replicate across AWS Regions that must be enabled in your account, for example, between the `ap-east-1` \(HKG\) and the `me-south-1` \(BAH\) Regions\. 

**AWS Systems Manager \(SSM\) Automation**  
A Systems Manager automation document defines the actions that Systems Manager performs on your managed instances and AWS resources\. SSM documents use JSON or YAML and include steps and parameters that you specify\. The steps you specify run in sequential order\. Automation documents are Systems Manager documents of type Automation, as opposed to Command and Policy documents\. For more information, see [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html)\.

**Amazon CloudWatch Logs**  
You can use Amazon CloudWatch Logs to monitor, store, and access your log files from Amazon EC2 instances, AWS CloudTrail, Amazon Route 53, and other sources\.