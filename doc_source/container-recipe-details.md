# List and view container recipe details<a name="container-recipe-details"></a>

This section describes the various ways you can find information and view details for your EC2 Image Builder container recipes\.

**Topics**
+ [List container recipes \(console\)](#list-container-recipes-console)
+ [List container recipes \(AWS CLI\)](#cli-list-container-recipes)
+ [View container recipe details \(console\)](#view-container-recipe-details-console)
+ [Get container recipe details \(AWS CLI\)](#cli-get-container-recipe)
+ [Get container recipe policy details \(AWS CLI\)](#cli-get-container-recipe-policy)

## List container recipes \(console\)<a name="list-container-recipes-console"></a>

To see a list of the container recipes created under your account in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Container recipes** from the navigation pane\. This shows a list of the container recipes that are created under your account\.

1. To view details or create a new recipe version, choose the **Recipe name** link\. This opens the detail view for the recipe\.
**Note**  
You can also select the check box next to the **Recipe name**, then choose **View details**\.

## List container recipes \(AWS CLI\)<a name="cli-list-container-recipes"></a>

The following example shows how to list all of your container recipes, using the AWS CLI\.

```
aws imagebuilder list-container-recipes
```

## View container recipe details \(console\)<a name="view-container-recipe-details-console"></a>

To view details for a specific container recipe using the Image Builder console, select the container recipe to review, using the steps described in [List container recipes \(console\)](#list-container-recipes-console)\.

On the recipe detail page, you can:
+ Delete the recipe\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.
+ Create a new version\.
+ Create a pipeline from the recipe\. After choosing **Create pipeline from this recipe**, you are taken to the pipeline wizard\. For more information about creating an Image Builder pipeline using the pipeline wizard, see [Create an image pipeline using the EC2 Image Builder console wizard](start-build-image-pipeline.md)
**Note**  
When you create a pipeline from an existing recipe, the option to create a new recipe is not available\.

## Get container recipe details \(AWS CLI\)<a name="cli-get-container-recipe"></a>

The following example shows how to use an imagebuilder CLI command to get the details of a container recipe by specifying its ARN\.

```
aws imagebuilder get-container-recipe --container-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:container-recipe/my-example-recipe/2020.12.03
```

## Get container recipe policy details \(AWS CLI\)<a name="cli-get-container-recipe-policy"></a>

The following example shows how to use an imagebuilder CLI command to get the details of a container recipe policy by specifying its ARN\.

```
aws imagebuilder get-container-recipe-policy --container-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:container-recipe/my-example-recipe/2020.12.03
```