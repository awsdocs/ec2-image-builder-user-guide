# How EC2 Image Builder works<a name="how-image-builder-works"></a>

When you use the EC2 Image Builder pipeline console wizard to create a custom image, a wizard guides you through the following steps\.

1. **Specify pipeline details** – Enter information about your pipeline, such as a name, description, tags, and a schedule to run automated builds\. You can choose manual builds, if you prefer\.

1. **Choose recipe** – Choose between building an AMI, or building a container image\. For both types of output images, you enter a name and version for your recipe, select a base image, and choose components to add for building and testing\. You can also choose automatic versioning, to ensure that you always use the latest available Operating System \(OS\) version for your base image\. Container recipes additionally define Dockerfiles, and the target Amazon ECR repository for your output Docker container image\.
**Note**  
Components are the building blocks that are consumed by an image recipe or a container recipe\. For example, packages for installation, security hardening steps, and tests\. The selected base image and components make up an image recipe\.

1. **Define infrastructure configuration** – Image Builder launches EC2 instances in your account to customize images and run validation tests\. The Infrastructure configuration settings specify infrastructure details for the instances that will run in your AWS account during the build process\.

1. **Define distribution settings** – Choose the AWS Regions to distribute your image to after the build is complete and has passed all its tests\. The pipeline automatically distributes your image to the Region where it runs the build, and you can add image distribution for other Regions\.

The images that you build from your custom base image are in your AWS account\. You can configure your image pipeline to produce updated and patched versions of your image by entering a build schedule\. When the build is complete, you can receive notification through [Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\. In addition to producing a final image, the Image Builder console wizard generates a recipe that can be used with existing version control systems and continuous integration/continuous deployment \(CI/CD\) pipelines for repeatable automation\. You can share and create new versions of your recipe\.

**Topics**
+ [AMI elements](#ami-image-elements)
+ [Default quotas](#image-builder-default-limits)
+ [AWS Regions and Endpoints](#image-builder-regions)
+ [Component management](#ibhow-component-management)
+ [Semantic versioning](ibhow-semantic-versioning.md)
+ [Resources created](#image-builder-resources)
+ [Distribution](#image-builder-distribution)
+ [Sharing Resources](#ibhow-sharing)
+ [Compliance](#ibhow-compliance)

## AMI elements<a name="ami-image-elements"></a>

An Amazon Machine Image \(AMI\) is a preconfigured virtual machine \(VM\) image that contains the OS and software to deploy EC2 instances\.

An AMI includes the following elements:
+ A template for the root volume of the VM\. When you launch an Amazon EC2 VM, the root device volume contains the image to boot the instance\. When instance store is used, the root device is an instance store volume created from a template in Amazon S3\. For more information, see [Amazon EC2 Root Device Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html)\. 
+ When Amazon EBS is used, the root device is an EBS volume created from an [EBS snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)\.
+ Launch permissions that determine the AWS accounts that can launch VMs with the AMI\.
+ [Block device mapping](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) data that specifies the volumes to attach to the instance after launch\.
+ A unique [resource identifier](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/resource-ids.html) for each Region, for each account\.
+ [Metadata ](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)payloads such as tags, and properties, such as Region, operating system, architecture, root device type, provider, launch permissions, storage for the root device, and signing status\.
+ An AMI signature for Windows images to protect against unauthorized tampering\. For more information, see [Instance Identity Documents](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-identity-documents.html)\.

## Default quotas<a name="image-builder-default-limits"></a>

To view the default quotas for Image Builder, see [Image Builder Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/imagebuilder.html)\. 

## AWS Regions and Endpoints<a name="image-builder-regions"></a>

To view the service endpoints for Image Builder, see [Image Builder Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/imagebuilder.html)\.

## Component management<a name="ibhow-component-management"></a>

EC2 Image Builder uses a component management application AWS Task Orchestrator and Executor \(AWSTOE\) that helps you orchestrate complex workflows, modify system configurations, and test your systems with YAML\-based script components\. Because AWSTOE is a standalone application, it does not require any additional setup\. It can run on any cloud infrastructure and on premises\. To get started using AWSTOE as a standalone application, see [Get started with AWSTOE](toe-get-started.md)\.

Image Builder uses AWSTOE to perform all on\-instance activities\. These include building and validating your image before taking a snapshot, and testing the snapshot to ensure that it functions as expected before creating the final image\. For more information about how Image Builder uses AWSTOE to manage its components, see [Manage components with AWSTOE](manage-components.md)\. For more information about creating components with AWSTOE, see [AWS Task Orchestrator and Executor component manager](toe-component-manager.md)\.

### Image testing<a name="ibhow-testing"></a>

You can use AWSTOE test components to validate your image, and ensure that it functions as expected, prior to creating the final image\.

Generally, each test component consists of a YAML document that contains a test script, a test binary, and test metadata\. The test script contains the orchestration commands to start the test binary, which can be written in any language supported by the OS\. Exit status codes indicate the test outcome\. Test metadata describes the test and its behavior; for example, the name, description, paths to test binary, and expected duration\.

## Resources created<a name="image-builder-resources"></a>

When you create a pipeline, no resources external to Image Builder are created, unless the following is true: 
+ When an image is created through the pipeline schedule
+ When you choose **Run Pipeline** from the **Actions** menu in the Image Builder console
+ When you run either of these commands from the API or AWS CLI: StartImagePipelineExecution or CreateImage

The following resources are created during the image build process:

**AMI image pipelines**
+ EC2 instance \(*temporary*\)
+ Systems Manager Inventory Association \(through Systems Manager State Manager\) `EnhancedImageMetadata` is Enabled\) on the EC2 instance
+ Amazon EC2 AMI
+ The Amazon EBS Snapshot associated with Amazon EC2 AMI

**Container image pipelines**
+ Docker container running on an EC2 instance \(*temporary*\)
+ Systems Manager Inventory Association \(through Systems Manager State Manager\) `EnhancedImageMetadata` is Enabled\) on the EC2 instance
+ Docker container image
+ Dockerfile

After the image has been created, all of the temporary resources are deleted\.

## Distribution<a name="image-builder-distribution"></a>

EC2 Image Builder can distribute AMIs or container images to any AWS Region\. The image is copied to each Region that you specify in the account used to build the image\.

For AMI output images, you can define AMI launch permissions to control which AWS accounts are permitted to launch EC2 instances with the created AMI\. For example, you can make the image private, public, or share with specific accounts\. If you both distribute the AMI to other Regions, and define launch permissions for other accounts, the launch permissions are propagated to the AMIs in all of the Regions in which the AMI is distributed\.

You can also use your AWS Organizations account to enforce limitations on member accounts to launch instances only with approved and compliant AMIs\. For more information, see [Managing the AWS accounts in Your Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts.html)\.

To update your distribution settings using the Image Builder console, follow the steps to [Create a new image recipe version \(console\)](create-image-recipes.md#create-image-recipe-version-console), or [Create a new container recipe version with the console](create-container-recipes.md#create-container-recipe-version)\.

## Sharing Resources<a name="ibhow-sharing"></a>

To share components, recipes, or images with other accounts or within AWS Organizations, see [Share EC2 Image Builder resources](manage-shared-resources.md)\.

## Compliance<a name="ibhow-compliance"></a>

For CIS, EC2 Image Builder uses Amazon Inspector to perform assessments for exposure, vulnerabilities, and deviations from best practices and compliance standards\. For example, Image Builder assesses unintended network accessibility, unpatched CVEs, public internet connectivity, and remote root login activation\. Amazon Inspector is offered as a test component that you can choose to add to your image recipe\. For more information about Amazon Inspector, see the [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html)User Guide\. For hardening, EC2 Image Builder validates with STIG\. For a complete list of STIG components available through Image Builder, see [AWS Task Orchestrator and Executor STIG components](toe-stig.md)\. For more information, see [Center for Internet Security \(CIS\) Benchmarks](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_cis.html)\.