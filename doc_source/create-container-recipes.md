# Create container recipes and versions<a name="create-container-recipes"></a>

This section covers creating new container recipes and recipe versions\.

**Topics**
+ [Create a new container recipe version \(console\)](#create-container-recipe-version)
+ [Create a container recipe \(AWS CLI\)](#create-container-recipe-cli)

## Create a new container recipe version \(console\)<a name="create-container-recipe-version"></a>

Creating a new version is virtually the same as creating a new recipe\. The difference is that certain details are pre\-selected to match the base recipe, in most cases\. The following list describes the differences between creating a new recipe and creating a new version of an existing recipe\.

**Base recipe details in the new version**
+ **Name** – *not editable*\.
+ **Version** – Required\. It is not pre\-filled with the current version or any kind of a sequence\. Enter the version number you want to create, using the format *major\.minor\.patch*\. If the version already exists, you will encounter an error\.
+ The **Select image** option – Pre\-selected, but editable\. If you change your choice for the source of your base image, you might lose other details that depend on the original option you chose\.

  To see details that are associated with your base image selection, choose the tab that matches your selection\.

------
#### [ Managed images ]
  + **Image Operating System \(OS\)** – *not editable*\.
  + The **Image name** – Pre\-selected, based on the combination of source image choices you made for the existing recipe\. However, if you change the **Select image** option, you lose the pre\-selected **Image name**\.
  + **Auto\-versioning options** – does *not* match your base recipe\. This defaults to the **Use selected OS version** option\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available OS version**\.

------
#### [ ECR image ]
  + **Image Operating System \(OS\)** – Pre\-selected, but editable\.
  + **OS version** – Pre\-selected, but editable\.
  + **ECR image ID** – Pre\-filled, but editable\.

------
#### [ Docker Hub image ]
  + **Image Operating System \(OS\)** – *not editable*\.
  + **OS version** – Pre\-selected, but editable\.
  + **Docker image ID** – Pre\-filled, but editable\.

------
+ **Working directory** – *not editable*\.
+ **Components** – Pre\-selected, but you can remove or reorder them to suit your needs\. For the pre\-filled build or test components, the versioning does *not* match what you have in your base recipe\. The **Versioning options** default to **Specify component version**, regardless of what you specified in the base recipe\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available component version** for each component\.
+ **Storage \(volumes\)** – Pre\-filled\. The root volume **Device name**, **Snapshot**, and **IOPS** selections are not editable\. However, you can change all of the remaining settings, such as the **Size**\. You can also add new volumes\.

**To create a new image recipe version:**

1. At the top of the recipe details page, choose **Create new version**\. You are taken to the **Create image recipe** page\.

1. To create the new version, make your changes, then choose **Create image recipe**\.

For more information about creating a container recipe, within the context of creating an image pipeline, see [Step 2: Choose recipe](start-build-container-pipeline.md#start-build-container-step2) in the **Get started** section of this guide\.

## Create a container recipe \(AWS CLI\)<a name="create-container-recipe-cli"></a>

A container recipe defines the source image to use as your starting point to create a new image, as well as the set of components that you add to customize your image\.

### Prerequisites<a name="container-recipes-cli-prereq"></a>

Before you run the Image Builder commands in this section to create a container recipe using the AWS CLI, you must have created the components that the recipe will use\. The container recipe example in the next section references two components that were created in the [Create a component \(AWS CLI\)](create-components-cli.md) section of this guide\.

### Create a basic container recipe<a name="container-recipes-cli-create"></a>

This example shows the use of a basic container recipe, which is the minimal configuration requirement to get started\. You must replace the ARNs shown in the example with the ARNs that you received when you created the components\. The AWS Region and account ID will also be different for your configuration\.

**Important**  
Components are installed in the order in which they are specified in the Dockerfile\.

This example references the latest version of the Windows Server 2016 English Full Base image\. To target the latest version of the image, you can specify semantic versioning \(x\.x\.x\)\. The "x" wildcards represent the major, minor, and patch positions of the version number\.

The ARN in this example references the latest image in the SKU based on the semantic version filters that you have specified: `arn:aws:imagebuilder:us-west-2:aws:image/windows-server-2016-english-full-base-x86/x.x.x`\. You can provide the specific version that you want to use, or you can use a wildcard in all of the fields\.

```
{
   "components": [ 
      { 
         "componentArn": "arn:aws:imagebuilder:us-east-1:123456789012:component/hellowworldal2/x.x.x"
      }
   ],
   "containerType": "DOCKER",
   "description": "My Linux Docker container image",
   "dockerfileTemplateData": "FROM {{{ imagebuilder:parentImage }}}\n{{{ imagebuilder:environments }}}\n{{{ imagebuilder:components }}}",
   "name": "amazonlinux-container-recipe",
   "parentImage": "amazonlinux:latest",
   "platformOverride": "Linux",
   "semanticVersion": "1.0.2",
   "tags": { 
      "sometag" : "Tag detail" 
   },
   "targetRepository": { 
      "repositoryName": "ECR",
      "service": "myrepo"
   },
   "workingDirectory": "/tmp"
}
```

Assuming that you have a container recipe definition stored in `create-container-recipe.json`, you can create the container recipe as follows:

```
aws imagebuilder create-container-recipe --cli-input-json file://create-container-recipe.json
```