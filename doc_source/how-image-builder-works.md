# How EC2 Image Builder works<a name="how-image-builder-works"></a>

When you use the EC2 Image Builder pipeline console wizard to create a custom image, a wizard guides you through the following steps\.

1. **Specify pipeline details** – Enter information about your pipeline, such as a name, description, tags, and a schedule to run automated builds\. You can choose manual builds, if you prefer\.

1. **Choose recipe** – Choose between building an AMI, or building a container image\. For both types of output images, you enter a name and version for your recipe, select a source image, and choose components to add for building and testing\. You can also choose automatic versioning, to ensure that you always use the latest available Operating System \(OS\) version for your source image\. Container recipes additionally define Dockerfiles, and the target Amazon ECR repository for your output Docker container image\.
**Note**  
Components are the building blocks that are consumed by an image recipe or a container recipe\. For example, packages for installation, security hardening steps, and tests\. The selected source image and components make up an image recipe\.

1. **Define infrastructure configuration** – Image Builder launches Amazon EC2 instances in your account to customize images and run validation tests\. The Infrastructure configuration settings specify infrastructure details for the instances that will run in your AWS account during the build process\.

1. **Define distribution settings** – Choose the AWS Regions to distribute your image to after the build is complete and has passed all its tests\. The pipeline automatically distributes your image to the Region where it runs the build, and you can add image distribution for other Regions\.

The images that you build from your custom base image are in your AWS account\. You can configure your image pipeline to produce updated and patched versions of your image by entering a build schedule\. When the build is complete, you can receive notification through [Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\. In addition to producing a final image, the Image Builder console wizard generates a recipe that can be used with existing version control systems and continuous integration/continuous deployment \(CI/CD\) pipelines for repeatable automation\. You can share and create new versions of your recipe\.

**Topics**
+ [AMI components](#ami-image-components)
+ [Default quotas](#image-builder-default-limits)
+ [AWS Regions and Endpoints](#image-builder-regions)
+ [Logs](#image-builder-logs)
+ [Component manager](#image-builder-component-management)
+ [Resources created](#image-builder-resources)
+ [Testing](#image-builder-testing)
+ [Distribution](#image-builder-distribution)
+ [Sharing Resources](#image-builder-distribution)
+ [Compliance](#image-builder-compliance)

## AMI components<a name="ami-image-components"></a>

An Amazon Machine Image \(AMI\) is a preconfigured Virtual Machine \(VM\) image that contains the OS and software to deploy EC2 instances\.

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

EC2 Image Builder integrates with AWS services for monitoring to help you troubleshoot image build issues\. Image Builder tracks and displays the progress for each step in the image building process\. You can configure the image\-building application to send logs to CloudWatch as well as to an S3 location that you provide\. For more information about CloudWatch Logs, see [What Is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)

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

For advanced troubleshooting, you can run predefined commands and scripts using [Amazon EC2 Systems Manager \(SSM\) Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\. For more information, see [Troubleshoot EC2 Image Builder](image-builder-troubleshooting.md)\.

## Component manager<a name="image-builder-component-management"></a>

EC2 Image Builder uses a component management application \(AWSTOE\) that helps you orchestrate complex workflows, modify system configurations, and test your systems without writing code\. This application uses a declarative document schema\. Because it is a standalone application, it does not require additional server setup\. It can run on any cloud infrastructure and on premises\. To download the component management application \(AWSTOE\) as a standalone application, see [Get started with the AWSTOE application ](image-builder-component-manager-local.md#image-builder-component-manager-commands)\.

Image Builder uses this application to perform all on\-instance activities, such as build, validation, and test\. You define a document that describes how to build, validate, and test your image\. Image Builder sends the component to your instance and the application interprets and applies it to your instance by executing the defined phases, steps, and actions\. When complete, the application sends a summary to Image Builder\. It also sends detailed execution outputs to Amazon S3 if you specified an S3 bucket in your pipeline configuration\. Image Builder then cleans up the application and removes it from the instance using [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements)\. 
+ **Build phase**\. The image is modified\. For example, you can configure your image to install an application or to modify the operating system firewall settings\. The validate phase is executed as part of the build phase, prior to the creation of the image\. 
+ **Test phase**\. Tests are executed against your new image after it is created\.

Image Builder uses the component management application as follows\.

1. You define an Image Builder component, which is a document that describes how to build, validate, and test your image\.

1. Image Builder dispatches the work to be performed by copying the document and application to your instance\.

1. The application executes the phases, steps, and actions defined in the document\. 

For more information about the Component Manager used by Image Builder to orchestrate workflows, including information about documents, supported action modules, and STIGs, see [EC2 Image Builder component manager](image-builder-component-manager.md)\.

## Resources created<a name="image-builder-resources"></a>

When you create a pipeline, no resources external to Image Builder are created, unless the following is true: 
+ When an image is created through the pipeline schedule
+ When you choose **Run Pipeline** from the **Actions** menu in the Image Builder console
+ When you run either of these commands from the API or AWS CLI: StartImagePipelineExecution or CreateImage

The following resources are created during the image build process:

**AMI image pipelines**
+ Amazon EC2 Instance \(*temporary*\)
+ SSM Inventory Association \(through SSM State Manager\) `EnhancedImageMetadata` is Enabled\) on the Amazon EC2 instance
+ Amazon EC2 AMI
+ The Amazon EBS Snapshot associated with Amazon EC2 AMI

**Container image pipelines**
+ Docker container running on an Amazon EC2 instance \(*temporary*\)
+ SSM Inventory Association \(through SSM State Manager\) `EnhancedImageMetadata` is Enabled\) on the Amazon EC2 instance
+ Docker container image
+ Dockerfile

After the image has been created, all of the temporary resources are deleted\.

## Testing<a name="image-builder-testing"></a>

Generally, each test consists of a test script, a test binary, and test metadata\. The test script contains the orchestration commands to start the test binary, which can be written in any language supported by the OS\. Exit status codes indicate the test outcome\. Test metadata describes the test and its behavior \(for example, the name, description, paths to test binary, and expected duration\)\.

## Distribution<a name="image-builder-distribution"></a>

EC2 Image Builder can distribute AMIs or container images to any AWS Region\. The image is copied to each Region that you specify in the account used to build the image\.

For AMI output images, you can define AMI launch permissions to control which AWS accounts are permitted to launch Amazon EC2 instances with the created AMI\. For example, you can make the image private, public, or share with specific accounts\. If you both distribute the AMI to other Regions, and define launch permissions for other accounts, the launch permissions are propagated to the AMIs in all of the Regions in which the AMI is distributed\.

To update your distribution settings using the Image Builder console, follow the steps to [Create a new image recipe version \(console\)](create-image-recipes.md#create-image-recipe-version-console), or [Create a new container recipe version \(console\)](create-container-recipes.md#create-container-recipe-version)\.

## Sharing Resources<a name="image-builder-distribution"></a>

To share components, recipes, or images with other accounts or within AWS Organizations, see [Share EC2 Image Builder resources](manage-shared-resources.md)\.

## Compliance<a name="image-builder-compliance"></a>

For CIS, EC2 Image Builder uses Amazon Inspector to perform automatic assessments for exposure, vulnerabilities, and deviations from best practices and compliance standards\. For example, it assesses unintended network accessibility, unpatched CVEs, public internet connectivity, and remote root login enablement\. Amazon Inspector is offered as a test component that you can choose to add to your image recipe\. \. For hardening, EC2 Image Builder validates using STIG\. For a complete list of STIG components available through Image Builder, see [EC2 Image Builder STIG components](image-builder-stig.md)\. For more information, see [Center for Internet Security \(CIS\) Benchmarks](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_cis.html) and [Amazon EC2 Windows Server AMIs for STIG Compliance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ami-windows-stig.html)\.