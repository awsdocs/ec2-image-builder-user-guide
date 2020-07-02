# Resource Sharing in EC2 Image Builder<a name="image-builder-resource-sharing"></a>

EC2 Image Builder integrates with AWS Resource Access Manager \(AWS RAM\) to allow you to share certain resources with any AWS account or through AWS Organizations\. EC2 Image Builder resources that can be shared are:
+ Components
+ Images
+ Image recipes

This section provides information to help you share these EC2 Image Builder resources\. 

**Topics**
+ [Working with Shared Components, Images, and Image Recipes in EC2 Image Builder](#image-builder-working-shared-resources)
+ [Prerequisites for Sharing Components, Images, and Image Recipes](#image-builder-shared-resources-prereqs)
+ [Related Services](#image-builder-shared-resources-related-services)
+ [Sharing Across Regions](#image-builder-shared-resources-regions)
+ [Sharing a Component, Image, or Image Recipe](#image-builder-shared-resources-sharing)
+ [Unsharing a Shared Component, Image, or Image Recipe](#image-builder-shared-resources-unsharing)
+ [Identifying a Shared Component, Image, or Image Recipe](#image-builder-shared-resources-identifying)
+ [Shared Component, Image, and Image Recipe Permissions](#image-builder-shared-resources-permissions)
+ [Billing and Metering](#image-builder-shared-resources-billing)
+ [Instance Limits](#image-builder-shared-resources-limits)

## Working with Shared Components, Images, and Image Recipes in EC2 Image Builder<a name="image-builder-working-shared-resources"></a>

Component, image, and image recipe sharing enables resource owners to share software configurations with other AWS accounts or within an AWS organization\. You can manage resource sharing centrally, and define a set of accounts with which the configuration can be shared\. 

In this model, the AWS account that owns the component, image, or image recipe \(owners\) shares it with other AWS accounts \(consumers\)\. Consumers can associate a shared component with their image pipelines to automatically consume updates to the shared component, image, or image recipe\. 

A component, image, or image recipe owner can share these resources with:
+ Specific AWS accounts inside or outside of its organization in AWS Organizations
+ An organizational unit inside its organization in AWS Organizations
+ Its entire organization in AWS Organizations

## Prerequisites for Sharing Components, Images, and Image Recipes<a name="image-builder-shared-resources-prereqs"></a>

To share a component, image, or image recipe:
+ You must own the component, image, or image recipe in your AWS account\. You cannot share resources that have been shared with you\. 
+ The AWS Key Management Service CMK associated with encrypted resources must be explicitly shared with the target accounts\.
+ You must enable sharing with AWS Organizations to share these resources with your organization or an organizational unit in AWS Organizations\. 
+ You are responsible for ensuring that dependencies external to these resources, or underlying resources that are managed outside of AWS, are also shared with consumers\. EC2 Image Builder does not manage dependencies external to EC2 Image Builder\. This includes the Amazon EC2 AMIs associated with EC2 Image Builder images, which require account\-level sharing\. This can be configured in the distribution configuration associated with your Image Builder workflows\.

## Related Services<a name="image-builder-shared-resources-related-services"></a>

**AWS Resource Access Manager**  
Component, image, and image recipe sharing integrates with AWS Resource Access Manager \(AWS RAM\)\. AWS RAM is a service that enables you to share your AWS resources with any AWS account or through AWS Organizations\. With AWS RAM, you share resources that you own by creating a resource share\. A resource share specifies the resources to share and the consumers with whom to share them\. Consumers can be individual AWS accounts, organizational units, or an entire organization in AWS Organizations\.

## Sharing Across Regions<a name="image-builder-shared-resources-regions"></a>

Shared components, images, and image recipes can only be shared in a specified AWS Region\. When you share these resources, they will not replicate across Regions\.

## Sharing a Component, Image, or Image Recipe<a name="image-builder-shared-resources-sharing"></a>

To share a component, image, or image recipe, you must add it to a resource share\. A resource share is an AWS Resource Access Manager resource that lets you share your resources across AWS accounts\. A resource share specifies the resources to share and the consumers with whom they are shared\. To add the component, image, or image recipe to a new resource share, you must first create the resource share using the AWS Resource Access Manager console\.

If you are part of an organization in AWS Organizations and sharing within your organization is enabled, consumers in your organization are automatically granted access to the shared component, image, or image recipe\. Otherwise, consumers receive an invitation to join the resource share and are granted access to the shared resource after accepting the invitation\.

You can share a component, image, or image recipe that you own using the AWS Resource Access Manager console or the AWS CLI\.

**To share a component, image, or image recipe that you own using the AWS Resource Access Manager console**  
See [Creating a Resource Share in the AWS Resource Access Manager User Guide](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-create)\.

**To share a component, image, or image recipe that you own using the AWS CLI**  
Use the [https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) command\.

**Using resource\-based policies to share a component, image, or image recipe**  
The `PutImagePolicy`, `PutComponentPolicy`, and `PutImageRecipePolicy` APIs provided by the Image Builder service allow you to define resource policies for images, components, and image recipes, respectively\. AWS RAM leverages these APIs to define the correct resource policies to allow the consumer with whom a resource is shared to make API calls involving the shared resource\. In order for AWS RAM to set the correct policies to share and unshare a resource , the resource owner must have `imagebuilder:put*` permissions\. 

## Unsharing a Shared Component, Image, or Image Recipe<a name="image-builder-shared-resources-unsharing"></a>

To unshare a shared component, image, or image recipe that you own, you must remove it from the resource share\. You can do this using the AWS Resource Access Manager console or the AWS CLI\.

**Note**  
To unshare a component, image, or image recipe, the consumer cannot have any dependencies on them\. The consumer must remove any dependencies on the shared resources before the owner can unshare them\.

**To unshare a shared component, image, or image recipe that you own using the AWS Resource Access Manager console**  
See [Updating a Resource Share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-update) in the AWS Resource Access Manager User Guide\.

**To unshare a shared component, image, or image recipe that you own using the AWS CLI**  
Use the [https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html](https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html) command\.

## Identifying a Shared Component, Image, or Image Recipe<a name="image-builder-shared-resources-identifying"></a>

Owners and consumers can identify shared components, images, and image recipes using the AWS CLI\.

**Identify a Shared Component**  
Use the [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListComponents.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListComponents.html) command\. The command returns the components that you own and the components that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetComponent.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetComponent.html) shows the AWS account ID of the component owner\. 

**Identify a Shared Image**  
Use the [ `list-images`](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImages.html) command\. The command returns the images that you own and images that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html) shows the AWS account ID of the image owner\. 

**Identify a Shared Image Recipe**  
Use the [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImageRecipes.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImageRecipes.html) command\. The command returns the image recipes that you own and image recipes that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImageRecipe.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImageRecipe.html) shows the AWS account ID of the image recipe owner\. 

## Shared Component, Image, and Image Recipe Permissions<a name="image-builder-shared-resources-permissions"></a>

**Permissions for Owners**  
Owners cannot delete a shared component, image, or image recipe until it is no longer shared\. An owner cannot unshare these resources until none of the consumers depend on them\. 

**Permissions for Consumers**  
Consumers can only read a component, image, or image recipe\. Consumers cannot modify them in any way, and they cannot view or modify these resources if they are owned by other consumers or the owner of the resource\. Consumers can use shared components and images in image recipes to create golden images\. Consumers can use shared image recipes to create golden images\.

## Billing and Metering<a name="image-builder-shared-resources-billing"></a>

There is no charge to use EC2 Image Builder\. 

## Instance Limits<a name="image-builder-shared-resources-limits"></a>

Shared components, images, and image recipes count towards only the corresponding resource limits of the owner\. The resource limits of the consumers are not affected by the resources that have been shared with them\. 