# How EC2 Image Builder works<a name="how-image-builder-works"></a>

When you use the EC2 Image Builder console to create a golden image, a wizard guides you through the following steps\.

1. **Select source image\.** You select a source OS image, for example, an existing AMI\. 

1. **Create image recipe\.** You add components to create an image recipe for your image pipeline\. Components are the building blocks that are consumed by an image recipe, for example, packages for installation, security hardening steps, and tests\. The selected OS and components make up an image recipe\. Components are installed in the order in which they are specified and cannot be reordered after selection\. 

1. **Output\.** Image Builder creates an OS image in the selected output format\.

1. **Distribute\.** You distribute your image to selected AWS Regions after it passes tests in the image pipeline\.

The images that you build from the golden image are in your AWS account\. You can configure your image pipeline to produce updated and patched versions of your AMI by entering a build schedule\. When the build is complete, you can receive notification via [Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\. In addition to producing a final image, Image Builder generates an image recipe that can be used with existing version control systems and continuous integration/continuous deployment \(CI/CD\) pipelines for repeatable automation\. You can share and create new versions of your image recipe\.

**Topics**
+ [AMI components](#image-builder-components)
+ [Default quotas](#image-builder-default-limits)
+ [AWS Regions and Endpoints](#image-builder-regions)
+ [Logs](#image-builder-logs)
+ [Component manager](#image-builder-component-management)
+ [Resources created](#image-builder-resources)

## AMI components<a name="image-builder-components"></a>

An Amazon Machine Image \(AMI\) is the basic unit of deployment in Amazon EC2\. It is a preconfigured Virtual Machine \(VM\) image that contains the OS and software to deploy EC2 instances\. 

An AMI includes the following components:
+ A template for the root volume of the VM\. When you launch an EC2 VM, the root device volume contains the image to boot the instance\. When instance store is used, the root device is an instance store volume created from a template in Amazon S3\. For more information, see [Amazon EC2 Root Device Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html)\. 
+ When Amazon EBS is used, the root device is an EBS volume created from an [EBS snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)\.
+ Launch permissions that determine the AWS accounts that can launch VMs with the AMI\.
+ [Block device mapping](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) data that specifies the volumes to attach to the instance after launch\.
+ A unique [resource identifier](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/resource-ids.html) per Region per account\.
+ [Metadata ](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)payloads such as tags, and properties such as Region, operating system, architecture, root device type, provider, launch permissions, storage for the root device, and signing status\.
+ An AMI signature to protect against unauthorized tampering\. For more information, see [Instance Identity Documents](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-identity-documents.html)\.

## Default quotas<a name="image-builder-default-limits"></a>

To view the default quotas for EC2 Image Builder, see [EC2 Image Builder Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/imagebuilder.html)\. 

## AWS Regions and Endpoints<a name="image-builder-regions"></a>

To view the service endpoints for EC2 Image Builder, see [EC2 Image Builder Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/imagebuilder.html)\.

## Logs<a name="image-builder-logs"></a>

EC2 Image Builder integrates with AWS services for monitoring to help you troubleshoot image build issues\. Image Builder tracks and displays the progress for each step in the image building process\. You can configure the image\-building application to send logs to CloudWatch as well as to an S3 location that you provide\. For more information about CloudWatch Logs, see [What Is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)\.

CloudWatch logging support is enabled by default\. Logs are retained on the instance and streamed to CloudWatch\. As part of the AMI creation process, logs are removed from the instance\. The logs are streamed to the following LogStream:
+ LogGroup: `"/aws/imagebuilder/<ImageName>`
+ LogStream: `<ImageVersion>/<ImageBuildVersion>["x.x.x/x"]`

You can opt out of CloudWatch streaming by removing the following permissions associated with the instance profile\.

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

For advanced troubleshooting, you can run predefined commands and scripts using [AWS Systems Manager \(SSM\) Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) \. For more information, see [Troubleshooting EC2 Image Builder](image-builder-troubleshooting.md)\.

## Component manager<a name="image-builder-component-management"></a>

Image Builder uses a component management application that helps you orchestrate complex workflows, modify system configurations, and test your systems without writing code\. This application uses a declarative document schema\. Because it is a standalone application, it does not require additional server setup\. It can run on any cloud infrastructure and on premises\. 

EC2 Image Builder uses this application to perform all on\-instance activities, such as build, validation, and test\. You define a document that describes how to build, validate, and test your image\. EC2 Image Builder sends the component to your instance and the application interprets and applies it to your instance by executing the defined phases, steps, and actions\. When complete, the application sends a summary to EC2 Image Builder\. It also sends detailed execution outputs to Amazon S3 if you specified an S3 bucket in your pipeline configuration\. EC2 Image Builder then cleans up the application and removes it from the instance using [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements)\. 
+ **Build phase**\. The image is modified\. For example, you can configure your image to install an application or to modify the operating system firewall settings\. The validate phase is executed as part of the build phase, prior to the creation of the image\. 
+ **Test phase**\. Tests are executed against your new image after it is created\.

EC2 Image Builder uses the component management application as follows\.

1. You define an EC2 Image Builder component, which is a document that describes how to build, validate, and test your image\.

1. EC2 Image Builder dispatches the work to be performed by copying the document and application to your instance\.

1. The application executes the phases, steps, and actions defined in the document\. 

For more information about the Component Manager used by Image Builder to orchestrate workflows, including information about documents, supported action modules, and STIGs, see [EC2 Image Builder Component Manager](image-builder-component-manager.md)\.

## Resources created<a name="image-builder-resources"></a>

When you create a pipeline, no resources external to Image Builder are created\. It is only when an image is created via the pipeline schedule, the **Run Pipeline **action from the Image Builder console, the `StartImagePipelineExecution` API, or the `CreateImage` API that resources external to Image Builder are created\. The following resources are created during image creation\.
+ Amazon EC2 Instance
+ SSM Inventory Association \(via SSM State Manager\) \(if `EnhancedImageMetadata` is Enabled\)
+ Amazon EC2 AMI 
+ EBS Snapshot \(associated with Amazon EC2 AMI\)

After the AMI has been created, all of the resources are deleted except for the Amazon EBS Snapshot and the Amazon EC2 AMI\.