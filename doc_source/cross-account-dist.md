# Set up cross\-account AMI distribution with Image Builder<a name="cross-account-dist"></a>

This section describes how you can configure distribution settings to deliver an Image Builder AMI to other accounts that you specify\.

The destination account can then launch or modify the AMI, as needed\.

**Note**  
AWS CLI command examples in this section assume that you have previously created image recipe and infrastructure configuration JSON files\. To create the JSON file for an image recipe, see [Create an image recipe \(AWS CLI\)](create-image-recipes.md#create-image-recipe-cli)\. To create the JSON file for an infrastructure configuration, see [Create an infrastructure configuration \(AWS CLI\)](create-infra-config.md#cli-create-infrastructure-configuration)\.

## Prerequisites<a name="cross-account-dist-prereqs"></a>

To ensure that target accounts can successfully launch instances from your Image Builder image, you must configure the appropriate permissions for all destination accounts in all Regions\.

If you encrypt your AMI using the AWS Key Management Service \(AWS KMS\), you must configure an AWS KMS key for your account that is used to encrypt the new image\.

When Image Builder performs cross\-account distribution for encrypted AMIs, the image in the source account is decrypted and pushed to the target Region, where it is re\-encrypted using the designated key for that Region\. Because Image Builder acts on behalf of the target account, and uses an IAM role that you create in the destination Region, that account must have access to keys in both the source and destination Regions\.

### Encryption keys<a name="cross-account-prereqs-encryption"></a>

The following prerequisites are required if your image is encrypted using AWS KMS\. IAM prerequisites are covered in the next section\.

**Source account requirements**
+ Create a KMS key in your account in all Regions where you build and distribute your AMI\. You can also use an existing key\.
+ Update the key policy for all of those keys to allow destination accounts to use your key\.

**Destination account requirements**
+ Add an inline policy to `EC2ImageBuilderDistributionCrossAccountRole` that allows the role to perform the required actions to distribute an encrypted AMI\. For IAM configuration steps, see the [IAM policies](#cross-account-prereqs-iam) prerequisites section\.

For more information about cross\-account access using AWS KMS, see [Allowing users in other accounts to use a KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying-external-accounts.html) in the *AWS Key Management Service Developer Guide*\.

Specify your encryption key in the image recipe, as follows:
+ If you are using the Image Builder console, choose your encryption key from the **Encryption \(KMS alias\)** dropdown list in the **Storage \(volumes\)** section of your recipe\.
+ If you are using the CreateImageRecipe API action, or the create\-image\-recipe command in the AWS CLI, configure your key in the `ebs` section under `blockDeviceMappings` in your JSON input\.

  The following JSON snippet shows encryption settings for an image recipe\. In addition to providing your encryption, you must also set the `encrypted` flag to `true`\.

  ```
  {
     ...
     "blockDeviceMappings": [ 
        { 
           "deviceName": "Example root volume",
           "ebs": { 
              "deleteOnTermination": true,
              "encrypted": true,
              "iops": 100,
              "kmsKeyId": "image-owner-key-id",
              ...
           },
           ...
        }
     ],
     ...
  }
  ```

### IAM policies<a name="cross-account-prereqs-iam"></a>

To configure cross\-account distribution permissions in AWS Identity and Access Management \(IAM\), follow these steps:

1. To use Image Builder AMIs that are distributed across accounts, the destination account owner must create a new IAM role in their account called `EC2ImageBuilderDistributionCrossAccountRole`\.

1. They must attach the [Ec2ImageBuilderCrossAccountDistributionAccess policy](security-iam-awsmanpol.md#sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess) to the role to enable cross\-account distribution\. For more information about managed policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *AWS Identity and Access Management User Guide*\.

1. Verify that the source account ID is added to the trust policy attached to the IAM role of the destination account\. For more information about trust policies, see [Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based) in the *AWS Identity and Access Management User Guide*\.

1. If the AMI you distribute is encrypted, the destination account owner must add the following inline policy to the `EC2ImageBuilderDistributionCrossAccountRole` in their account so that they can use your KMS keys\. The `Principal` section contains their account number\. This enables Image Builder to act on their behalf when it uses AWS KMS to encrypt and decrypt the AMI with the appropriate keys for each Region\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
       	{
   	    	"Sid": "AllowRoleToPerformKMSOperationsOnBehalfOfTheDestinationAccount",
   		    "Effect": "Allow",
   		    "Action": [
   		        "kms:Encrypt",
   		        "kms:Decrypt",
   		        "kms:ReEncrypt*",
   		        "kms:GenerateDataKey*",
   		        "kms:DescribeKey",
   		        "kms:CreateGrant",
   		        "kms:ListGrants",
   		        "kms:RevokeGrant"
   		    ],
   		    "Resource": "*"
   		}
   	]
   }
   ```

   For more information about inline policies, see [Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#inline-policies) in the *AWS Identity and Access Management User Guide*\.

1. If you are using `launchTemplateConfigurations` to specify an Amazon EC2 launch template, you must also add the following policy to your `EC2ImageBuilderDistributionCrossAccountRole` in each destination account\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:CreateLaunchTemplateVersion",
                   "ec2:ModifyLaunchTemplate"
               ],
               "Resource": "*",
               "Condition": {
                   "StringEquals": {
                       "aws:ResourceTag/CreatedBy": "EC2 Image Builder"
                   }
               }
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:DescribeLaunchTemplates"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:CreateTags"
               ],
               "Resource": "arn:aws:ec2:*:*:launch-template/*",
               "Condition": {
                   "StringEquals": {
                       "aws:RequestTag/CreatedBy": "EC2 Image Builder"
                   }
               }
           }
       ]
   }
   ```

## Limits for cross\-account distribution<a name="cross-account-dist-limits"></a>

There are some limitations when distributing Image Builder images across accounts:
+ The destination account is limited to 50 concurrent AMI copies for each destination Region\.
+ If you want to copy a paravirtual \(PV\) virtualization AMI to another Region, the destination Region must support PV virtualization AMIs\. For more information, see [Linux AMI virtualization types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html)\.
+ You cannot create an unencrypted copy of an encrypted snapshot\. If you do not specify an AWS Key Management Service \(AWS KMS\) customer managed key for the `KmsKeyId` parameter, Image Builder uses the default key for Amazon Elastic Block Store \(Amazon EBS\)\. For more information, see [Amazon EBS Encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) in the *Amazon Elastic Compute Cloud User Guide*\.

For more information, see [CreateDistributionConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateDistributionConfiguration.html) in the *EC2 Image Builder API Reference*\.

## Configure cross\-account distribution for an Image Builder AMI \(console\)<a name="cross-account-dist-console-create-ami"></a>

This section describes how to create and configure distribution settings for cross\-account distribution of your Image Builder AMIs using the AWS Management Console\. Configuring cross\-account distribution requires specific IAM permissions\. You must complete the [Prerequisites](#cross-account-dist-prereqs) for this section before you continue\.

To create distribution settings in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Distribution settings** from the navigation pane\. This shows a list of the distribution settings that are created under your account\.

1. At the top of the **Distribution settings** page, choose **Create distribution settings**\. This takes you to the **Create distribution settings** page\.

1. In the **Image type** section, choose **Amazon Machine Image \(AMI\)** as the **Output type**\. This is the default setting\.

1. In the **General** section, enter the **Name** of the distribution settings resource that you want to create \(*required*\)\.

1. In the **Region settings** section, enter a 12\-digit account ID that you want to distribute your AMI to in **Target accounts** for the selected Region, and press **Enter**\. This checks for the correct formatting, and then displays the account ID that you entered below the box\. Repeat the process to add more accounts\.

   To remove an account that you entered, choose the **X** displayed to the right of the account ID\.

   Enter the **Output AMI name** for each Region\.

1. Continue specifying any additional settings that you require, and choose **Create settings** to create your new distribution settings resource\.

## Configure cross\-account distribution for an Image Builder AMI \(AWS CLI\)<a name="cross-account-dist-cli-ami-create"></a>

This section describes how to configure a distribution settings file and use the create\-image command in the AWS CLI to build and distribute an Image Builder AMI across accounts\.

Configuring cross\-account distribution requires specific IAM permissions\. You must complete the [Prerequisites](#cross-account-dist-prereqs) for this section before you run the create\-image command\.

1. 

**Configure a distribution settings file**

   Before you use the create\-image command in the AWS CLI to create an Image Builder AMI that is distributed to another account, you must create a `DistributionConfiguration` JSON structure that specifies the target account IDs in the `AmiDistributionConfiguration` settings\. You must specify at least one `AmiDistributionConfiguration` in the source Region\.

   The following sample file, named `create-distribution-configuration.json`, shows configuration for cross\-account image distribution in the source Region\.

   ```
   {
       "name": "cross-account-distribution-example",
       "description": "Cross Account Distribution Configuration Example",
       "distributions": [
           {
               "amiDistributionConfiguration": {
                   "targetAccountIds": ["123456789012", "987654321098"],
                   "name": "Name {{ imagebuilder:buildDate }}", 
                   "description": "ImageCopy Ami Copy Configuration"
               }, 
               "region": "us-west-2"
           }
       ]
   }
   ```

1. 

**Create the distribution settings**

   To create an Image Builder distribution settings resource using the [create\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-distribution-configuration.html) command in the AWS CLI, provide the following parameters in the command:
   + Enter the name of the distribution in the `--name` parameter\.
   + Attach the distribution configuration JSON file you created in the `--cli-input-json` parameter\.

   ```
   aws imagebuilder create-distribution-configuration --name my distribution name --cli-input-json file://create-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

*You can also provide JSON directly in the command, using the `--distributions` parameter\.*