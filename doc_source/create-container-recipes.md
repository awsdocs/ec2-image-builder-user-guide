# Create container recipes and versions<a name="create-container-recipes"></a>

This section covers creating new container recipes and recipe versions\.

**Topics**
+ [Create a new container recipe version \(console\)](#create-container-recipe-version)
+ [Create a container recipe \(AWS CLI\)](#create-container-recipe-cli)

## Create a new container recipe version \(console\)<a name="create-container-recipe-version"></a>

Creating a new version is virtually the same as creating a new recipe\. The difference is that certain details are pre\-selected to match the base recipe, in most cases\. The following list describes the differences between creating a new recipe and creating a new version of an existing recipe\.

**Recipe details**
+ **Name** – *not editable*\.
+ **Version** – Required\. It is not pre\-filled with the current version or any kind of a sequence\. Enter the version number you want to create, using the format *major\.minor\.patch*\. If the version already exists, you will encounter an error\.

**Base image**
+ The **Select image** option – Pre\-selected, but editable\. If you change your choice for the source of your base image, you might lose other details that depend on the original option you chose\.

  To see details that are associated with your base image selection, choose the tab that matches your selection\.

------
#### [ Managed images ]
  + **Image Operating System \(OS\)** – *not editable*\.
  + The **Image name** – Pre\-selected, based on the combination of base image choices you made for the existing recipe\. However, if you change the **Select image** option, you lose the pre\-selected **Image name**\.
  + **Auto\-versioning options** – does *not* match your base recipe\. This defaults to the **Use selected OS version** option\.
**Important**  
If you are using semantic versioning to kick off pipeline builds, make sure you change this value to **Use latest available OS version**\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.

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

**Instance configuration**
+ **AMI ID** – Pre\-filled, but editable\.
+ 

**Storage \(volumes\)**  
**EBS volume 1 \(AMI root\)** – Pre\-filled\. You cannot edit the root volume **Device name**, **Snapshot**, and **IOPS** selections\. However, you can change all of the remaining settings, such as the **Size**\. You can also add new volumes\.

**Working directory**
+ **Working directory path** – Pre\-filled, but editable\.

**Components**
+ **Components** – Components that are already included in the recipe are displayed in the **Selected components** section at the end of each of the component lists \(build and test\)\. You can remove or reorder the selected components to suit your needs\.

  You can configure the following settings for your selected component:
  + **Versioning options** – Pre\-selected, but you can change them\. We recommend that you choose the **Use latest available component version** option to ensure that your image builds always pick up the latest version of the component\. If you need to use a specific component version in your recipe, you can choose **Specify component version**, and enter the version in the **Component version** box that appears\.
  + **Input parameters** – Displays input parameters that the component accepts\. The **Value** is pre\-filled with the value from the prior version of the recipe\. If you are using this component for the first time in this recipe, and a default value was defined for the component, the default value appears in the **Value** box with greyed\-out text\. If no other value is entered, AWSTOE uses the default value\.

  To expand **Versioning options** or **Input parameters** settings, you can either choose the arrow next to the name of the setting, or you can toggle the **Expand all** switch off and on to expand all of the settings for all of the selected components\.

**Target repository**
+ **Target repository name** – Pre\-filled, but editable\.

**To create a new container recipe version:**

1. At the top of the container recipe details page, choose **Create new version**\. You are taken to the **Create recipe** page for container recipes\.

1. To create the new version, make your changes, then choose **Create recipe**\.

For more information about creating a container recipe, within the context of creating an image pipeline, see [Step 2: Choose recipe](start-build-container-pipeline.md#start-build-container-step2) in the **Get started** section of this guide\.

## Create a container recipe \(AWS CLI\)<a name="create-container-recipe-cli"></a>

To create an Image Builder container recipe, using the `imagebuilder create-container-recipe` command in the AWS CLI, follow these steps:

**Prerequisites**  
Before you run the Image Builder commands in this section to create a container recipe using the AWS CLI, you must have created the components that the recipe will use\. The container recipe example in the following step refers to example components that are created in the [Create a component \(AWS CLI\)](create-components-cli.md) section of this guide\.

After you create your components, or if you are using existing components, take note of the ARNs that you want to include in the recipe\.

1. 

**Create a CLI input JSON file**

   To streamline the imagebuilder create\-container\-recipe command that is used in the AWS CLI, we create a JSON file that contains all of the recipe parameters that we want to pass into the command\. Save the file as `create-container-recipe.json`, to use in the imagebuilder create\-container\-recipe command\.
**Note**  
The naming convention for the data points in the JSON file follows the pattern that is specified for the Image Builder API command request parameters\. To review the API command request parameters, see the [CreateContainerRecipe](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateContainerRecipe.html) command in the *EC2 Image Builder API Reference*\.  
Do not use the naming convention that is specified for providing these datapoints directly to the imagebuilder create\-container\-recipe command as options\.

   Here is a summary of the parameters that we specify in is example:
   + **components** \(array of objects, required\) – Contains an array of `ComponentConfiguration` objects\. At least one build component must be specified:
**Important**  
Components are installed in the order in which they are specified\.
     + **componentARN** \(string, required\) – The component ARN\.
**Note**  
To use the example to create your own container recipe, you must replace the example ARNs with the ARNs for the components that you are using for your recipe, including the AWS Region, name, and version number for each\.
     + **parameters** \(array of objects\) – Contains an array of `ComponentParameter` objects\.
       + **name** \(string, required\) – The name of the component parameter to set\.
       + **value** \(array of strings, required\) – Contains an array of strings to set the value for the named component parameter\. If there is a default value defined for the component, and no other value is provided, AWSTOE uses the default value\.
   + **containerType** \(string, required\) – The type of container to create\. Valid values include: "DOCKER"\.
   + **dockerfileTemplateData** \(string\) – The Dockerfile template that is used to build your image, as an inline data blob\.
   + **name** \(string, required\) – The name of the container recipe\.
   + **description** \(string\) – The description of the container recipe\.
   + **parentImage** \(string, required\) – The source \(parent\) image that the container recipe uses as its base environment\.
   + **platformOverride** \(string\) – Specifies the operating system platform when you use a custom base image\.
   + **semanticVersion** \(string, required\) – The semantic version of the container recipe, which specifies the version in the following format, with numeric values in each position to indicate a specific version: <major>\.<minor>\.<patch>\. For example, `1.0.0`\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.
   + **tags** \(string map\) – Tags that are attached to the container recipe\.
   + **instanceConfiguration** \(object\) – A group of options that can be used to configure an instance for building and testing container images\.
     + **image** \(string\) – The AMI ID to use as the base image for a container build and test instance\. If not specified, Image Builder will use the appropriate Amazon ECS\-optimized AMI as a base image\.
     + **blockDeviceMappings** \(array of objects\) – Defines the block devices to attach for building an instance from the Image Builder AMI specified in the image parameter\.
       + **deviceName** \(string\) – The device to which these mappings apply\.
       + **ebs** \(object\) – Used to manage Amazon EBS\-specific configuration for this mapping\.
         + **deleteOnTermination** \(Boolean\) – Used to configure delete on termination of the associated device\.
         + **encrypted** \(Boolean\) – Used to configure device encryption\.
         + **volumeSize** \(integer\) – Used to override the device's volume size\.
         + **volumeType** \(string\) – Used to override the device's volume type\.
   + **targetRepository** \(object, required\) – The destination repository for the container image\.
     + **repositoryName** \(string, required\) – The name of the container repository where the output container image is stored\. This name is prefixed by the repository location\.
     + **service** \(string, required\) – Specifies the service in which this image was registered\.
   + **workingDirectory** \(string\) – The working directory for use during build and test workflows\.

   ```
   {
      "components": [ 
         { 
            "componentArn": "arn:aws:imagebuilder:us-east-1:123456789012:component/helloworldal2/x.x.x"
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
      "instanceConfiguration": {
         "image": "ami-1234567890",
         "blockDeviceMappings": [
            {
               "deviceName": "/dev/xvda",
               "ebs": {
                  "deleteOnTermination": true,
                  "encrypted": false,
                  "volumeSize": 8,
                  "volumeType": "gp2"
                }
             }   		
         ]
      },
      "targetRepository": { 
         "repositoryName": "myrepo",
         "service": "ECR"
      },
      "workingDirectory": "/tmp"
   }
   ```

1. 

**Create the recipe**

   Use the following command to create the recipe, referencing the file name for the JSON file that you created in the prior step:

   ```
   aws imagebuilder create-container-recipe --cli-input-json file://create-container-recipe.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.