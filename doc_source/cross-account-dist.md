# Set up cross\-account AMI distribution with Image Builder<a name="cross-account-dist"></a>

This section describes how you can configure distribution settings to deliver an Image Builder AMI to other accounts that you specify\.

The destination account can then launch or modify the AMI, as needed\.

## Prerequisites<a name="cross-account-dist-prereqs"></a>

To ensure that target accounts can successfully launch instances from your Image Builder image, you must configure the appropriate permissions for all destination accounts in all Regions\.

To configure cross\-account distribution permissions in IAM, follow these steps:

1. Create a new IAM role in all of the destination accounts called `EC2ImageBuilderDistributionCrossAccountRole`\.

1. Attach the [Ec2ImageBuilderCrossAccountDistributionAccess policy](security-iam-awsmanpol.md#sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess) to the role\. For more information about managed policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *AWS Identity and Access Management User Guide*\.

1. Verify that the source account ID is added to the trust policy attached to the IAM role of the destination account\. For more information about trust policies, see [Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based) in the *AWS Identity and Access Management User Guide*\.

1. If you are using `launchTemplateConfigurations` to specify an Amazon EC2 launch template, you must also add the following policy to your `EC2ImageBuilderDistributionCrossAccountRole` in each destination account and Region\.

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

**Note**  
AWS CLI command examples in this section assume you have previously created image recipe and infrastructure configuration JSON files\.

## Limits for cross\-account distribution<a name="cross-account-dist-limits"></a>

There are some limitations when distributing Image Builder images across accounts:
+ The destination account is limited to 50 concurrent AMI copies per destination Region\.
+ If you want to copy a paravirtual \(PV\) virtualization AMI to another Region, the destination Region must support PV virtualization AMIs\. For more information, see [Linux AMI virtualization types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html)\.
+ You cannot create an unencrypted copy of an encrypted snapshot\. If you do not specify a AWS Key Management Service \(AWS KMS\) customer\-managed key for the `KmsKeyId` parameter, Image Builder uses the default key for Amazon EBS\. For more information, see [Amazon EBS Encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) in the *Amazon Elastic Compute Cloud User Guide*\.

For more information, see [CreateDistributionConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateDistributionConfiguration.html) in the *EC2 Image Builder API Reference*\.

## Configure cross\-account distribution for an Image Builder AMI \(console\)<a name="cross-account-dist-console-create-ami"></a>

This section describes how to create and configure distribution settings for cross\-account distribution of your Image Builder AMIs using the AWS Management Console\. Configuring cross\-account distribution requires specific IAM permissions\. You must complete the [Prerequisites](#cross-account-dist-prereqs) for this section before you continue\.

To create distribution settings in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Distribution settings** from the navigation pane\. This shows a list of the distribution settings that are created under your account\.

1. At the top of the **Distribution settings** page, choose **Create distribution settings**\. This takes you to the **Create distribution settings** page\.

1. In the **Image type** section, choose **Amazon Machine Image \(AMI\)** as the **Output type**\. This is the default setting\.

1. In the **General** section, enter the **Name** of the distribution settings resource you wish to create \(*required*\)\.

1. In the **Region settings** section, enter a 12\-digit account ID that you want to distribute your AMI to in **Target accounts** for the selected Region, and press **Enter**\. This checks for the correct formatting, and then displays the account ID you entered below the box\. Repeat the process to add more accounts\.

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