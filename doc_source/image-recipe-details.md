# List and view image recipe details<a name="image-recipe-details"></a>

This section describes the various ways that you can find information and view details for your EC2 Image Builder image recipes\.

**Topics**
+ [List image recipes \(console\)](#list-image-recipes-console)
+ [List image recipes \(AWS CLI\)](#cli-list-image-recipes)
+ [View image recipe details \(console\)](#view-image-recipe-details-console)
+ [Get image recipe details \(AWS CLI\)](#cli-get-image-recipe)
+ [Get image recipe policy details \(AWS CLI\)](#cli-get-image-recipe-policy)

## List image recipes \(console\)<a name="list-image-recipes-console"></a>

To see a list of the image recipes created under your account in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Image recipes** from the navigation pane\. This shows a list of the image recipes that are created under your account\.

1. To view details or create a new recipe version, choose the **Recipe name** link\. This opens the detail view for the recipe\.
**Note**  
You can also select the check box next to the **Recipe name**, then choose **View details**\.

## List image recipes \(AWS CLI\)<a name="cli-list-image-recipes"></a>

The following example shows how to list all of your image recipes, using the AWS CLI\.

```
aws imagebuilder list-image-recipes
```

## View image recipe details \(console\)<a name="view-image-recipe-details-console"></a>

To view details for a specific image recipe using the Image Builder console, select the image recipe to review, using the steps described in [List image recipes \(console\)](#list-image-recipes-console)\.

On the recipe detail page, you can:
+ Delete the recipe\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.
+ Create a new version\.
+ Create a pipeline from the recipe\. After choosing **Create pipeline from this recipe**, you are taken to the pipeline wizard\. For more information about creating an Image Builder pipeline using the pipeline wizard, see [Create an image pipeline using the EC2 Image Builder console wizard](start-build-image-pipeline.md)
**Note**  
When you create a pipeline from an existing recipe, the option to create a new recipe is not available\.

## Get image recipe details \(AWS CLI\)<a name="cli-get-image-recipe"></a>

The following example shows how to use an imagebuilder CLI command to get the details of an image recipe by specifying its Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-image-recipe --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2020.12.03
```

## Get image recipe policy details \(AWS CLI\)<a name="cli-get-image-recipe-policy"></a>

The following example shows how to use an imagebuilder CLI command to get the details of an image recipe policy by specifying its ARN\.

```
aws imagebuilder get-image-recipe-policy --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2020.12.03
```