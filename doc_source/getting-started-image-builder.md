# Getting Started with EC2 Image Builder<a name="getting-started-image-builder"></a>

This section contains information you need to set up your environment and create an image pipeline with Amazon Elastic Compute Cloud Image Builder\.

**Topics**
+ [Setting Up](#image-builder-setting-up)
+ [Accessing EC2 Image Builder](#image-builder-accessing-prereq)
+ [Build and Automate an OS Image Deployment Using the EC2 Image Builder Console](#image-builder-image-deployment-console)

## Setting Up<a name="image-builder-setting-up"></a>

The following prerequisites must be verified in order to create an image pipeline with EC2 Image Builder\.

### Auto Scaling Groups<a name="image-builder-auto-scaling-prereq"></a>

EC2 Image Builder uses Auto Scaling groups to launch instances during build and test phases of the image pipeline\. To create an image with EC2 Image Builder, you must create an Amazon EC2 Auto Scaling group at least one time\. When you use Amazon EC2 Auto Scaling, a required service\-linked role is created in your account\. For steps on how to create an Auto Scaling group, see [Getting Started with Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/GettingStartedTutorial.html)\. 

### EC2 Image Builder Service\-Linked Role<a name="image-builder-auto-scaling-prereq"></a>

EC2 Image Builder uses a service\-linked role to grant permissions to other AWS services on your behalf\. You don't need to manually create a service\-linked role\. When you create your first Image Builder resource in the AWS Management Console, the AWS CLI, or the AWS API, Image Builder creates the service\-linked role for you\. For more information about the service\-linked role that Image Builder creates in your account, see [Using Service\-Linked Roles for EC2 Image Builder](image-builder-service-linked-role.md)\. 

## Accessing EC2 Image Builder<a name="image-builder-accessing-prereq"></a>

You can manage EC2 Image Builder from one of the following interfaces\.
+ **EC2 Image Builder console landing page**\. From the [EC2 Image Builder landing page](https://us-east-1.console.aws.amazon.com/imagebuilder/home?region=us-east-1)\.
+ **AWS Command Line Interface \(AWS CLI\)**\. You can use the AWS CLI to access AWS API operations\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the AWS Command Line Interface User Guide\. 
+ **AWS Tools for SDKs**\. You can use [AWS SDKs and Tools](https://aws.amazon.com/getting-started/tools-sdks/) to access and manage Image Builder using your preferred language\. 

## Build and Automate an OS Image Deployment Using the EC2 Image Builder Console<a name="image-builder-image-deployment-console"></a>

The following steps guide you through an image deployment with Image Builder from the EC2 Image Builder console\.

1. From the **EC2 Image Builder** landing page, select **Create image pipeline**\.

1. On the **Define Recipe** page, create an image recipe, which includes your source image and components\. 

   1. Choose your source image\. The source image includes the image OS and the image to configure\. After selecting your image OS, you have three options to select an image to configure\.

      1. Select an image from the managed images, which includes Image Builder images to help you get started, images that you have already created, and images that have been shared with you\. To select an image, enter the image ARN in the text box, or select **Browse images **to view managed images\. 

      1. Use a custom AMI by entering the AMI ID\. 

   1. Select the **Build components**\. Components are installation packages, security hardening steps, and tests to be consumed by the image recipe when building your image\. After an image recipe has been created, its components cannot be modified or replaced\. If you want to update the components in an image recipe, create a new image recipe or image recipe version\. Components include two component types\.

      1. **Build components**\. Build components are installation packages and security hardening steps\. You can enter a component ARN or browse and select from a list of Image Builder components to help you get started\. To create a new component, select **Create Component**\. See [Create New Component](managing-image-builder-console.md#image-builder-create-component) for information about how to create a component\. Enter or select the components in the order that you want them to run in the image build pipeline\. 

      1. **Tests**\. Test components are tests to perform on the output image built by your image pipeline\. Enter a test component ARN or browse and select from a list of Image Builder test components to help you get started\. To create a new component, select **Create Component**\. See [Create New Component](managing-image-builder-console.md#image-builder-create-component) for information about how to create a component\. Enter or select the components in the order that you want them to run in the image build pipeline\.

      After you have entered your source image and components, select **Next**\. 

1. From the **Configure Pipeline** page, define the image pipeline infrastructure and build schedule\. 

   1. Provide the following specifications under **Pipeline details**\. 

      1. Enter a **Name** for your image pipeline\. You must use a unique name for your image pipeline\. 

      1. Provide an optional **Description** for your image deployment pipeline\. 

      1. Select an **IAM** role to associate with the instance profile or **Create a new role**\. If you create a new role, Image Builder will take you to the IAM console\. As a starting point, you can use the following IAM role policy: **EC2InstanceProfileForImageBuilder**

   1. Select a **Build schedule** to run your image pipeline\. 

      1. If you select **Manual**, you can choose when to run the pipeline\. When you want to run the pipeline, select **Run pipeline** on the **Pipeline details** page\. 

      1. If you select **Schedule builder**, you can set the build pipeline to run automatically using the job scheduler\. Enter the cadence after **Run pipeline every**\. You can select to run the pipeline daily, weekly, or monthly\. 

      1. If you select **CRON expression**, you can set the build pipeline to run using a syntax that specifies the time and intervals to run it\. Enter the expression in the text box\. 

   1. Optionally, enter the **Infrastructure** specifications to define the infrastructure for your image\. These settings are associated with the EC2 instance that is launched in your account for the purpose of building the image\. 

      1. Select an **Instance type**\. The instance type selected should adhere to the requirements of the software that you plan to run on your instance\. For more information about EC2 instance types, see [Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html#AvailableInstanceTypes) in the *EC2 User Guide*\. 

      1. If you want to receive notifications and alerts from Image Builder regarding any steps performed in your image pipeline, you can enter an **SNS topic ARN** to be notified by the **AWS Simple Notification Service \(SNS\)**\. For more information, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/welcome.html)\.

      1. Under **Troubleshooting settings**, provide the following information\. These settings are useful for performing troubleshooting on your instance if the image build fails\. 

         Under **Key pair name**, select an existing key pair from the dropdown list or create a new one\. If you select **Create key pair name** to create a new key pair, you are directed to the Amazon EC2 console\. Choose **Create a new key pair**, enter a name for the key pair, and then choose **Download Key Pair**\.
**Important**  
This is the only chance for you to save the private key file, so be sure to download it and save it in a safe place\. You must provide the name of your key pair when you launch an instance, and provide the corresponding private key each time that you connect to the instance\. 

         Return to the Image Builder console and choose refresh next to the **Key pair name** dropdown\. The newly created key pair appears in the dropdown list\. For more information about key pairs, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

      1. Select whether or not you want to **Terminate your instance upon failure** by selecting the check box\. If you want to be able to troubleshoot the instance when the image build fails, then make sure the check box is not checked\.

      1. Under **S3 Logs**, select the **S3 bucket** to which you want to send your instance log files\. To browse and select your Amazon S3 bucket locations, select **Browse S3**\. 

      1. Under **Advanced Settings**, provide the following information if you want to select a VPC to launch your instance\.

         1. Select a Virtual Private Cloud into which to launch your instance\. For more information about VPCs, see the [VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)\. You can also choose to **Create a new VPC**\. If you select to do this, you will be taken to the **VPC console**\. In order to allow communication between your VPC and the internet, you must enable this connectivity with an internet gateway\. To add an internet gateway to your VPC, follow the steps in [Creating and Attaching an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon VPC User Guide*\.

         1. If you select a VPC, choose the **Public subnet ID** associated with your selected VPC or select **Create new subnet**\. For more information, see [VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\. 

         1. If you select a VPC, select the **Security groups** that are associated with your VPC, or select **Create a new security group**\. For help with security groups, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\. 

      After you have entered all of your infrastructure specifications, choose **Next**\. 

1. From the **Configure additional settings** page, you can optionally define the test and distribution settings, along with other optional configuration parameters that are performed after the image is built\. If you want to define these configurations, provide the following information\. 

   1. Under **Associate license configuration to AMI**, you can choose to associate the output AMI with a pre\-existing license configuration that you created with AWS License Manager\. Select one or more unique license configuration IDs from the dropdown\. If you want to create a new license configuration, select **Create new License Configuration**\. This will take you to the License Manager console\. For more information, see [What Is AWS License Manager? ](https://docs.aws.amazon.com/license-manager/latest/userguide/license-manager.html)

   1. Provide the following specifications under **Output AMI**\.

      1. Enter a **Name** for your output AMI\. When the image pipeline has completed, this will be the name of the created AMI\. 

      1. Under **AMI tags**, add a **Key** and optional **Value** tag for your image\. For more information about tagging resources, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/latest/UserGuide/Using_Tags.html)\. 

   1. Under **AMI distribution settings**, you can specify other AWS Regions to which you would like your AMI to be copied\. You can also configure permissions for the outbound AMI\. You can choose to allow all AWS accounts, or only specific accounts, to launch the created AMI\. If you choose to allow all AWS accounts to launch the AMI, the output AMI will be public\. 

      1. **Select the AWS Regions to distribute the AMI**\. Your current Region is included by default\. 

      1. Under **Launch permissions**, you can set the AMI as **Private** or **Public**\. The default setting is **Private**\. When you set launch permissions to private, you can grant permissions to specific AWS accounts\. If you set them to public, all AWS users will have access to the output AMI\. 

         1. Select **Public** or **Private**\.

         1. If you select **Private**, enter the account numbers of the accounts to which you want to grant launch permissions and select **Add**\. 

1. On the **Review and create** page, you can review all of your settings before you create your image pipeline\. Review your **Recipe details**, your **Pipeline configuration details**, and your **Additional settings**\. If you want to make changes, select **Edit** to return to the specification settings that you want to change or update\. When the settings reflect your desired configuration, select **Create Pipeline**\. 

1. If the creation of your image pipeline fails, you will receive a message with the returned errors\. Address these errors and try to create your pipeline again\.

1. When your image pipeline creation succeeds, you are taken to the **Image pipelines** page\. From here, you can manage, delete, disable, view details about, and run your image pipeline\. 
