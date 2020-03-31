# How EC2 Image Builder Works<a name="how-image-builder-works"></a>

When you use the EC2 Image Builder console to create a golden image, a wizard guides you through the following steps\.

1. **Select source image\.** You select a source OS image, for example, an existing AMI\. 

1. **Create image recipe\.** You add components to create an image recipe for your image pipeline\. Components are the building blocks that are consumed by an image recipe, for example, packages for installation, security hardening steps, and tests\. The selected OS and components make up an image recipe\. Components are installed in the order in which they are specified and cannot be reordered after selection\. 

1. **Output\.** Image Builder creates an OS image in the selected output format\.

1. **Distribute\.** You distribute your image to selected AWS Regions after it passes tests in the image pipeline\.

The images that you build from the golden image are in your AWS account\. You can configure your image pipeline to produce updated and patched versions of your AMI by entering a build schedule\. When the build is complete, you can receive notification via [Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\. In addition to producing a final image, Image Builder generates an image recipe that can be used with existing version control systems and continuous integration/continuous deployment \(CI/CD\) pipelines for repeatable automation\. You can share and create new versions of your image recipe\.

**Topics**
+ [Components](#image-builder-components)
+ [Default Quotas](#image-builder-default-limits)
+ [AWS Regions and Endpoints](#image-builder-regions)
+ [Logs](#image-builder-logs)
+ [Component Manager](#image-builder-component-management)

## Components<a name="image-builder-components"></a>

An Amazon Machine Image \(AMI\) is the basic unit of deployment in Amazon EC2\. It is a preconfigured Virtual Machine \(VM\) image that contains the OS and software to deploy EC2 instances\. 

An AMI includes the following components:
+ A template for the root volume of the VM\. When you launch an EC2 VM, the root device volume contains the image to boot the instance\. When instance store is used, the root device is an instance store volume created from a template in Amazon S3\. For more information, see [Amazon EC2 Root Device Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html)\. 
+ When Amazon EBS is used, the root device is an EBS volume created from an [EBS snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)\.
+ Launch permissions that determine the AWS accounts that can launch VMs with the AMI\.
+ [Block device mapping](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) data that specifies the volumes to attach to the instance after launch\.
+ A unique [resource identifier](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/resource-ids.html) per Region per account\.
+ [Metadata ](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)payloads such as tags, and properties such as Region, operating system, architecture, root device type, provider, launch permissions, storage for the root device, and signing status\.
+ An AMI signature to protect against unauthorized tampering\. For more information, see [Instance Identity Documents](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-identity-documents.html)\.

## Default Quotas<a name="image-builder-default-limits"></a>

The following table provides the default quotas for EC2 Image Builder\. Unless otherwise noted, each quota is per AWS Region\. Please contact [AWS Support ](https://console.aws.amazon.com/support/home#/case/create?issueType=technical)to request an increase in your service quota\. 


| Name | Description  | Default Quota | 
| --- | --- | --- | 
| Concurrent builds | The maximum number of concurrent builds that can be in progress in this account in the current Region\. | 100 builds per account per Region | 
| Components  | The maximum number of EC2 Image Builder components that you can create in an account in the current Region\. | 1,000 components per account per Region | 
| Component size  | The maximum size of the data field of an EC2 Image Builder component\. | 16 KB | 
| Image pipelines  | The maximum number of EC2 Image Builder image pipelines that you can create in an account in the current Region\.  | 75 image pipelines per account per Region | 
| Image recipes | The maximum number of EC2 Image Builder image recipes that you can create in an account in the current Region\. |  1,000 image recipes per account per Region  | 
| Components per image recipe | The maximum number of EC2 Image Builder components that can be associated with a single EC2 Image Builder image recipe\. |  20 components per image per Region  | 
| Infrastructure configurations | The maximum number of EC2 Image Builder infrastructure configurations that you can create in an account in the current Region\. |  1,000 configurations per account per Region  | 
| Distribution configurations | The maximum number of EC2 Image Builder distribution configurations that you can create in an account in the current Region\.  | 1,000 configurations per account per Region | 

## AWS Regions and Endpoints<a name="image-builder-regions"></a>

The following AWS Regions and endpoints are currently supported by EC2 Image Builder\.


| Region Name | Region | Endpoint | Protocol | 
| --- | --- | --- | --- | 
| Asia Pacific \(Hong Kong\) | ap\-east\-1 | imagebuilder\.ap\-east\-1\.amazonaws\.com  | HTTPS | 
| Asia Pacific \(Tokyo\)  | ap\-northeast\-1  | imagebuilder\.ap\-northeast\-1\.amazonaws\.com | HTTPS | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | imagebuilder\.ap\-northeast\-2\.amazonaws\.com | HTTPS | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | imagebuilder\.ap\-south\-1\.amazonaws\.com | HTTPS | 
| Asia Pacific \(Singapore\)  | ap\-southeast\-1  | imagebuilder\.ap\-southeast\-1\.amazonaws\.com  | HTTPS | 
| Asia Pacific \(Sydney\)  | ap\-southeast\-2  | imagebuilder\.ap\-southeast\-2\.amazonaws\.com  | HTTPS | 
| Canada \(Central\)  | ca\-central\-1  | imagebuilder\.ca\-central\-1\.amazonaws\.com | HTTPS | 
| EU \(Frankfurt\) | eu\-central\-1 | imagebuilder\.eu\-central\-1\.amazonaws\.com | HTTPS | 
| EU \(Ireland\) | eu\-west\-1 | imagebuilder\.eu\-west\-1\.amazonaws\.com  | HTTPS | 
| EU \(London\) | eu\-west\-2 | imagebuilder\.eu\-west\-2\.amazonaws\.com | HTTPS | 
| EU \(Paris\) | eu\-west\-3 | imagebuilder\.eu\-west\-3\.amazonaws\.com  | HTTPS | 
| EU \(Stockholm\)  | eu\-north\-1 | imagebuilder\.eu\-north\-1\.amazonaws\.com  | HTTPS | 
| Middle East \(Bahrain\)  | me\-south\-1 | imagebuilder\.me\-south\-1\.amazonaws\.com | HTTPS | 
| South America \(Sao Paulo\) | sa\-east\-1 | imagebuilder\.sa\-east\-1\.amazonaws\.com | HTTPS | 
| US East \(N\. Virginia\) | us\-east\-1 | imagebuilder\.us\-east\-1\.amazonaws\.com  | HTTPS | 
| US East \(Ohio\) | us\-east\-2 | imagebuilder\.us\-east\-2\.amazonaws\.com | HTTPS | 
| US West \(N\. California\)  | us\-west\-1  | imagebuilder\.us\-west\-1\.amazonaws\.com  | HTTPS | 
| US West \(Oregon\)  | us\-west\-2  | imagebuilder\.us\-west\-2\.amazonaws\.com  | HTTPS | 
| AWS GovCloud \(US\-East\) | us\-gov\-east\-1  | imagebuilder\.us\-gov\-east\-1\.amazonaws\.com | HTTPS | 
| AWS GovCloud \(US\-West\)  | us\-gov\-west\-1  | imagebuilder\.us\-gov\-west\-1\.amazonaws\.com  | HTTPS | 

## Logs<a name="image-builder-logs"></a>

For common failure modes, you can use predefined AWS Systems Manager troubleshooting scripts\. These scripts can help you troubleshoot the inability to connect to the VM, source image not booting, installed software not being listed, and customization steps only partially executing\. For more information, see [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\.

## Component Manager<a name="image-builder-component-management"></a>

Image Builder uses a component management application that helps you orchestrate complex workflows, modify system configurations, and test your systems without writing code\. This application uses a declarative document schema\. Because it is a standalone application, it does not require additional server setup\. It can run on any cloud infrastructure and on premises\. 

EC2 Image Builder uses this application to perform all on\-instance activities, such as build, validation, and test\. You define a document that describes how to build, validate, and test your image\. EC2 Image Builder sends the component to your instance and the application interprets and applies it to your instance by executing the defined phases, steps, and actions\. When complete, the application sends a summary to EC2 Image Builder\. It also sends detailed execution outputs to Amazon S3 if you specified an S3 bucket in your pipeline configuration\. EC2 Image Builder then cleans up the application and removes it from the instance using [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements)\. 
+ **Build phase**\. The image is modified\. For example, you can configure your image to install an application or to modify the operating system firewall settings\. The validate phase is executed as part of the build phase, prior to the creation of the image\. 
+ **Test phase**\. Tests are executed against your new image after it is created\.

EC2 Image Builder uses the component management application as follows\.

1. You define an EC2 Image Builder component, which is a document that describes how to build, validate, and test your image\.

1. EC2 Image Builder dispatches the work to be performed by copying the document and application to your instance\.

1. The application executes the phases, steps, and actions defined in the document\. 

For more information about the Component Manager used by Image Builder to orchestrate workflows, including information about documents, supported action modules, and STIGs, see [EC2 Image Builder Component Manager](image-builder-component-manager.md)\.
