# Data Protection in EC2 Image Builder<a name="data-protection"></a>

EC2 Image Builder conforms to the AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/), which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN Partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud\. 

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\), so that each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3, or Amazon GuardDuty, which continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with Image Builder or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into Image Builder or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

All images created by Image Builder and the Amazon EC2 instances used to build them are located in your account\. Therefore, any of your content that is included in the images or instances remains outside of the Image Builder service\. The Image Builder service stores the component definitions that you create\. You can upload custom component definitions that are stored in the Image Builder service\. Custom components are encrypted with your KMS key or a KMS key owned by Image Builder\. 

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

## Encryption and Key Management in EC2 Image Builder<a name="image-builder-enrcyption"></a>

Image Builder encrypts data in transit and at rest by default\. Custom components defined in the service can be added to your image pipelines and shared with other customer accounts\. You are not required to share your components to build images\. 

Custom components are encrypted with your KMS key or a KMS key owned by Image Builder\. Image Builder does not store any of your logs in the service\. All logs are saved on your Amazon EC2 instance that is used to build the image, or in your SSM automation logs\. 

You can manage your keys through AWS KMS\. You cannot manage the Image Builder AWS KMS key owned by Image Builder\. 

For more information about managing your AWS KMS keys with AWS Key Management Service, see [Getting Started](https://docs.aws.amazon.com/kms/latest/developerguide/getting-started.html) in the AWS Key Management Service Developer Guide\.

## Internetwork Traffic Privacy in EC2 Image Builder<a name="image-builder-internetwork"></a>

Connections are secured between Image Builder and on\-premises locations, between AZs within an AWS Region, and between AWS Regions through HTTPS\. There are no direct connections between accounts\.