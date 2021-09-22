# Manage recipes<a name="manage-recipes"></a>

An EC2 Image Builder recipe defines the base image to use as your starting point to create a new image, along with the set of components that you add to customize your image and verify that everything is working as expected\. Automatic version choices are provided for each component\. A maximum of 20 components, which include build and test, can be applied to a recipe\.

After you create an image recipe, or a container recipe, you cannot modify or replace the recipe\. To update components after a recipe is created, you must create a new recipe or recipe version\. You can, however, always apply tags to your recipe\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

**Tip**  
You can streamline the process of putting together Image Builder recipes by developing components locally, so that they are ready when you need them\. To get started developing components locally, with AWSTOE, see [Get started with AWSTOE](toe-get-started.md)\.

This section covers how to list, view, and create recipes\.

**Topics**
+ [List and view image recipe details](image-recipe-details.md)
+ [List and view container recipe details](container-recipe-details.md)
+ [Create image recipes and versions](create-image-recipes.md)
+ [Create container recipes and versions](create-container-recipes.md)
+ [Clean up resources](#recipes-cleanup)

## Clean up resources<a name="recipes-cleanup"></a>

To avoid unexpected charges, make sure to clean up resources and pipelines that you created from the examples in this guide\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.