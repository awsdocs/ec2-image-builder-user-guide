# Manage EC2 Image Builder images<a name="manage-images"></a>

After you have created image resources for AMI or container images with Image Builder, you can manage them using the Image Builder console, through the Image Builder API, or with imagebuilder commands in the AWS CLI\.

**Tip**  
When you have multiple resources of the same type, tagging helps you to identify a specific resource based on the tags you've assigned to it\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

This section covers how to list, view, and create images\.

**Topics**
+ [List images and build versions](image-details-list.md)
+ [View image details](view-image-details.md)
+ [Create images](create-images.md)
+ [Import a VM image](import-vm-image.md)
+ [Manage security findings for Image Builder images](image-security-findings.md)
+ [Clean up resources](#images-cleanup)

## Clean up resources<a name="images-cleanup"></a>

To avoid unexpected charges, make sure to clean up resources and pipelines that you created from the examples in this guide\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.