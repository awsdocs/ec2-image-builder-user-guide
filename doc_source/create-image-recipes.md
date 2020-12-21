# Create image recipes and versions<a name="create-image-recipes"></a>

This section describes creating new image recipes and recipe versions\.

**Topics**
+ [Create a new image recipe version \(console\)](#create-image-recipe-version-console)
+ [Create an image recipe \(AWS CLI\)](#create-image-recipe-cli)

## Create a new image recipe version \(console\)<a name="create-image-recipe-version-console"></a>

Creating a new version is virtually the same as creating a new recipe\. The difference is that certain details are pre\-selected to match the base recipe, in most cases\. The following list describes the differences between creating a new recipe and creating a new version of an existing recipe\.

**Base recipe details in the new version**
+ **Name** – *not editable*\.
+ **Version** – Required\. It is not pre\-filled with the current version or any kind of a sequence\. Enter the version number you want to create, using the format *major\.minor\.patch*\. If the version already exists, you will encounter an error\.
+ The **Select image** option – Pre\-selected, but editable\. If you change your choice for the source of your base image, you might lose other details that depend on the original option you chose\.

  To see details that are associated with your base image selection, choose the tab that matches your selection\.

------
#### [ Managed images ]
  + **Image Operating System \(OS\)** – *not editable*\.
  + **Image name** – Pre\-selected, based on the combination of source image choices you made for the existing recipe\. However, if you change the **Select image** option, you lose the pre\-selected **Image name**\.
  + **Auto\-versioning options** – does *not* match your base recipe\. This defaults to the **Use selected OS version** option\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available OS version**\.

------
#### [ Custom AMI ]
  + **AMI ID** – Required\. However, it is not pre\-filled with your original entry\. You must enter the AMI ID for your source image\.

------
+ **Working directory** – *not editable*\.
+ **Components** – Pre\-selected, but you can remove or reorder them to suit your needs\. Be aware that for the pre\-filled build or test components, the versioning does *not* match what you have in your base recipe\. The **Versioning options** default to **Specify component version**, regardless of what you specified in the base recipe\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available component version** for each component\.
+ **Storage \(volumes\)** – are pre\-filled\. The root volume **Device name**, **Snapshot**, and **IOPS** selections are not editable\. However, you can change all of the remaining settings, such as the **Size**\. You can also add new volumes\.

**To create a new image recipe version:**

1. At the top of the recipe details page, choose **Create new version**\. This takes you to the **Create image recipe** page\.

1. To create the new version, make your changes, then choose **Create image recipe**\.

For more information about creating an image recipe, within the context of creating an image pipeline, see [Step 2: Choose recipe](start-build-image-pipeline.md#start-build-image-step2) in the **Get started** section of this guide\.

## Create an image recipe \(AWS CLI\)<a name="create-image-recipe-cli"></a>

This section covers creating a new Image Builder image recipe using an imagebuilder command in the AWS CLI\.

### Prerequisites<a name="image-recipes-cli-prereq"></a>

Before you run the Image Builder commands in this section to create an image recipe using the AWS CLI, you must have created the components that the recipe will use\. The image recipe example in the next section references two components that were created in the [Create a component \(AWS CLI\)](create-components-cli.md) section of this guide\.

### Create a basic image recipe<a name="image-recipes-cli-create-recipe"></a>

This example shows the use of a basic image recipe, which is the minimal configuration requirement to get started\. You must replace the ARNs shown in the example with the ARNs that you received when you created the components\. The AWS Region and account ID will also be different for your configuration\.

**Important**  
Components are installed in the order in which they are specified\.

This example references the latest version of the Windows Server 2016 English Full Base image\. To target the latest version of the image, you can specify semantic versioning \(x\.x\.x\)\. The "x" wildcards represent the major, minor, and patch positions of the version number\.

The ARN in this example references the latest image in the SKU based on the semantic version filters that you have specified: `arn:aws:imagebuilder:us-west-2:aws:image/windows-server-2016-english-full-base-x86/x.x.x`\. You can provide the specific version that you want to use, or you can use a wildcard in all of the fields\.

```
{
    "name": "MyBasicRecipe",
    "description": "This example image recipe creates a Windows 2016 image.",
    "semanticVersion": "2019.12.03",
    "components": [
        {
            "componentArn": "arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02/1"
        },
        {
            "componentArn": "arn:aws:imagebuilder:us-west-2:123456789012:component/my-imported-component/1.0.0/1"
        }
    ],
    "parentImage": "arn:aws:imagebuilder:us-west-2:aws:image/windows-server-2016-english-full-base-x86/x.x.x"
}
```

Assuming that you have an image recipe definition stored in `create-image-recipe.json`, you can create the image recipe as follows:

```
aws imagebuilder create-image-recipe --cli-input-json file://create-image-recipe.json
```