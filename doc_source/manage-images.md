# Manage EC2 Image Builder images<a name="manage-images"></a>

After you have created AMI or container images with Image Builder, you can manage them using the Image Builder console, through the Image Builder API, or with imagebuilder commands in the AWS CLI\.

**Tip**  
When you have multiple resources of the same type, tagging helps you to identify a specific resource based on the tags you've assigned to it\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

This section covers how to list, view, and create images\.

**Topics**
+ [List and view image details](image-details.md)
+ [Create images](create-images.md)
+ [Cancel an image creation \(AWS CLI\)](#image-builder-cli-cancel-image-creation)
+ [Clean up resources](#images-cleanup)

## Cancel an image creation \(AWS CLI\)<a name="image-builder-cli-cancel-image-creation"></a>

To cancel an in\-progress image build, use the imagebuilder cancel\-image\-creation command\.

```
aws imagebuilder cancel-image-creation --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-recipe/2019.12.03/1
```

## Clean up resources<a name="images-cleanup"></a>

To avoid unexpected charges, make sure to clean up resources and pipelines that you created from the examples in this guide\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.