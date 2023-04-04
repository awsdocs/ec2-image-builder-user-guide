# Share EC2 Image Builder resources<a name="manage-shared-resources"></a>

EC2 Image Builder integrates with AWS Resource Access Manager \(AWS RAM\) to allow you to share certain resources with any AWS account or through AWS Organizations\. EC2 Image Builder resources that can be shared are:
+ Components
+ Images
+ Recipes

This section provides information to help you share these EC2 Image Builder resources\. 

**Topics**
+ [Working with shared components, images, and recipes in EC2 Image Builder](#manage-shared-resources-using)
+ [Prerequisites for sharing components, images, and recipes](#manage-shared-resources-prereqs)
+ [Related services](#manage-shared-resources-related-svcs)
+ [Sharing across Regions](#manage-shared-resources-regions)
+ [Sharing a component, image, or recipe](#manage-shared-resources-share)
+ [Unsharing a shared component, image, or recipe](#manage-shared-resources-unshare)
+ [Identifying a shared component, image, or recipe](#manage-shared-resources-identify)
+ [Shared component, image, and recipe permissions](#manage-shared-resources-perms)
+ [Billing and metering](#manage-shared-resources-billing)
+ [Resource limits](#manage-shared-resources-limits)

## Working with shared components, images, and recipes in EC2 Image Builder<a name="manage-shared-resources-using"></a>

Component, image, and recipe sharing enables resource owners to share software configurations with other AWS accounts or within an AWS organization\. You can manage resource sharing centrally, and define a set of accounts with which the configuration can be shared\.

In this model, the AWS account that owns the component, image, or recipe \(owners\) shares it with other AWS accounts \(consumers\)\. Consumers can associate a shared component with their image pipelines to automatically consume updates to the shared component, image, or recipe\.

A component, image, or recipe owner can share these resources with:
+ Specific AWS accounts inside or outside of its organization in AWS Organizations\.
+ An organizational unit \(OU\) inside of its organization in AWS Organizations\.
+ Its entire organization in AWS Organizations\.
+ AWS Organizations or OUs outside of its organization\.

## Prerequisites for sharing components, images, and recipes<a name="manage-shared-resources-prereqs"></a>

To share an Image Builder component, image, or recipe:
+ You must own the component, image, or recipe in your AWS account\. You cannot share resources that have been shared with you\.
+ The AWS Key Management Service \(AWS KMS\) key associated with encrypted resources must be explicitly shared with the target accounts, organizations, or OUs\.
+ In order to share your Image Builder resources with AWS Organizations and OUs using AWS RAM, you must enable sharing\. For more information, see [Enable Sharing with AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html) in the *AWS RAM User Guide*\.
+ If you distribute an image encrypted with AWS KMS across accounts in different Regions, you must create a KMS key and alias in each target Region\. Additionally, the people who will be launching instances in those Regions will need access to the KMS key specified via the Key Policy\.

The following resources that Image Builder creates from your pipeline build are not considered Image Builder resources – rather, they are external resources that Image Builder distributes in your account, and to the AWS Regions, accounts, and organizations or organizational units \(OUs\) that you specify in your distribution configuration\.
+ Amazon Machine Images \(AMIs\)
+ Container images that reside in Amazon ECR

For more information about distribution settings for your AMI, see [Create and update AMI distribution configurations](cr-upd-ami-distribution-settings.md)\. For more information about distribution settings for your container image in Amazon ECR, see [Create and update distribution settings for container images](cr-upd-container-distribution-settings.md)\.

For more information about sharing your AMI with AWS Organizations and OUs, see [Share an AMI with organizations or OUs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/share-amis-with-organizations-and-OUs.html)\. 

## Related services<a name="manage-shared-resources-related-svcs"></a>

**AWS Resource Access Manager**  
Component, image, and recipe sharing integrate with AWS Resource Access Manager \(AWS RAM\)\. AWS RAM is a service that enables you to share your AWS resources with any AWS account or through AWS Organizations\. With AWS RAM, you share resources that you own by creating a resource share\. A resource share specifies the resources to share and the consumers with whom to share them\. Consumers can be individual AWS accounts, organizational units, or an entire organization in AWS Organizations\.

For more information about AWS RAM, see the [https://docs.aws.amazon.com/ram/latest/userguide/what-is.html](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html)\.

## Sharing across Regions<a name="manage-shared-resources-regions"></a>

Shared components, images, and recipes can be shared only in a specified AWS Region\. When you share these resources, they will not replicate across Regions\.

## Sharing a component, image, or recipe<a name="manage-shared-resources-share"></a>

To share an Image Builder component, image, or recipe, you must add it to a resource share\. A resource share is an AWS RAM resource that lets you share your resources across AWS accounts\. A resource share specifies the resources to share and the consumers with whom they are shared\. To add the component, image, or recipe to a new resource share, you must first create the resource share using the AWS RAM console\.

If you are part of an organization in AWS Organizations and sharing within your organization is enabled, consumers in your organization are automatically granted access to the shared component, image, or recipe\. Otherwise, consumers receive an invitation to join the resource share and are granted access to the shared resource after accepting the invitation\.

The following options are available for sharing your resources:\.

### Option 1: Create a RAM resource share<a name="share-opt1-create-resource-share"></a>

When you create a RAM resource share, you can share a component, image, or recipe that you own in a single step\. Use one of the following methods to create your resource share:
+ 

**Console**  
To create your resource share using the AWS RAM console, see [Share AWS resources owned by you](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-create) in the *AWS RAM User Guide*\.
+ 

**AWS CLI**  
To create your resource share using the AWS RAM command line interface, run the [create\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/create-resource-share.html) command in the AWS CLI\.

### Option 2: Apply a resource policy and promote to a RAM resource share<a name="share-opt2-promote-resource-share"></a>

The second option for sharing your resources involves two steps, running commands in the AWS CLI for both\. The first step uses Image Builder commands in the AWS CLI to apply resource\-based policies to the shared resource\. The second step promotes the resource to a RAM resource share using the [promote\-resource\-share\-created\-from\-policy](https://docs.aws.amazon.com/cli/latest/reference/ram/promote-resource-share-created-from-policy.html) AWS RAM command in the AWS CLI to ensure that the resource is visible to all principals with whom you've shared it\.

1. 

**Apply the resource policy**

   To successfully apply the resource policy, you must ensure that the account with which you are sharing has permission to access any underlying resources\.

   Choose the tab that matches your resource type for the applicable command\.

------
#### [ Image ]

   You can apply a resource policy to an image, to allow others to use it as the base image in their recipes\. 

   Run the [put\-image\-policy](https://docs.aws.amazon.com/cli/latest/reference//imagebuilder/put-image-policy.html) Image Builder command in the AWS CLI, to identify the AWS principals to share the image with\.

   ```
   aws imagebuilder put-image-policy --image-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03/1 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": ["imagebuilder:GetImage", "imagebuilder:ListImages"], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03/1" ] } ] }'
   ```

------
#### [ Component ]

   You can apply a resource policy to a build or test component to enable cross\-account sharing\. This command gives other accounts permission to use your component in their recipes\. To successfully apply the resource policy, you must ensure that the account with which you are sharing has permission to access any resources referenced by the shared component, such as files hosted on private repositories\.

   Run the [put\-component\-policy](https://docs.aws.amazon.com/cli/latest/reference//imagebuilder/put-component-policy.html) Image Builder command in the AWS CLI, to identify the AWS principals to share the component with\.

   ```
   aws imagebuilder put-component-policy --component-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03/1 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": [ "imagebuilder:GetComponent", "imagebuilder:ListComponents" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03/1" ] } ] }'
   ```

------
#### [ Image recipe ]

   You can apply a resource policy to an image recipe to enable cross\-account sharing\. This command gives other accounts permission to use your recipe to create images in their accounts\. To successfully apply the resource policy, you must ensure that the account with which you are sharing has permission to access any resources that the recipe references, such as the base image, or selected components\.

   Run the [put\-image\-recipe\-policy](https://docs.aws.amazon.com/cli/latest/reference//imagebuilder/put-image-recipe-policy.html) Image Builder command in the AWS CLI, to identify the AWS principals to share the image with\.

   ```
   aws imagebuilder put-image-recipe-policy --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image-recipe/2019.12.03 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": [ "imagebuilder:GetImageRecipe", "imagebuilder:ListImageRecipes" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image-recipe/2019.12.03" ] } ] }'
   ```

------
#### [ Container recipe ]

   You can apply a resource policy to a container recipe to enable cross\-account sharing\. This command gives other accounts permission to use your recipe to create images in their accounts\. To successfully apply the resource policy, you must ensure that the account with which you are sharing has permission to access any resources that the recipe references, such as the base image, or selected components\.

   Run the [put\-container\-recipe\-policy](https://docs.aws.amazon.com/cli/latest/reference//imagebuilder/put-container-recipe-policy.html) Image Builder command in the AWS CLI, to identify the AWS principals to share the image with\.

   ```
   aws imagebuilder put-container-recipe-policy --container-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:container-recipe/my-example-container-recipe/2021.12.03 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "123456789012" ] }, "Action": [ "imagebuilder:GetContainerRecipe", "imagebuilder:ListContainerRecipes" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:container-recipe/my-example-container-recipe/2021.12.03" ] } ] }'
   ```

------
**Note**  
To set the correct policies for sharing and unsharing a resource, the resource owner must have `imagebuilder:put*` permissions\.

1. 

**Promote as a RAM resource share**

   To ensure that the resource is visible to all principals with whom you've shared it, run the [promote\-resource\-share\-created\-from\-policy](https://docs.aws.amazon.com/cli/latest/reference/ram/promote-resource-share-created-from-policy.html) AWS RAM command in the AWS CLI\.

## Unsharing a shared component, image, or recipe<a name="manage-shared-resources-unshare"></a>

To unshare a shared component, image, or recipe that you own, you must remove it from the resource share\. You can do this using the AWS Resource Access Manager console or the AWS CLI\.

**Note**  
To unshare a component, image, or recipe, the consumer cannot have any dependencies on them\. The consumer must remove any dependencies on the shared resources before the owner can unshare them\.

**To unshare a shared component, image, or recipe that you own using the AWS Resource Access Manager console**  
See [Updating a Resource Share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing.html#working-with-sharing-update) in the *AWS RAM User Guide*\.

**To unshare a shared component, image, or recipe that you own using the AWS CLI**  
Use the [disassociate\-resource\-share](https://docs.aws.amazon.com/cli/latest/reference/ram/disassociate-resource-share.html) command to stop sharing the resource\.

## Identifying a shared component, image, or recipe<a name="manage-shared-resources-identify"></a>

Owners and consumers can identify shared components, images, and image recipes using Image Builder commands in the AWS CLI\.

**Identify a shared component**  
Run the [list\-components](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-components.html) command to get a list of the components that you own and the components that are shared with you\. The [get\-component](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-component.html) command shows the AWS account ID of the component owner\.

**Identify a shared image**  
Run the [list\-images](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-images.html) command to get a list of the images that you own and images that are shared with you\. The [get\-image ](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-image.html) command shows the AWS account ID of the image owner\.

**Identify a shared container image**  
Run the [list\-images](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-images.html) command to get a list of the images that you own and images that are shared with you\. The [get\-image ](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-image.html) command shows the AWS account ID of the image owner\.

**Identify a shared image recipe**  
Run the [list\-image\-recipes](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-image-recipes.html) command to get a list of the image recipes that you own and image recipes that are shared with you\. The [get\-image\-recipe](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-image-recipe.html) command shows the AWS account ID of the image recipe owner\.

**Identify a shared container recipe**  
Run the [list\-container\-recipes](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-container-recipes.html) command to get a list of the container recipes that you own and container recipes that are shared with you\. The [get\-container\-recipe](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-container-recipe.html) command shows the AWS account ID of the container recipe owner\.

## Shared component, image, and recipe permissions<a name="manage-shared-resources-perms"></a>

**Permissions for owners**  
Owners cannot delete a shared component, image, or image recipe until it is no longer shared\. An owner cannot unshare these resources until none of the consumers depend on them\.

**Permissions for consumers**  
Consumers can read a component, image, or image recipe, but cannot modify them in any way\. They cannot view or modify these resources if they are owned by other consumers or the owner of the resource\. Consumers can use shared components and images in image recipes to create custom images\. Consumers can use shared image recipes to create their own custom images\.

## Billing and metering<a name="manage-shared-resources-billing"></a>

There is no charge to use EC2 Image Builder\.

## Resource limits<a name="manage-shared-resources-limits"></a>

Shared components, images, and image recipes count toward the corresponding resource limits of the owner only\. The resource limits of the consumers are not affected by the resources that have been shared with them\.