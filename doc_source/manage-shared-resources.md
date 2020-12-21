# Share EC2 Image Builder resources<a name="manage-shared-resources"></a>

EC2 Image Builder integrates with AWS Resource Access Manager \(AWS RAM\) to allow you to share certain resources with any AWS account or through AWS Organizations\. EC2 Image Builder resources that can be shared are:
+ Components
+ Images
+ Recipes

This section provides information to help you share these EC2 Image Builder resources\. 

**Topics**
+ [Working with shared components, images, and recipes in EC2 Image Builder](#manage-shared-resources-using)
+ [Prerequisites for sharing components, images, and image recipes](#manage-shared-resources-prereqs)
+ [Related services](#manage-shared-resources-related-svcs)
+ [Sharing across Regions](#manage-shared-resources-regions)
+ [Sharing a component, image, or image recipe](#manage-shared-resources-share)
+ [Unsharing a shared component, image, or recipe](#manage-shared-resources-unshare)
+ [Identifying a shared component, image, or recipe](#manage-shared-resources-identify)
+ [Shared component, image, and recipe permissions](#manage-shared-resources-perms)
+ [Billing and metering](#manage-shared-resources-billing)
+ [Instance limits](#manage-shared-resources-limits)

## Working with shared components, images, and recipes in EC2 Image Builder<a name="manage-shared-resources-using"></a>

Component, image, and image recipe sharing enables resource owners to share software configurations with other AWS accounts or within an AWS organization\. You can manage resource sharing centrally, and define a set of accounts with which the configuration can be shared\.

In this model, the AWS account that owns the component, image, or image recipe \(owners\) shares it with other AWS accounts \(consumers\)\. Consumers can associate a shared component with their image pipelines to automatically consume updates to the shared component, image, or image recipe\.

A component, image, or image recipe owner can share these resources with:
+ Specific AWS accounts inside or outside of its organization in AWS Organizations\.
+ An organizational unit inside its organization in AWS Organizations\.
+ Its entire organization in AWS Organizations\.

## Prerequisites for sharing components, images, and image recipes<a name="manage-shared-resources-prereqs"></a>

To share a component, image, or image recipe:
+ You must own the component, image, or image recipe in your AWS account\. You cannot share resources that have been shared with you\.
+ The AWS Key Management Service \(AWS KMS\) CMK associated with encrypted resources must be explicitly shared with the target accounts\.
+ You must enable sharing with AWS Organizations to share these resources with your organization or an organizational unit in AWS Organizations\. 
+ You are responsible for ensuring that dependencies external to these resources, or underlying resources that are managed outside of AWS, are also shared with consumers\. EC2 Image Builder does not manage dependencies external to EC2 Image Builder\. This includes the Amazon EC2 AMIs associated with EC2 Image Builder images, which require account\-level sharing\. This can be configured in the distribution configuration associated with your Image Builder workflows\.

## Related services<a name="manage-shared-resources-related-svcs"></a>

**AWS Resource Access Manager**  
Component, image, and image recipe sharing integrates with AWS Resource Access Manager \(AWS RAM\)\. AWS RAM is a service that enables you to share your AWS resources with any AWS account or through AWS Organizations\. With AWS RAM, you share resources that you own by creating a resource share\. A resource share specifies the resources to share and the consumers with whom to share them\. Consumers can be individual AWS accounts, organizational units, or an entire organization in AWS Organizations\.

## Sharing across Regions<a name="manage-shared-resources-regions"></a>

Shared components, images, and image recipes can be shared only in a specified AWS Region\. When you share these resources, they will not replicate across Regions\.

## Sharing a component, image, or image recipe<a name="manage-shared-resources-share"></a>

To share a component, image, or image recipe, you must add it to a resource share\. A resource share is an AWS Resource Access Manager resource that lets you share your resources across AWS accounts\. A resource share specifies the resources to share and the consumers with whom they are shared\. To add the component, image, or image recipe to a new resource share, you must first create the resource share using the AWS Resource Access Manager console\.

If you are part of an organization in AWS Organizations and sharing within your organization is enabled, consumers in your organization are automatically granted access to the shared component, image, or image recipe\. Otherwise, consumers receive an invitation to join the resource share and are granted access to the shared resource after accepting the invitation\.

You can share a component, image, or recipe that you own using the AWS Resource Access Manager console or the AWS CLI\.

**To share a component, image, or recipe that you own using the AWS Resource Access Manager console**  
See [Creating a Resource Share in the AWS Resource Access Manager User Guide](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-create)\.

**To share a component, image, or image recipe that you own using the AWS CLI**  
Use the [https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) command\.

**Using resource\-based policies to share a component, image, or recipe**  
The `PutImagePolicy`, `PutComponentPolicy`, and `PutImageRecipePolicy` APIs provided by the Image Builder service allow you to define resource policies for images, components, and image recipes, respectively\. AWS RAM uses these APIs to define the correct resource policies to allow the consumer with whom a resource is shared to make API calls involving the shared resource\. For AWS RAM to set the correct policies to share and unshare a resource, the resource owner must have `imagebuilder:put*` permissions\.

### Apply a resource policy to an image<a name="image-builder-cli-apply-resource-policy-image"></a>

You can apply a resource policy to an image to allow other users to use the image in their image recipes\. For the command to be successful, you must ensure that the account with which you are sharing has permission to access the underlying resource \(for example, the Amazon EC2 AMI\)\. We recommend that you use the RAM CLI command [create\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) to share resources\. If you use the EC2 Image Builder CLI command [put\-image\-policy](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/put-image-policy.html), you must also use the RAM CLI command [promote\-resource\-share\-created\-from\-policy](https://docs.aws.amazon.com/cli/latest/reference/ram/promote-resource-share-created-from-policy.html) for the resource to be visible to all principals with whom the resource is shared\. For more information, see [Share EC2 Image Builder resources](#manage-shared-resources)\.

```
aws imagebuilder put-image-policy --image-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03/1 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": ["imagebuilder:GetImage", "imagebuilder:ListImages"] "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03/1" ] } ] }'
```

### Apply a resource policy to a component<a name="image-builder-cli-apply-resource-policy-component"></a>

You can apply a resource policy to a build component to enable cross\-account sharing of build components\. This command gives other accounts permission to use your build component in their image recipes\. For the command to be successful, you must ensure that the account with which you are sharing has permission to access any resources referenced by the shared build component, such as files hosted on private repositories\. We recommend that you use the RAM CLI command [create\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) to share resources\. If you use the EC2 Image Builder CLI command [put\-component\-policy](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/put-component-policy.html), you must also use the RAM CLI command [promote\-resource\-share\-created\-from\-policy](https://docs.aws.amazon.com/cli/latest/reference/ram/promote-resource-share-created-from-policy.html) for the resource to be visible to all principals with whom the resource is shared\. For more information, see [Share EC2 Image Builder resources](#manage-shared-resources)\.

```
aws imagebuilder put-component-policy --component-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03/1 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": [ "imagebuilder:GetComponent", "imagebuilder:ListComponents" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03/1" ] } ] }'
```

### Apply a resource policy to a recipe<a name="image-builder-cli-apply-resource-policy-recipe"></a>

You can apply a resource policy to an image recipe to enable cross\-account sharing of image recipes\. This command gives other accounts permission to use your image recipes to create images in their accounts\. For the command to be successful, you must ensure that the account with which you are sharing has permission to access any images or components referenced by the image recipe\. We recommend that you use the RAM CLI command [create\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) to share resources\. If you use the EC2 Image Builder CLI command [put\-image\-recipe\-policy](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/put-image-recipe-policy.html), you must also use the RAM CLI command [promote\-resource\-share\-created\-from\-policy](https://docs.aws.amazon.com/cli/latest/reference/ram/promote-resource-share-created-from-policy.html) for the resource to be visible to all principals with whom the resource is shared\. For more information, see [Share EC2 Image Builder resources](#manage-shared-resources)\.

```
aws imagebuilder put-image-recipe-policy --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image-recipe/2019.12.03 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": [ "imagebuilder:GetImageRecipe", "imagebuilder:ListImageRecipes" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image-recipe/2019.12.03" ] } ] }'
```

## Unsharing a shared component, image, or recipe<a name="manage-shared-resources-unshare"></a>

To unshare a shared component, image, or recipe that you own, you must remove it from the resource share\. You can do this using the AWS Resource Access Manager console or the AWS CLI\.

**Note**  
To unshare a component, image, or recipe, the consumer cannot have any dependencies on them\. The consumer must remove any dependencies on the shared resources before the owner can unshare them\.

**To unshare a shared component, image, or recipe that you own using the AWS Resource Access Manager console**  
See [Updating a Resource Share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-update) in the AWS Resource Access Manager User Guide\.

**To unshare a shared component, image, or recipe that you own using the AWS CLI**  
Use the [https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html](https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html) command\.

## Identifying a shared component, image, or recipe<a name="manage-shared-resources-identify"></a>

Owners and consumers can identify shared components, images, and image recipes using the AWS CLI\.

**Identify a shared component**  
Use the [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListComponents.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListComponents.html) command\. The command returns the components that you own and the components that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetComponent.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetComponent.html) shows the AWS account ID of the component owner\.

**Identify a shared image**  
Use the [ `list-images`](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImages.html) command\. The command returns the images that you own and images that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html) shows the AWS account ID of the image owner\.

**Identify a shared container image**  
Use the [ `list-images`](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImages.html) command\. The command returns the images that you own and images that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html) shows the AWS account ID of the image owner\.

**Identify a shared image recipe**  
Use the [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImageRecipes.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImageRecipes.html) command\. The command returns the image recipes that you own and image recipes that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImageRecipe.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImageRecipe.html) shows the AWS account ID of the image recipe owner\.

**Identify a shared container recipe**  
Use the [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListContainerRecipes.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListContainerRecipes.html) command\. The command returns the container recipes that you own and container recipes that are shared with you\. [https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetContainerRecipe.html](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetContainerRecipe.html) shows the AWS account ID of the container recipe owner\.

## Shared component, image, and recipe permissions<a name="manage-shared-resources-perms"></a>

**Permissions for owners**  
Owners cannot delete a shared component, image, or image recipe until it is no longer shared\. An owner cannot unshare these resources until none of the consumers depend on them\.

**Permissions for consumers**  
Consumers can read a component, image, or image recipe, but cannot modify them in any way\. They cannot view or modify these resources if they are owned by other consumers or the owner of the resource\. Consumers can use shared components and images in image recipes to create custom images\. Consumers can use shared image recipes to create their own custom images\.

## Billing and metering<a name="manage-shared-resources-billing"></a>

There is no charge to use EC2 Image Builder\.

## Instance limits<a name="manage-shared-resources-limits"></a>

Shared components, images, and image recipes count toward the corresponding resource limits of the owner only\. The resource limits of the consumers are not affected by the resources that have been shared with them\.