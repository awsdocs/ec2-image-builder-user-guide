# Document History for EC2 Image Builder User Guide<a name="doc-history"></a>

The following table describes important changes to the documentation by date\. For notification about updates to this documentation, you can subscribe to an RSS feed\.
+ **API version: 2020\-12\-17**
+ **Latest documentation update:** December 17, 2020

| Change | Description | Date | 
| --- |--- |--- |
| [Update cron expressions](https://docs.aws.amazon.com/imagebuilder/latest/userguide/cron-expressions.html) | Image Builder cron processing is updated to increase cron expression granularity to the minute, and use a standard cron scheduling engine\. Examples are updated with the new format\. | February 8, 2021 | 
| [Feature release: Container support](#doc-history) | Added support for creating Docker container images using Image Builder, with registration and storage of the resulting images on Amazon Elastic Container Registry \(Amazon ECR\)\. Content has been rearranged to reflect new functionality and accomodate future growth\. | December 17, 2020 | 
| [Restructured cron documentation](https://docs.aws.amazon.com/imagebuilder/latest/userguide/cron-expressions.html) | This page now highlights more information about how cron works with Image Builder pipeline builds, and includes details about UTC time\. Wildcards that are not allowed for specific fields have been removed\. Examples now include expression samples for both console and CLI\. | November 13, 2020 | 
| [Console version 2\.0: updated pipeline editing](#doc-history) | Content changes in getting started tutorial and manage image pipelines pages to incorporate new console features and flow\. | November 13, 2020 | 
| [Console version 2\.0: updated create pipeline tutorial](https://docs.aws.amazon.com/imagebuilder/latest/userguide/start-build-image-pipeline.html) | Content changes in the create pipeline tutorial to incorporate new console features and flow\. | November 13, 2020 | 
| [New STIG versions](https://docs.aws.amazon.com/imagebuilder/latest/userguide/image-builder-stig.html) | STIG versions and applied STIGS have been updated\. Note \- list format changed to show STIGs that are applied by default\. | October 15, 2020 | 
| [Support for looping constructs in the EC2 Image Builder AWSTOE application](image-builder-looping-constructs.md) | Create looping constructs to define a repeated sequence of instructions in the AWSTOE application\. | July 29, 2020 | 
| [Support for local development of EC2 Image Builder components](image-builder-component-manager.md) | Develop and test Image Builder components locally with the AWSTOE application\. | July 28, 2020 | 
| [Encrypted AMIs](#doc-history) | EC2 Image Builder adds support for encrypted AMI distribution\. | July 1, 2020 | 
| [AutoScaling deprecation](#doc-history) | Deprecation of the use of AutoScaling to launch instances\.  | June 15, 2020 | 
| [Support for connectivity through AWS PrivateLink](https://docs.aws.amazon.com/imagebuilder/latest/userguide/vpc-interface-endpoints.html) | You can establish a private connection between your VPC and EC2 Image Builder by creating an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Image Builder APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with Image Builder APIs\. Traffic between your VPC and Image Builder does not leave the Amazon network\. | June 10, 2020 | 
| [New STIG versions](https://docs.aws.amazon.com/imagebuilder/latest/userguide/image-builder-stig.html) | STIG versions and applied STIGS have been updated\. | January 23, 2020 | 
| [Troubleshooting](#doc-history) | Added general troubleshooting scenarios\. | January 22, 2020 | 
| [STIG Components](https://docs.aws.amazon.com/imagebuilder/latest/userguide/image-builder-stig.html) | You can create STIG\-compliant images with Image Builder STIG components\. | January 22, 2020 | 