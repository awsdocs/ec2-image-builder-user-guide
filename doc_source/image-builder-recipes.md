# Manage EC2 Image Builder image recipes using the console<a name="image-builder-recipes"></a>

After you create an image recipe, its components, and the order in which they run, cannot be modified or replaced\. To update components after an image recipe is created, you must create a new recipe or recipe version\.

**Note**  
Automatic version choices are provided for each component\. A maximum of 20 components, which include build and test, can be applied to a recipe\.

**Choose an existing Image Builder recipe**

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. To see a list of the image recipes created under your account, choose **Image recipes** from the navigation pane\.

1. To view details or create a new recipe version, choose the **Recipe name** link\. This opens the detail view for the pipeline\.
**Note**  
You can also choose the check box next to the **Recipe name**, then choose **View detail**\.

On the recipe detail page, you can:
+ Delete the recipe\. For more information about deleting resources in Image Builder, see [Delete resources](image-builder-delete-pipeline.md)\.
+ Create a new version\.
+ Create a pipeline from the recipe\. After choosing **Create pipeline from this recipe**, you are taken to the pipeline wizard\. For more information about creating an Image Builder pipeline using the pipeline wizard, see [Create an automated build pipeline using the EC2 Image Builder console wizard](image-builder-image-deployment-console.md)

## Create a new image recipe version<a name="create-recipe-version"></a>

Creating a new version is virtually the same as creating a new recipe\. The difference is that certain details are pre\-selected to match the base recipe, in most cases\. The following list describes the differences between creating a new recipe and creating a new version of an existing recipe\.

**Base recipe details in the new version**
+ **Name** – *not editable*\.
+ **Version** – Required\. It is not pre\-filled with the current version or any kind of a sequence\. Enter the version number you want to create, using the format *major\.minor\.patch*\. If the version already exists, you will encounter an error\.
+ **Image Operating System \(OS\)** – If your base image selection was **Select managed images**, this is not editable
+ The **Image name** – Pre\-selected, based on the combination of source image choices you made for the existing recipe\. However, if you change the **Select image** option, you lose the pre\-selected **Image name**\.
+ **Auto\-versioning options** – does *not* match your base recipe\. This defaults to the **Use selected OS version** option\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available OS version**\.
+ **Working directory** – *not editable*\.
+ Build and Test **Components** – Pre\-selected, but you can remove or reorder it to suit your needs\. Be aware that for the pre\-filled build or test components, the versioning does *not* match what you have in your base recipe\. The **Versioning options** default to **Specify component version**, regardless of what you specified in the base recipe\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available component version**\.
+ **Storage \(volumes\)** – are pre\-filled\. The root volume **Device name**, **Snapshot**, and **IOPS** selections are not editable\. However, you can change all of the remaining settings, such as the **Size**\. You can also add new volumes\.

To create a new image recipe version:

1. At the top of the view recipe details page, choose **Create new version**\. This takes you to the **Create image recipe** page\.

1. To create the new version, make your changes, then choose **Create image recipe**\.

For more information about creating an image recipe, within the context of creating an image pipeline, see [Step 2: Choose recipe](image-builder-image-deployment-console.md#start-build-step2) in the **Get started** section of this guide\.