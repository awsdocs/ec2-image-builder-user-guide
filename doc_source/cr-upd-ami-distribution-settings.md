# Create and update AMI distribution configurations<a name="cr-upd-ami-distribution-settings"></a>

This section covers creating and updating distribution configurations for an Image Builder AMI\.

**Topics**
+ [Create an AMI distribution configuration \(console\)](#create-ami-distribution-config-console)
+ [Create an AMI distribution configuration \(AWS CLI\)](#cli-create-ami-distribution-configuration)
+ [Update AMI distribution settings \(console\)](#update-ami-distribution-config-console)
+ [Update AMI distribution settings \(AWS CLI\)](#cli-update-ami-distribution-configuration)

## Create an AMI distribution configuration \(console\)<a name="create-ami-distribution-config-console"></a>

Distribution configurations include the output AMI name, specific Region settings for encryption, launch permissions, and AWS accounts, organizations, and organizational units \(OUs\) that can launch the output AMI, and license configurations\.

**To create a new AMI distribution configuration:**

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Distribution settings** from the navigation pane\. This shows a list of the distribution configurations that are created under your account\.

1. Choose **Create distribution settings** near the top of the **Distribution settings** panel\.

1. In the **Image type** section, choose the **Amazon Machine Image \(AMI\)** output type\.

1. In the **General** section, enter a **Name** for your distribution configuration, and optional description\.

1. In the **Region settings** section, enter the following details for each Region where you are distributing your AMI:

   1. The AMI is distributed to the current Region, by default\. The default Region is displayed as **Region 1** in the Region settings\. Some settings for this default Region are not open for editing\. For any Regions you add, you can choose a Region from the **Region** dropdown list\.

      The **Kms key** identifies which AWS KMS key is used to encrypt the image for distribution to the target Region\.

      Image Builder copies the AMI to the **Target accounts** you specify for the Region\.
**Prerequisite**  
To copy an image across accounts, you must create the `EC2ImageBuilderDistributionCrossAccountRole` role in all of the target accounts in the target Regions, and attach the [Ec2ImageBuilderCrossAccountDistributionAccess policy](security-iam-awsmanpol.md#sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess) managed policy to the role\.

      The **Output AMI name** is optional\. If you provide a name, the final output AMI name includes an appended timestamp of when the AMI is built\. If you do not specify a name, Image Builder appends the build timestamp to the recipe name\. This ensures unique AMI names for each build\.

      1. With AMI sharing, you can grant access for specified AWS Principals to launch instances from your AMI\. If you expand the **AMI sharing** section, you can enter the following details:
         + **Launch permissions** – Select **Private** if you want to keep your AMI private, and allow access for specific AWS Principals to launch an instance from your private AMI\. Select **Public** if you want to make your AMI public\. Any AWS Principal can launch an instance from your public AMI\.
         + **Principals** – You can grant access for the following types of AWS Principals to launch instances:
           + **AWS account** – Grant access to a specific AWS account
           + **Organizational unit \(OU\)** – Grant access to an OU, and all of its child entities\. Child entities include OUs and AWS accounts\.
           + **Organization** – Grant access to your AWS Organizations, and all of its child entities\. Child entities include OUs and AWS accounts\.

             First, select the Principal type\. Then enter the ID for the AWS Principal to which you want to grant access in the box to the right of the drop\-down list\. You can enter multiple IDs of different types\.

      1. You can expand the **License configuration** section to attach license configurations created with AWS License Manager to your Image Builder images\. License configurations contain licensing rules based on the terms of your enterprise agreements\. Image Builder automatically includes license configurations that were associated with your base AMI\.

      1. You can expand the **Launch template configuration** section to specify an EC2 launch template to use for launching instances from the AMI you create\.

         If you are using an EC2 launch template, you can instruct Image Builder to create a new version of your launch template that includes the latest AMI ID after the build completes\. To update the launch template, configure the settings as follows:
         + **Launch template name** – Select the name of the launch template that you want Image Builder to update\.
         + **Set the default version** – Select this check box to update the launch template default version to the new version\.

         To add another launch template configuration, choose **Add launch template configuration**\. You can have up to five launch template configurations per Region\.

   1. To add distribution settings for another Region, choose **Add Region**\.

1. Choose **Create settings** when you are done\.

## Create an AMI distribution configuration \(AWS CLI\)<a name="cli-create-ami-distribution-configuration"></a>

The following example shows how to use the create\-distribution\-configuration command to create a new distribution configuration for your AMI, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   Use your favorite file\-editing tool to create a JSON file with keys shown in one of the following examples, and values that are valid for your environment\. These examples define which AWS accounts, AWS Organizations or organizational units \(OUs\) have permission to launch the AMI you distribute to the specified Regions\. Name the file `create-ami-distribution-configuration.json`, for use in the next step:

------
#### [ Accounts ]

   This example distributes an AMI to two Regions, and specifies AWS accounts that have launch permissions in each Region\.

   ```
   {
       "name": "MyExampleAccountDistribution",
       "description": "Copies AMI to eu-west-1, and specifies accounts that can launch instances in each Region.",
       "distributions": [
           {
               "region": "us-west-2",
               "amiDistributionConfiguration": {
                   "name": "Name {{imagebuilder:buildDate}}",
                   "description": "An example image name with parameter references",
                   "amiTags": {
                       "KeyName": "Some Value"
                   },
                   "launchPermission": {
                       "userIds": [
                           "987654321012"
                       ]
                   }
               }
           },
           {
               "region": "eu-west-1",
               "amiDistributionConfiguration": {
                   "name": "My {{imagebuilder:buildVersion}} image {{imagebuilder:buildDate}}",
                   "amiTags": {
                       "KeyName": "Some value"
                   },
                   "launchPermission": {
                       "userIds": [
                           "100000000001"
                       ]
                   }
               }
           }
       ]
   }
   ```

------
#### [ Organizations and OUs ]

   This example distributes an AMI to the default Region, and specifies organization and OU launch permissions\.

   ```
   {
     "name": "MyExampleAWSOrganizationDistribution",
     "description": "Shares AMI with the Organization and OU",
     "distributions": [
       {
         "region": "us-west-2",
         "amiDistributionConfiguration": {
           "name": "Name {{ imagebuilder:buildDate }}",
           "launchPermission": {
             "organizationArns": [
               "arn:aws:organizations::123456789012:organization/o-myorganization123"
             ],
             "organizationalUnitArns": [
               "arn:aws:organizations::123456789012:ou/o-123example/ou-1234-myorganizationalunit"
             ]
           }
         }
       }
     ]
   }
   ```

------

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder create-distribution-configuration --cli-input-json file://create-ami-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [create\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-distribution-configuration.html) in the *AWS CLI Command Reference*\.

## Update AMI distribution settings \(console\)<a name="update-ami-distribution-config-console"></a>

You can change your AMI distribution settings using the Image Builder console\. The updated distribution settings are used for all automated and manual pipeline deployments going forward\. However, the changes you make do not apply to any resources that Image Builder has already distributed\. For example, if you have distributed an AMI to a Region that you later remove from your distribution, the AMI that was already distributed remains in that Region until you remove it manually\.

**Update AMI distribution configuration**

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Distribution settings** from the navigation pane\. This shows a list of the distribution configurations that are created under your account\.

1. To view details or update a distribution configuration, choose the **Configuration name** link\. This opens the detail view for the distribution settings\.
**Note**  
You can also select the check box next to the **Configuration name**, then choose **View details**\.

1. To edit distribution configuration, choose **Edit** from the upper right corner of the **Distribution details** section\. Some fields are locked, such as the **Name** of the distribution configuration, and the default **Region** that is displayed as **Region 1**\. For more information about the distribution configuration settings, see [Create an AMI distribution configuration \(console\)](#create-ami-distribution-config-console)\.

1. Choose **Save changes** when you are done\.

## Update AMI distribution settings \(AWS CLI\)<a name="cli-update-ami-distribution-configuration"></a>

The following example shows how to use the update\-distribution\-configuration command to update distribution settings for your AMI, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   Use your favorite file\-editing tool to create a JSON file with the keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `update-ami-distribution-configuration.json`\.

   ```
   {
       "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/update-ami-distribution-configuration.json",
       "description": "Copies AMI to eu-west-2, and specifies accounts that can launch instances in each Region.",
       "distributions": [
         {
               "region": "us-west-2",
               "amiDistributionConfiguration": {
                   "name": "Name {{imagebuilder:buildDate}}",
                   "description": "An example image name with parameter references",
                   "launchPermissions": {
                       "userIds": [
                           "987654321012"
                       ]
                   }
               }
           },
           {
               "region": "eu-west-2",
               "amiDistributionConfiguration": {
                   "name": "My {{imagebuilder:buildVersion}} image {{imagebuilder:buildDate}}",
                   "tags": {
                       "KeyName": "Some value"
                   },
                   "launchPermissions": {
                       "userIds": [
                           "100000000001"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder update-distribution-configuration --cli-input-json file://update-ami-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [update\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/update-distribution-configuration.html) in the *AWS CLI Command Reference*\. To update tags for your distribution configuration resource, see the [Tag resources](tag-resources.md) section\.