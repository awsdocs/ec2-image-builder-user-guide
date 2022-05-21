# Manage EC2 Image Builder distribution settings<a name="manage-distribution-settings"></a>

After you create distribution settings with Image Builder, you can manage them using the Image Builder console, the Image Builder API, or imagebuilder commands in the AWS CLI\. With distribution settings, you can perform the following actions:

**AMI distribution**
+ Specify the name and description of your output AMI\.
+ Authorize other AWS accounts, organizations, and OUs to launch the AMI from the owner's account\. The owner account is billed for charges that are associated with the AMI\.
**Note**  
To make an AMI public, set the launch permission authorized accounts to `all`\. See the examples for making an AMI public at EC2 [ModifyImageAttribute](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ModifyImageAttribute.html)\.
+ Create a copy of the output AMI for each of the specified target accounts, organizations, and OUs in the destination Region\. The target accounts, organizations, and OUs own their AMI copies, and are billed for any associated charges\. For more information about distributing your AMI to AWS Organizations and OUs, see [Share an AMI with organizations or OUs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/share-amis-with-organizations-and-OUs.html)\.
+ Replicate the AMI to the owner's account in other AWS Regions\.
+ Export the AMI to Amazon Simple Storage Service \(Amazon S3\)\.

**Container image distribution**
+ Specify the ECR repository where Image Builder stores the output image in the distribution Region\.

You can use your distribution settings in the following ways to deliver images to target Regions, accounts, AWS Organizations and organizational units \(OUs\) one time, or with every pipeline build:
+ To automatically deliver updated images to specified Regions, accounts, Organizations, and OUs, use distribution settings with an Image Builder pipeline that runs on a schedule\.
+ To create a new image and deliver it to the specified Regions, accounts, Organizations, and OUs, use distribution settings with an Image Builder pipeline that you run one time from the Image Builder console, using **Run pipeline** from the **Actions** menu\.
+ To create a new image and deliver it to the specified Regions, accounts, Organizations, and OUs, use distribution settings with the following API action or Image Builder command in the AWS CLI:
  + The [CreateImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImage.html) action in the Image Builder API\.
  + The [create\-image](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-image.html) command in the AWS CLI\.
+ To export virtual machine \(VM\) image disks to S3 buckets in target Regions as part of your regular image build process\.

**Tip**  
When you have multiple resources of the same type, tagging helps you to identify a specific resource based on the tags you've assigned to it\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

This topic covers how to list, view, and create distribution settings\.

**Topics**
+ [List and view distribution settings detail](distribution-settings-detail.md)
+ [Create and update AMI distribution configurations](cr-upd-ami-distribution-settings.md)
+ [Create and update distribution settings for container images](cr-upd-container-distribution-settings.md)
+ [Set up cross\-account AMI distribution with Image Builder](cross-account-dist.md)
+ [Configure AMI distribution settings to use an Amazon EC2 launch template](dist-using-launch-template.md)