# Create a component \(AWS CLI\)<a name="create-components-cli"></a>

In this section, we'll cover creating AWS TOE components by using Image Builder commands in the AWS CLI\.

## Create a YAML component document<a name="create-component-yaml"></a>

To build a component, you must provide a YAML application component document, which represents the phases and steps to create the component\.

**Create a YAML component document**  
To create a YAML application component document for a sample application, follow the steps that are shown in the tab that matches your image operating system\.

------
#### [ Linux ]

**Create a YAML component file**  
Use your favorite file editing tool to create a file named `component.yaml`, that has the following content:

```
# Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of this
# software and associated documentation files (the "Software"), to deal in the Software
# without restriction, including without limitation the rights to use, copy, modify,
# merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
# INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
# PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
name: Update Linux
description: Updates Linux with the latest security updates.
schemaVersion: 1
phases:
  - name: build
    steps:
    - name: UpdateOS
      action: UpdateOS
# Document End
```

**Tip**  
Use a tool like this online [YAML Validator](https://jsonformatter.org/yaml-validator), or a YAML lint extension in your code environment to verify that your YAML is well\-formed\.

------
#### [ Windows ]

**Create a YAML component file**  
Use your favorite file editing tool to create a file named `component.yaml`, that has the following content:

```
# Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of this
# software and associated documentation files (the "Software"), to deal in the Software
# without restriction, including without limitation the rights to use, copy, modify,
# merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
# INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
# PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
name: Update Windows
description: Updates Windows with the latest security updates.
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: UpdateOS
        action: UpdateOS
# Document End
```

**Tip**  
Use a tool like this online [YAML Validator](https://jsonformatter.org/yaml-validator), or a YAML lint extension in your code environment to verify that your YAML is well\-formed\.

------

To learn more about the phases, steps, and syntax for AWS TOE YAML application component documents, see [Use documents in AWS TOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/image-builder-application-documents.html)\.

## Create AWS TOE components using Image Builder \(AWS CLI\)<a name="create-component-cli"></a>

This section walks you through the following steps to create an AWS TOE application component, using Image Builder commands in the AWS CLI:
+ Upload your YAML component document to an Amazon S3 bucket that you can reference from the command line\.
+ Upload dependencies for your YAML component document to the same Amazon S3 bucket\.
+ Create the AWS TOE application component using the imagebuilder create\-component command\.
+ List component versions using the imagebuilder list\-components command with a name filter to determine the next version for updates\.

To create an AWS TOE application component from the YAML document you created in the prior section, follow the steps that match your image operating system\.

------
#### [ Linux ]

**Store your application component document in Amazon S3**

You can use Amazon S3 as a repository for the YAML application component document you created in a prior section, and all of its dependencies\. Follow these steps to store your AWS TOE application component source document and dependencies:

1. 

**Upload the document to Amazon S3**

   *If your document is smaller than 64 KB, you can skip this step\.* Documents that are 64 KB or larger in size must be stored in Amazon S3\.

   ```
   aws s3 cp component.yaml s3://my-s3-bucket/my-path/component.yaml
   ```

1. 

**Upload resource dependencies for the YAML document to Amazon S3**

   You must upload any resources that your document references, or your document will fail at runtime\. In this example, we upload the zip archive that the `component.yaml` document references\.

   ```
   aws s3 cp my_zip_archive.zip s3://my-s3-bucket/my-path/my_zip_archive.zip
   ```

**Create a component from the YAML document**

To streamline the imagebuilder create\-component command that is used in the AWS CLI, we create a JSON file that contains all of the component parameters that we want to pass into the command, including the location of the `component.yaml` document created in the prior steps\. The `uri` key value pair contains the file reference\.
**Note**  
The naming convention for the data points in the JSON file follows the pattern that is specified for the Image Builder API command request parameters\. To review the API command request parameters, see the [CreateComponent](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateComponent.html) command in the *EC2 Image Builder API Reference*\.  
Do not use the naming convention that is specified for providing these datapoints directly to the imagebuilder create\-component command as options\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a file named `create-component.json`, that has the following content:

   ```
   {
   	"name": "MyExampleComponent",
   	"semanticVersion": "1.1.2",
   	"description": "An example component that updates the Linux operating system",
   	"changeDescription": "Initial version.",
   	"platform": "Linux",
   	"uri": "s3://my-s3-bucket/my-path/component.yaml",
   	"kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/98765432-b123-456b-7f89-0123456f789c",
   	"tags": {
   		"MyTagKey": "Some Value"
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
   aws imagebuilder create-component --cli-input-json file://create-component.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

------
#### [ Windows ]

**Store your application component document in Amazon S3**

You can use Amazon S3 as a repository for the YAML application component document you created in a prior section, and all of its dependencies\. Follow these steps to store your AWS TOE application component source document and dependencies:

1. 

**Upload the document to Amazon S3**

   *If your document is smaller than 64 KB, you can skip this step\.* Documents that are 64 KB or larger in size must be stored in Amazon S3\.

   ```
   aws s3 cp component.yaml s3://my-s3-bucket/my-path/component.yaml
   ```

1. 

**Upload resource dependencies for the YAML document to Amazon S3**

   You must upload any resources that your document references, or your document will fail at runtime\. In this example, we upload the zip archive that the `component.yaml` document references\.

   ```
   aws s3 cp my_zip_archive.zip s3://my-s3-bucket/my-path/my_zip_archive.zip
   ```

**Create a component from the YAML document**

To streamline the imagebuilder create\-component command that is used in the AWS CLI, we create a JSON file that contains all of the component parameters that we want to pass into the command, including the location of the `component.yaml` document created in the prior steps\. The `uri` key value pair contains the file reference\.
**Note**  
The naming convention for the data points in the JSON file follows the pattern that is specified for the Image Builder API command request parameters\. To review the API command request parameters, see the [CreateComponent](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateComponent.html) command in the *EC2 Image Builder API Reference*\.  
Do not use the naming convention that is specified for providing these datapoints directly to the imagebuilder create\-component command as options\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a file named `create-component.json`, that has the following content:

   ```
   {
   	"name": "MyExampleComponent",
   	"semanticVersion": "1.1.2",
   	"description": "An example component that updates the Windows operating system.",
   	"changeDescription": "Initial version.",
   	"platform": "Windows",
   	"uri": "s3://my-s3-bucket/my-path/component.yaml",
   	"kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/98765432-b123-456b-7f89-0123456f789c",
   	"tags": {
   		"MyTagKey": "Some Value"
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
   aws imagebuilder create-component --cli-input-json file://create-component.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

------

### AWS TOE component versioning for updates \(AWS CLI\)<a name="component-update-cli"></a>

AWS TOE component names and versions are embedded in the component's Amazon Resource Name \(ARN\), after the component prefix\. Each new version of a component has its own unique ARN\. The steps to create a new version are exactly the same as the steps to create a new component, as long as the semantic version is unique for that component name\.

To ensure that you assign the next logical version, first get a list of the existing versions for the component that you want to change, using the imagebuilder list\-components, command in the AWS CLI, and filtering on the name, as follows:

```
aws imagebuilder list-components --filters name="name",values="mytestcomponent"	
{
    "requestId": "123a4567-b890-123c-45d6-ef789ab0cd1e",
    "componentVersionList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:1234560087789012:component/mytestcomponent/1.0.0",
            "name": "mytestcomponent",
            "version": "1.0.0",
            "platform": "Linux",
            "type": "BUILD",
            "owner": "123456789012",
            "dateCreated": "2020-09-24T16:58:24.444Z"
        },
        {
            "arn": "arn:aws:imagebuilder:us-west-2:1234560087789012:component/mytestcomponent/1.0.1",
            "name": "mytestcomponent",
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