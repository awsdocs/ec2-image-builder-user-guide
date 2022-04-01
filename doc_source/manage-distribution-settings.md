# Manage EC2 Image Builder distribution settings<a name="manage-distribution-settings"></a>

After you create distribution settings with Image Builder, you can manage them using the Image Builder console, the Image Builder API, or imagebuilder commands in the AWS CLI\.

You can use your distribution settings in the following ways to deliver images to target Regions and accounts one time, or with every pipeline build:
+ To automatically deliver updated images to specified Regions and accounts, use distribution settings with an Image Builder pipeline that runs on a schedule\.
+ To create a new image and deliver it to the specified Regions and accounts, use the distribution configuration as input to one of the following commands:
  + The [create\-image](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-image.html) command in the AWS CLI\.
  + The [CreateImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImage.html) action in the Image Builder API\.
+ To export virtual machine \(VM\) image disks to S3 buckets in target Regions as part of your regular image build process\.

**Tip**  
When you have multiple resources of the same type, tagging helps you to identify a specific resource based on the tags you've assigned to it\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

This section covers how to list, view, and create distribution settings\.

**Topics**
+ [List and view distribution settings detail](distribution-settings-detail.md)
+ [Create and update distribution settings for AMIs](crud-ami-distribution-settings.md)
+ [Create and update distribution settings for container images](crud-container-distribution-settings.md)
+ [Set up cross\-account AMI distribution with Image Builder](cross-account-dist.md)
+ [Configure AMI distribution settings to use an Amazon EC2 launch template](dist-using-launch-template.md)