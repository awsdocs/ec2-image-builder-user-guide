# Create a component with the AWS CLI<a name="create-components-cli"></a>

This section describes how to use Image Builder commands to create AWS Task Orchestrator and Executor \(AWSTOE\) components from the AWS Command Line Interface\. To build a component, provide a YAML application component document\. This represents the phases and steps that you need to create the component\. To create a new YAML component document, see [Create a YAML component document](create-component-yaml.md)\.

## Create AWSTOE components with Image Builder with the AWS CLI<a name="create-component-cli"></a>

In this section, you'll learn how to set up and use Image Builder commands in the AWS CLI to create an AWSTOE application component, as follows\.
+ Upload your YAML component document to an S3 bucket that you can reference from the command line\.
+ Create the AWSTOE application component with the create\-component command\.
+ List component versions with the list\-components command and a name filter to see what versions already exist\. You can use the output to determine what the next version should be for updates\.

To create an AWSTOE application component from an input YAML document, follow the steps that match your image operating system platform\.

------
#### [ Linux ]

**Store your application component document in Amazon S3**

You can use an S3 bucket as a repository for your AWSTOE application component source document\. To store your component document, follow these steps:
+ 

**Upload the document to Amazon S3**

  *If your document is smaller than 64 KB, you can skip this step\.* Documents that are 64 KB or larger in size must be stored in Amazon S3\.

  ```
  aws s3 cp update-linux-os.yaml s3://my-s3-bucket/my-path/update-linux-os.yaml
  ```

**Create a component from the YAML document**

To streamline the create\-component command that you use in the AWS CLI, create a JSON file that contains all of the component parameters that you want to pass into the command\. Include the location of the `update-linux-os.yaml` document that you created in the prior steps\. The `uri` key\-value pair contains the file reference\.
**Note**  
The naming convention for the data values in the JSON file follows the pattern that is specified for the Image Builder API action request parameters\. To review the API command request parameters, see the [CreateComponent](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateComponent.html) command in the *EC2 Image Builder API Reference*\.  
To provide the data values as command line parameters, refer to the parameter names specified in the *AWS CLI Command Reference*\.

1. 

**Create a CLI input JSON file**

   Use a file editing tool to create a file named `create-update-linux-os-component.json`\. Include the following content:

   ```
   {
   	"name": "update-linux-os",
   	"semanticVersion": "1.1.2",
   	"description": "An example component that updates the Linux operating system",
   	"changeDescription": "Initial version.",
   	"platform": "Linux",
   	"uri": "s3://my-s3-bucket/my-path/update-linux-os.yaml",
   	"kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/98765432-b123-456b-7f89-0123456f789c",
   	"tags": {
   		"MyTagKey-purpose": "security-updates"
   	}
   }
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

1. 

**Create the component**

   Use the following command to create the component, referencing the file name for the JSON file that you created in the prior step:

   ```
   aws imagebuilder create-component --cli-input-json file://create-update-linux-os-component.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

------
#### [ Windows ]

**Store your application component document in Amazon S3**

You can use an S3 bucket as a repository for your AWSTOE application component source document\. To store your component document, follow these steps:
+ 

**Upload the document to Amazon S3**

  *If your document is smaller than 64 KB, you can skip this step\.* Documents that are 64 KB or larger in size must be stored in Amazon S3\.

  ```
  aws s3 cp update-windows-os.yaml s3://my-s3-bucket/my-path/update-windows-os.yaml
  ```

**Create a component from the YAML document**

To streamline the create\-component command that you use in the AWS CLI, create a JSON file that contains all of the component parameters that you want to pass into the command\. Include the location of the `update-windows-os.yaml` document that you created in the prior steps\. The `uri` key\-value pair contains the file reference\.
**Note**  
The naming convention for the data values in the JSON file follows the pattern that is specified for the Image Builder API action request parameters\. To review the API command request parameters, see the [CreateComponent](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateComponent.html) command in the *EC2 Image Builder API Reference*\.  
To provide the data values as command line parameters, refer to the parameter names specified in the *AWS CLI Command Reference*\.\.

1. 

**Create a CLI input JSON file**

   Use a file editing tool to create a file named `create-update-windows-os-component.json`\. Include the following content:

   ```
   {
   	"name": "update-windows-os",
   	"semanticVersion": "1.1.2",
   	"description": "An example component that updates the Windows operating system.",
   	"changeDescription": "Initial version.",
   	"platform": "Windows",
   	"uri": "s3://my-s3-bucket/my-path/update-windows-os.yaml",
   	"kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/98765432-b123-456b-7f89-0123456f789c",
   	"tags": {
   		"MyTagKey-purpose": "security-updates"
   	}
   }
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

1. 

**Create the component**

   Use the following command to create the component, referencing the file name for the JSON file that you created in the prior step:

   ```
   aws imagebuilder create-component --cli-input-json file://create-update-windows-os-component.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

------

### AWSTOE component versioning for updates \(AWS CLI\)<a name="component-update-cli"></a>

AWSTOE component names and versions are embedded in the component's Amazon Resource Name \(ARN\), after the component prefix\. Each new version of a component has its own unique ARN\. The steps to create a new version are exactly the same as the steps to create a new component, as long as the semantic version is unique for that component name\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.

To ensure that you assign the next logical version, first get a list of the existing versions for the component that you want to change\. Use the list\-components command with the AWS CLI, and filter on the name\.

In this example, you filter on the name of the component that you created in the prior Linux examples\. To list the component that you created, use the value of the `name` parameter from the JSON file that you used in the create\-component command\.

```
aws imagebuilder list-components --filters name="name",values="update-linux-os"	
{
    "requestId": "123a4567-b890-123c-45d6-ef789ab0cd1e",
    "componentVersionList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:1234560087789012:component/update-linux-os/1.0.0",
            "name": "update-linux-os",
            "version": "1.0.0",
            "platform": "Linux",
            "type": "BUILD",
            "owner": "123456789012",
            "dateCreated": "2020-09-24T16:58:24.444Z"
        },
        {
            "arn": "arn:aws:imagebuilder:us-west-2:1234560087789012:component/update-linux-os/1.0.1",
            "name": "update-linux-os",
            "version": "1.0.1",
            "platform": "Linux",
            "type": "BUILD",
            "owner": "123456789012",
            "dateCreated": "2021-07-10T03:38:46.091Z"
        }
    ]
}
```

Based on your results, you can determine what the next version should be\.