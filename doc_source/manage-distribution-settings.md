# Manage EC2 Image Builder distribution settings<a name="manage-distribution-settings"></a>

After you create distribution settings with Image Builder, you can manage them using the Image Builder console, the Image Builder API, or imagebuilder commands in the AWS CLI\.

You can use your distribution settings in the following ways to deliver images to target Regions and accounts one time, or with every pipeline build:
+ To automatically deliver updated images to specified Regions and accounts, use distribution settings with an Image Builder pipeline that runs on a schedule\.
+ To copy an image to the specified Regions and accounts, use distribution settings with the Image Builder API [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CopyImage.html) action\.
+ To create a new image and deliver it to the specified Regions and accounts, use distribution settings with the []()create\-image command using the AWS CLI\.

This section covers how to list, view, and create distribution settings\.

**Topics**
+ [List and view distribution settings detail](distribution-settings-detail.md)
+ [Create and update distribution settings](create-distribution-settings.md)
+ [Set up cross\-account AMI distribution with Image Builder](cross-account-dist.md)
+ [Configure AMI distribution settings to use an Amazon EC2 launch template](dist-using-launch-template.md)