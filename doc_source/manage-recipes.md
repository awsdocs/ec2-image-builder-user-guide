# Manage recipes<a name="manage-recipes"></a>

An EC2 Image Builder recipe defines the source image to use as your starting point to create a new image, along with the set of components that you add to customize your image and verify that everything is working as expected\.

After you create an image recipe, or a container recipe, you cannot modify or replace the recipe\. To update components after a recipe is created, you must create a new recipe or recipe version\.

**Note**  
Automatic version choices are provided for each component\. A maximum of 20 components, which include build and test, can be applied to a recipe\.

**Topics**
+ [List and view image recipe details](image-recipe-details.md)
+ [List and view container recipe details](container-recipe-details.md)
+ [Create image recipes and versions](create-image-recipes.md)
+ [Create container recipes and versions](create-container-recipes.md)
+ [Clean up resources](#recipes-cleanup)

## Clean up resources<a name="recipes-cleanup"></a>

To avoid unexpected charges, make sure to clean up resources and pipelines that you created from the examples in this guide\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.