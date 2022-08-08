# Document history for EC2 Image Builder user guide<a name="doc-history"></a>

The following table describes important changes to the documentation by date\. For notification about updates to this documentation, you can subscribe to an RSS feed\.
+ **API version: 2022\-02\-21**

| Change | Description | Date | 
| --- |--- |--- |
| [New STIG Versions](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-stig.html) | Introduced Ubuntu support, updated STIG versions, and applied STIGS for 2022 second quarter release\. | July 20, 2022 | 
| [Document update: Navigation for Create YAML component document page](#doc-history) | Moved the Create YAML component document content to its own page, and updated other pages to reference it\. | June 7, 2022 | 
| [New STIG Versions](toe-stig.md) | Updated STIG versions and applied STIGS for 2022 first quarter release\. | April 25, 2022 | 
| [Added ExecuteDocument action module](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html) | Added documentation for the `ExecuteDocument` action module under `General execution`\. | March 28, 2022 | 
| [Feature release: Support for faster launching Windows AMI](#doc-history) | Added distribution configuration settings to support faster launching for Windows AMIs\. | February 21, 2022 | 
| [Maintenance release: Update AWSTOE binary thumbprint](#doc-history) | Updated binary thumbprint for AWSTOE signer certificate\. | February 18, 2022 | 
| [Feature release: Configure input for AWSTOE](#doc-history) | Added support for using a JSON configuration file as input for the AWSTOE run command\. | February 3, 2022 | 
| [New STIG Versions](toe-stig.md) | Updated STIG versions and applied STIGS for 2021 fourth quarter release\. Also added a section for new SCAP Compliance Checker \(SCC\) components\. | December 22, 2021 | 
| [Feature release: VM Import/Export \(VMIE\) integration](#doc-history) | Added support for VM import via all channels \(console, API/CLI, etc\.\), and for VM export via API/CLI\. VM export is not currently available from the Image Builder console\. | December 20, 2021 | 
| [Feature release: AMI sharing for AWS Organizations and OUs](#doc-history) | Updated distribution configuration to add support for sharing output AMIs with AWS Organizations and OUs\. | November 24, 2021 | 
| [Document update: Update component stages and phases](#doc-history) | Expanded content for component stages in Image Builder, and how those interact with AWSTOE component phases\. | September 22, 2021 | 
| [Document update: Add CloudTrail integration content](#doc-history) | Added monitoring summary and CloudTrail integration content\. | September 17, 2021 | 
| [New STIG Versions](toe-stig.md) | Updated STIG versions and applied STIGS for 2021 third quarter release\. | September 10, 2021 | 
| [Feature release: Amazon EventBridge integration](#doc-history) | Added EventBridge support that enables you to connect Image Builder with events from related AWS services, and initiate events based on rules defined in EventBridge\. | August 18, 2021 | 
| [Document update: Reorder AWSTOE pages](#doc-history) | Rearranged AWSTOE pages for clarity\. | August 11, 2021 | 
| [Feature release: Parameterized components and additional instance configuration](#doc-history) | Added support for specifying parameters to customize components for recipes\. Expanded configuration of the EC2 instances that are used for building and testing images, including the ability to specify commands to run on launch, and more control over installation and removal of the Systems Manager agent\. | July 7, 2021 | 
| [New STIG versions](toe-stig.md) | Updated STIG versions and applied STIGS for 2021 second quarter release\. | June 30, 2021 | 
| [Enhancement: Tagging enhancements](#doc-history) | Improved messaging around resource tagging\. | June 25, 2021 | 
| [Feature release: Launch template integration](#doc-history) | Added support for using Amazon EC2 launch templates for AMI distribution in the Distribution settings\. | April 7, 2021 | 
| [Feature release: Container build enhancements](#doc-history) | Added support for configuring block device mappings and specifying AMIs to use as the base image for container builds\. | April 7, 2021 | 
| [New STIG versions](toe-stig.md) | Updated STIG versions and applied STIGS\. | March 5, 2021 | 
| [Update cron expressions](#doc-history) | Image Builder cron processing is updated to increase cron expression granularity to the minute, and use a standard cron scheduling engine\. Examples are updated with the new format\. | February 8, 2021 | 
| [Feature release: Container support](#doc-history) | Added support for creating Docker container images using Image Builder, with registration and storage of the resulting images on Amazon Elastic Container Registry \(Amazon ECR\)\. Content has been rearranged to reflect new functionality and accomodate future growth\. | December 17, 2020 | 
| [Restructured cron documentation](#doc-history) | This page now highlights more information about how cron works with Image Builder pipeline builds, and includes details about UTC time\. Wildcards that are not allowed for specific fields have been removed\. Examples now include expression samples for both console and CLI\. | November 13, 2020 | 
| [Console version 2\.0: updated pipeline editing](#doc-history) | Content changes in getting started and create pipeline tutorials, plus the manage image pipelines page, to incorporate new console features and flow\. | November 13, 2020 | 
| [New STIG versions](toe-stig.md) | Updated STIG versions and applied STIGS\. Note \- list format changed to show STIGs that are applied by default\. | October 15, 2020 | 
| [Support for looping constructs in AWSTOE](image-builder-looping-constructs.md) | Create looping constructs to define a repeated sequence of instructions in the AWSTOE application\. | July 29, 2020 | 
| [Support for local development of AWSTOE components](toe-component-manager.md) | Develop and test image components locally with the AWSTOE application\. | July 28, 2020 | 
| [Encrypted AMIs](#doc-history) | EC2 Image Builder adds support for encrypted AMI distribution\. | July 1, 2020 | 
| [AutoScaling deprecation](#doc-history) | Deprecation of the use of AutoScaling to launch instances\.  | June 15, 2020 | 
| [Support for connectivity through AWS PrivateLink](#doc-history) | You can establish a private connection between your VPC and EC2 Image Builder by creating an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Image Builder APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with Image Builder APIs\. Traffic between your VPC and Image Builder does not leave the Amazon network\. | June 10, 2020 | 
| [New STIG versions](toe-stig.md) | Updated STIG versions and applied STIGS\. | January 23, 2020 | 
| [Troubleshooting](#doc-history) | Added general troubleshooting scenarios\. | January 22, 2020 | 
| [STIG Components](toe-stig.md) | You can create STIG\-compliant images with Image Builder STIG components\. | January 22, 2020 | 