# Setting Up and Managing an EC2 Image Builder Image Pipeline Using the AWS CLI<a name="managing-image-builder-cli"></a>

You can set up, configure, and manage image pipelines using the AWS CLI\. The following example CLI commands show common operations and sample file configurations to help you create and manage image pipelines\. 

**Note**  
Creating images, recipes, or pipelines with tags may fail when using Amazon\-provided or shared resources not owned by your account\. To avoid this failure, create the resources without tags, and then add tags after the resources are created\.

**Topics**
+ [Create a Component Document](#image-builder-cli-create-component)
+ [Upload Document to Amazon S3](#image-builder-cli-upload-document)
+ [Upload Any Resources Referenced by Document](#image-builder-cli-upload-resources)
+ [Create a Component](#image-builder-cli-create-component)
+ [Import a Component](#image-builder-cli-import-component)
+ [Create a Basic Image Recipe](#image-builder-cli-create-recipe)
+ [Create an Image](#image-builder-cli-create-image)
+ [Get an Image](#image-builder-cli-get-image)
+ [Create a Distribution Configuration](#image-builder-cli-create-distribution-configuration)
+ [Create an Infrastructure Configuration](#image-builder-cli-create-infrastructure-configuration)
+ [Create an Image Pipeline](#image-builder-cli-create-image-pipeline)
+ [Cancel an Image Creation](#image-builder-cli-cancel-image-creation)
+ [Apply a Resource Policy to a Component](#image-builder-cli-apply-resource-policy-component)
+ [Apply a Resource Policy to an Image Recipe](#image-builder-cli-apply-resource-policy-recipe)
+ [Apply a Resource Policy to an Image](#image-builder-cli-apply-resource-policy-image)
+ [Update a Distribution Configuration](#image-builder-cli-update-distribution-configuration)
+ [Update an Infrastructure Configuration](#image-builder-cli-update-infrastructure-configuration)
+ [Update an Image Pipeline](#image-builder-cli-update-image-pipeline)
+ [Start an Image Pipeline Manually](#image-builder-cli-start-image-pipeline)
+ [Tag a Resource](#image-builder-cli-tag-resource)
+ [Untag a Resource](#image-builder-cli-untag-resource)
+ [Delete an Image Pipeline](#image-builder-cli-delete-image-pipeline)
+ [Delete an Infrastructure Configuration](#image-builder-cli-delete-infrastructure-configuration)
+ [Delete a Distribution Configuration](#image-builder-cli-delete-distribution-configuration)
+ [Delete an Image Recipe](#image-builder-cli-delete-image-recipe)
+ [Delete a Component](#image-builder-cli-delete-component)
+ [Delete an Image](#image-builder-cli-delete-image)
+ [Get Component Details](#image-builder-cli-get-component-details)
+ [Get Component Policy Details](#image-builder-cli-get-component-policy-details)
+ [Get Distribution Configuration Details](#image-builder-cli-get-distribution-configuration-details)
+ [Get Image Details](#image-builder-cli-get-image-details)
+ [Get Image Pipeline Details](#image-builder-cli-get-image-pipeline-details)
+ [Get Image Policy Details](#image-builder-cli-get-image-policy-details)
+ [Get Image Recipe Details](#image-builder-cli-get-image-recipe-details)
+ [Get Image Recipe Policy Details](#image-builder-cli-get-image-recipe-policy-details)
+ [Get an Infrastructure Configuration Details](#image-builder-cli-get-infrastructure-configuration-details)
+ [List Components](#image-builder-cli-list-components)
+ [List Component Build Versions](#image-builder-cli-list-component-versions)
+ [List Distributions](#image-builder-cli-list-distributions)
+ [List Images](#image-builder-cli-list-images)
+ [List Image Build Versions](#image-builder-cli-list-image-build-versions)
+ [List Image Pipeline Images](#image-builder-cli-list-image-pipeline-images)
+ [List Image Pipelines](#image-builder-cli-list-image-pipelines)
+ [List Image Recipes](#image-builder-cli-list-image-pipeline-recipes)
+ [List Infrastructure Configurations](#image-builder-cli-list-infrastructure-configurations)
+ [List All of the Tags for a Specific Resource](#image-builder-cli-list-all-tags-resource)

## Create a Component Document<a name="image-builder-cli-create-component"></a>

The first step in setting up your pipeline is to define a document that will perform the AMI customizations\. The document can contain build, validate, and test phases\. For more information, see [Document Schema and Definitions](managing-image-builder-console.md#document-schema)\.

This example assumes that we have named this document `component.yaml`\.

```
name: 'An_Example_Document'
description: 'This document has a build, validate and test phase'
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: Download_Scripts
        action: S3Download
        inputs:
          - source: 's3://my-s3-bucket/my-path/my_zip_archive.zip'
            destination: 'c:\mydirectory\my_zip_archive.zip'
      - name: Extract_Tools
        action: ExecutePowerShell
        inputs:
          commands:
            - 'Expand-Archive -LiteralPath {{build.Download_Scripts.inputs[0].destination}}'
      - name: 'DisableHibernation'
        action: ExecutePowerShell
        inputs:
          commands:
            - c:\ec2amibuild\scripts\windows\Disable-Hibernation.ps1
  - name: validate
    steps:
      - name: DiskPercentageFree
        action: ExecutePowerShell
        inputs:
          commands:
            - |
              Function DiskPercentFree {
                [CmdletBinding()]
                Param (
                    [Parameter(Mandatory=$true)]
                    [ValidateNotNullOrEmpty()]
                    [string]$driveLetter,

                    [Parameter(Mandatory=$true)]
                    [ValidateNotNullOrEmpty()]
                    [string]$threshHold
                )
                $disk = Get-PSDrive $driveLetter | Select-Object Used,Free
                $percentage_free = [Math]::round($disk.free/($disk.free+$disk.used) * 100,2)
                if($percentage_free -ge $threshHold) {
                    return 0
                }
                return -1
              }
              DiskPercentFree -driveLetter C -threshHold 10
  - name: test
    steps:
      - name: 'RunTests'
        action: ExecutePowerShell
        inputs:
          commands:
            - c:\ec2amibuild\scripts\windows\TestAMI.ps1
```

## Upload Document to Amazon S3<a name="image-builder-cli-upload-document"></a>

This step is required only if your document exceeds 64 KB\. Smaller documents can be provided inline when you create the EC2 Image Builder component\. Documents over 64 KB must be stored in Amazon S3\. 

```
aws s3 cp component.yaml s3://my-s3-bucket/my-path/component.yaml
```

## Upload Any Resources Referenced by Document<a name="image-builder-cli-upload-resources"></a>

You must upload any resources referenced by your document or your document execution will fail at runtime\. 

```
aws s3 cp my_zip_archive.zip s3://my-s3-bucket/my-path/my_zip_archive.zip
```

## Create a Component<a name="image-builder-cli-create-component"></a>

Next, create a component that references the document that you created as described in the preceding steps\. You will reference this component later in an image recipe used to customize your image\. 

This example assumes we have a file named `create-component.json`\.

```
{
    "name": "MyExampleComponent",
    "semanticVersion": "2019.12.02",
    "description": "An example component that builds, validates and tests an image",
    "changeDescription": "Initial version.",
    "platform": "Windows",
    "uri": "s3://my-s3-bucket/my-path/component.yaml",
    "kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/60763706-b131-418b-8f85-3420912f020c",
    "tags": {
        "MyTagKey": "Some Value"
    }
}
```

To create the component, use the following command\.

```
aws imagebuilder create-component --cli-input-json file://create-component.json
```

## Import a Component<a name="image-builder-cli-import-component"></a>

For some scenarios, it might be easier to start with a pre\-existing script\. For this scenario, you can do the following\. 

This example assumes that you have a file called `import-component.json` \(as shown\)\. Note that the file directly references a PowerShell script called `AdminConfig.ps1` that is already uploaded to `my-s3-bucket`\. Currently, `SHELL` is supported for the component `type`\. 

```
{
    "name": "MyImportedComponent",
    "semanticVersion": "1.0.0",
    "description": "An example of how to import a component",
    "changeDescription": "First commit message.",
    "format": "SHELL",
    "platform": "Windows",
    "type": "BUILD",
    "uri": "s3://my-s3-bucket/AdminConfig.ps1",
    "kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/60763706-b131-418b-8f85-3420912f020c"
}
```

To import the component, run the following command\.

```
aws imagebuilder import-component --cli-input-json file://import-component.json
```

## Create a Basic Image Recipe<a name="image-builder-cli-create-recipe"></a>

After you have the components in place, you can create an image recipe\. An image recipe defines the image to use as your starting point, along with the set of components that customize the image\. The image recipe defines the contents of your output image\. This example shows the use of a basic image recipe, which is the minimal configuration requirement to get started\. 

This image recipe references the two components that you created in the preceding steps\. You must replace the ARNs shown in the example with the ARNs that you received when you created the components\. The AWS Region and account ID will also be different for your configuration\.

**Important**  
Components are installed in the order in which they are specified\.

This example references the Windows Server 2016 English Full Base image\. This ARN references the latest image in the SKU based on the semantic version filters that you have specified\. In this example, the image ARN is `arn:aws:imagebuilder:us-west-2:aws:image/windows-server-2016-english-full-base-x86/2019.*.*`\. The ARN ends with `/2019.*.*`, which communicates to EC2 Image Builder that you want to use the latest AMI created in 2019\. You can provide the specific version that you want to use, or you can use a wildcard in all of the fields\. 

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
    "parentImage": "arn:aws:imagebuilder:us-west-2:aws:image/windows-server-2016-english-full-base-x86/2019.x.x"
}
```

Assuming that you have an image recipe definition stored in `create-image-recipe.json`, you can create the image recipe\. 

```
aws imagebuilder create-image-recipe --cli-input-json file://create-image-recipe.json
```

## Create an Image<a name="image-builder-cli-create-image"></a>

When you have a basic recipe, you can create an image from it\. 

```
aws imagebuilder create-image —image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03
```

## Get an Image<a name="image-builder-cli-get-image"></a>

To check the progress of your image, use the `get-image` operation\. `get-image` returns details about the image, metadata, current state, and output resources when they are available\. 

```
aws imagebuilder get-image —image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-recipe/2019.12.03/1
```

## Create a Distribution Configuration<a name="image-builder-cli-create-distribution-configuration"></a>

A distribution configuration allows you to specify the name and description of your output AMI, authorize other AWS accounts to launch the AMI, and replicate the AMI to other AWS Regions\. It also allows you to export the AMI to Amazon S3\. 

The contents of the `create-distribution-configuration.json` are as follows\.

```
{
    "name": "MyExampleDistribution",
    "description": "Copies AMI to eu-west-1 and exports to S3",
    "distributions": [
        {
            "region": "us-west-2",
            "amiDistributionConfiguration": {
                "name": "Name {{imagebuilder:buildDate}}",
                "description": "An example image name with parameter references",
                "amiTags": {
                    "KeyName": "{{ssm:parameter_name}}"
                },
                "launchPermission": {
                    "userIds": [
                        "987654321012"
                    ]
                }
            }
        },
        {
            "region": "eu-west-1",
            "amiDistributionConfiguration": {
                "name": "My {{imagebuilder:region}} image {{imagebuilder:buildDate}}",
                "amiTags": {
                    "KeyName": "Some value"
                },
                "launchPermission": {
                    "userIds": [
                        "100000000001"
                    ]
                }
            },
            "s3ExportConfiguration": {
                "roleName": "MyExportRole",
                "diskImageFormat": "AMI",
                "s3Bucket": "my-eu-west-1-bucket",
                "s3Prefix": "my-path"
            }
        }
    ]
}
```

Use the JSON file to create the distribution configuration\.

```
aws imagebuilder create-distribution-configuration --cli-input-json file://create-distribution-configuration.json
```

## Create an Infrastructure Configuration<a name="image-builder-cli-create-infrastructure-configuration"></a>

Infrastructure configurations allow you to specify the infrastructure within which to build and test your image\. In the infrastructure configuration, you can specify instance types, subnets, and security groups to associate with your instance\. You can also associate an Amazon EC2 key pair with the instance used to build your image\. This allows you to log on to your instance to troubleshoot if your build fails and you set `terminateInstanceOnFailure` to `false`\. 

```
{
    "name": "MyExampleInfrastructure",
    "description": "An example that will retain instances of failed builds",
    "instanceTypes": [
        "m5.large", "m5.xlarge"
    ],
    "instanceProfileName": "myIAMInstanceProfileName",
    "securityGroupIds": [
        "sg-12345678"
    ],
    "subnetId": "sub-12345678",
    "logging": {
        "s3Logs": {
            "s3BucketName": "my-logging-bucket",
            "s3KeyPrefix": "my-path"
        }
    },
    "keyPair": "myKeyPairName",
    "terminateInstanceOnFailure": false,
    "snsTopicArn": "arn:aws:sns:us-west-2:123456789012:MyTopic"
}
```

The example infrastructure configuration is stored in a file called `create-infrastructure-configuration.json`\.

The example configuration specifies two instance types, `m5.large` and `m5.xlarge`\. We recommend specifying more than one instance type because this allows EC2 Image Builder to launch an instance from a pool with sufficient capacity\. This can reduce your transient build failures\. 

The instance profile name is used to provide the instance with the permissions that are required to perform customization activities\. For example, if you have a component that retrieves resources from Amazon S3, the instance profile requires permissions to access those files\. This instance profile also requires a minimal set of permissions for EC2 Image Builder to successfully communicate with the instance\. For more information, see [Setting Up](getting-started-image-builder.md#image-builder-setting-up)\.

Use the JSON file to create the infrastructure configuration\.

```
aws imagebuilder create-infrastructure-configuration --cli-input-json file://create-infrastructure-configuration.json
```

## Create an Image Pipeline<a name="image-builder-cli-create-image-pipeline"></a>

An image pipeline automates the creation of golden images\. This command is similar to the create\-image step that we performed in the preceding steps\. However, in this case, a pipeline enables you to configure EC2 Image Builder to periodically build new images for you\. 

The build cadence depends on the schedule that you have configured in your pipeline\. A schedule has two attributes: a `scheduleExpression` and a `pipelineExecutionStartCondition`\. The `scheduleExpression` determines how often EC2 Image Builder evaluates your `pipelineExecutionStartCondition`\. When the `pipelineExecutionStartCondition` is set to `EXPRESSION_MATCH_AND_DEPENDENCY_UPDATES_AVAILABLE`, EC2 Image Builder will build a new image only when there are known changes pending\. When it is set to `EXPRESSION_MATCH_ONLY`, it will build a new image every time the CRON expression matches the current time\.

The contents of the `create-image-pipeline.json` are as follows\.

```
{
    "name": "MyWindows2016Pipeline",
    "description": "Builds Windows 2016 Images",
    "imageRecipeArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03",
    "infrastructureConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration",
    "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration",
    "imageTestsConfiguration": {
        "imageTestsEnabled": true,
        "timeoutMinutes": 60
    },
    "schedule": {
        "scheduleExpression": "cron(0 0 * * SUN)",
        "pipelineExecutionStartCondition": "EXPRESSION_MATCH_AND_DEPENDENCY_UPDATES_AVAILABLE"
    },
    "status": "ENABLED"
}
```

Use the JSON file to create the image pipeline\.

```
aws imagebuilder create-image-pipeline --cli-input-json file://create-image-pipeline.json
```

## Cancel an Image Creation<a name="image-builder-cli-cancel-image-creation"></a>

You can use the `cancel-image-creation` API when you want to cancel an image that is in the process of being built\.

```
aws imagebuilder cancel-image-creation —image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-recipe/2019.12.03/1
```

## Apply a Resource Policy to a Component<a name="image-builder-cli-apply-resource-policy-component"></a>

You can apply a resource policy to a build component to enable cross\-account sharing of build components\. This command gives other accounts permission to use your build component in their image recipes\. For the command to be successful, you must ensure that the account with which you are sharing has permission to access any resources referenced by the shared build component, such as files hosted on private repositories\.

```
aws imagebuilder put-component-policy --component-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03/1 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "arn:aws:iam::account-id:user/Alice", "account-id-2" ] }, "Action": [ "imagebuilder:GetComponent", "imagebuilder:ListComponents" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03/1" ] } ] }'
```

## Apply a Resource Policy to an Image Recipe<a name="image-builder-cli-apply-resource-policy-recipe"></a>

You can apply a resource policy to a component to enable cross\-account sharing of image recipes\. This command gives other accounts permission to use your image recipes to create images in their accounts\. For the command to be successful, you must ensure that the account with which you are sharing has permission to access any images or components referenced by the image recipe\.

```
aws imagebuilder put-image-recipe-policy --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image-recipe/2019.12.03 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "arn:aws:iam::account-id:user/Alice", "account-id-2" ] }, "Action": [ "imagebuilder:GetImageRecipe", "imagebuilder:ListImageRecipes" ], "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-image-recipe/2019.12.03" ] } ] }'
```

## Apply a Resource Policy to an Image<a name="image-builder-cli-apply-resource-policy-image"></a>

You can apply a resource policy to an image to allow other users to use the image in their image recipes\. For the command to be successful, you must ensure that the account with which you are sharing has permission to access the underlying resource \(for example, the Amazon EC2 AMI\)\.

```
aws imagebuilder put-image-policy --image-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03 --policy '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": [ "arn:aws:iam::account-id:user/Alice", "account-id-2" ] }, "Action": "imagebuilder:GetComponent", "Resource": [ "arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03" ] } ] }'
```

## Update a Distribution Configuration<a name="image-builder-cli-update-distribution-configuration"></a>

The following example shows an `update-distribution-configuration.json` followed by the CLI command that allows you to update a distribution configuration that references the JSON file\. 

The example `update-distribution-configuration.json` contents are as follows\.

```
{
    "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration",
    "description": "Copies AMI to eu-west-2 and exports to S3",
    "distributions": [
      {
            "region": "us-west-2",
            "amiDistributionConfiguration": {
                "name": "Name {{imagebuilder:buildDate}}",
                "description": "An example image name with parameter references",
                "tags": {
                    "KeyName": "{{ssm:parameter_name}}"
                },
                "launchPermissions": {
                    "userIds": [
                        "987654321012"
                    ]
                }
            }
        },
        {
            "region": "eu-west-2",
            "amiDistributionConfiguration": {
                "name": "My {{imagebuilder:region}} image {{imagebuilder:buildDate}}",
                "tags": {
                    "KeyName": "Some value"
                },
                "launchPermissions": {
                    "userIds": [
                        "100000000001"
                    ]
                }
            },
            "s3ExportConfiguration": {
                "roleName": "MyExportRole",
                "diskImageFormat": "AMI",
                "s3Bucket": "my-eu-west-2-bucket",
                "s3Prefix": "my-path"
            }
        }
    ]
}
```

Run the following command, which references the preceding `update-distribution-configuration.json` file\. 

```
aws imagebuilder update-distribution-configuration --cli-input-json file://update-distribution-configuration.json
```

## Update an Infrastructure Configuration<a name="image-builder-cli-update-infrastructure-configuration"></a>

The following example shows an `update-infrastructure-configuration.json` followed by the CLI command that allows you to update an infrastructure configuration that references the JSON file\. 

 The example `update-infrastructure-configuration.json` contents are as follows\. 

```
{
    "infrastructureConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration",
    "description": "An example that will terminate instances of failed builds",
    "instanceTypes": [
        "m5.large", "m5.2xlarge"
    ],
    "instanceProfileName": "myIAMInstanceProfileName",
    "securityGroupIds": [
        "sg-12345678"
    ],
    "subnetId": "sub-12345678",
    "logging": {
        "s3Logs": {
            "s3BucketName": "my-logging-bucket",
            "s3KeyPrefix": "my-path"
        }
    },
    "terminateInstanceOnFailure": true,
    "snsTopicArn": "arn:aws:sns:us-west-2:123456789012:MyTopic"
}
```

Run the following command, which references the preceding update\-infrastructure\-configuration\.json file\. 

```
aws imagebuilder update-infrastructure-configuration --cli-input-json file://update-infrastructure-configuration.json
```

## Update an Image Pipeline<a name="image-builder-cli-update-image-pipeline"></a>

The following example shows an `update-image-pipeline.json` followed by the CLI command that allows you to update an image pipeline that references the JSON file\. 

The example `update-image-pipeline.json` contents are as follows\.

```
    {
    "imagePipelineArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline",
    "imageRecipeArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.08",
    "infrastructureConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration",
    "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration",
    "imageTestsConfiguration": {
        "imageTestsEnabled": true,
        "timeoutMinutes": 120
    },
    "schedule": {
        "scheduleExpression": "cron(0 0 * * MON)",
        "pipelineExecutionStartCondition": "EXPRESSION_MATCH_AND_DEPENDENCY_UPDATES_AVAILABLE"
    },
    "status": "DISABLED"
}
```

Run the following command, which references the preceding `update-image-pipeline.json` file\. 

```
aws imagebuilder update-image-pipeline --cli-input-json file://update-image-pipeline.json
```

## Start an Image Pipeline Manually<a name="image-builder-cli-start-image-pipeline"></a>

The following CLI example command shows how to manually start an image pipeline\. Running this command results in the pipeline creating a new image on demand\.

```
aws imagebuilder start-image-pipeline-execution --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```

## Tag a Resource<a name="image-builder-cli-tag-resource"></a>

The following example CLI command shows how to add a resource to EC2 Image Builder\. You must provide the `resourceArn` and the tags to apply to it\.

The example `tag-resource.json` contents are as follows\.

```
{
    "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline",
    "tags": {
        "KeyName": "KeyValue"
    }
}
```

Run the following command, which references the preceding `tag-resource.json` file\.

```
aws imagebuilder tag-resource --cli-input-json file://tag-resource.json
```

## Untag a Resource<a name="image-builder-cli-untag-resource"></a>

The following example CLI command shows how to remove a tag from a resource\. You must provide the resourceArn and the keys to remove the tag\.

The example `untag-resource.json` contents are as follows\.

```
{
    "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline",
    "tagKeys": [
        "KeyName"
    ]
}
```

Run the following command, which references the preceding `untag-resource.json` file\.

```
aws imagebuilder untag-resource --cli-input-json file://untag-resource.json
```

## Delete an Image Pipeline<a name="image-builder-cli-delete-image-pipeline"></a>

The following example shows how to delete an image pipeline by specifying its ARN\.

```
aws imagebuilder delete-image-pipeline --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```

**Note**  
Image pipeline
Infrastructure configuration/Distribution configuration/Image recipe
Component
Image

## Delete an Infrastructure Configuration<a name="image-builder-cli-delete-infrastructure-configuration"></a>

The following example shows how to delete a distribution configuration by specifying its ARN\.

```
aws imagebuilder delete-infrastructure-configuration --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration
```

## Delete a Distribution Configuration<a name="image-builder-cli-delete-distribution-configuration"></a>

The following example shows how to delete a distribution configuration by specifying its ARN\.

```
aws imagebuilder delete-distribution-configuration --distribution-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration
```

## Delete an Image Recipe<a name="image-builder-cli-delete-image-recipe"></a>

The following example shows how to delete an image recipe by specifying its ARN\.

```
aws imagebuilder delete-image-recipe --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03
```

## Delete a Component<a name="image-builder-cli-delete-component"></a>

The following example shows how to delete a component build version by specifying its ARN\.

```
aws imagebuilder delete-component --component-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02/1
```

## Delete an Image<a name="image-builder-cli-delete-image"></a>

The following example shows how to delete an image build version by specifying its ARN\.

```
aws imagebuilder delete-image --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.02/1
```

## Get Component Details<a name="image-builder-cli-get-component-details"></a>

The following example shows how to get the details of a component by specifying its ARN\.

```
aws imagebuilder get-component --component-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02/1
```

## Get Component Policy Details<a name="image-builder-cli-get-component-policy-details"></a>

The following example shows how to get the details of a component policy by specifying its ARN\.

```
aws imagebuilder get-component-policy --component-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02
```

## Get Distribution Configuration Details<a name="image-builder-cli-get-distribution-configuration-details"></a>

The following example shows how to get the details of a distribution configuration by specifying its ARN\.

```
aws imagebuilder get-distribution-configuration --distribution-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration
```

## Get Image Details<a name="image-builder-cli-get-image-details"></a>

The following example shows how to get the details of an image by specifying its ARN\.

```
aws imagebuilder get-image --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.02/1
```

## Get Image Pipeline Details<a name="image-builder-cli-get-image-pipeline-details"></a>

The following example shows how to get the details of an image pipeline by specifying its ARN\.

```
aws imagebuilder get-image-pipeline --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```

## Get Image Policy Details<a name="image-builder-cli-get-image-policy-details"></a>

The following example shows how to get the details of an image policy by specifying its ARN\.

```
aws imagebuilder get-image-policy --image-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.02
```

## Get Image Recipe Details<a name="image-builder-cli-get-image-recipe-details"></a>

The following example shows how to get the details of an image recipe by specifying its ARN\.

```
aws imagebuilder get-image-recipe --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03
```

## Get Image Recipe Policy Details<a name="image-builder-cli-get-image-recipe-policy-details"></a>

The following example shows how to get the details of an image recipe policy by specifying its ARN\.

```
aws imagebuilder get-image-recipe-policy --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03
```

## Get an Infrastructure Configuration Details<a name="image-builder-cli-get-infrastructure-configuration-details"></a>

The following example shows how to get the details of an infrastructure configuration by specifying its ARN\.

```
aws imagebuilder get-infrastructure-configuration --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration
```

## List Components<a name="image-builder-cli-list-components"></a>

The following example shows how to list all of the component semantic versions that you have access to\.

```
aws imagebuilder list-components
```

You can optionally filter on whether you want to view components owned by you, by Amazon, or those shared with you by other accounts\. By default, this request will show only components owned by your account\.

```
aws imagebuilder list-components --owner Self
```

```
aws imagebuilder list-components --owner Amazon
```

```
aws imagebuilder list-components --owner Shared
```

## List Component Build Versions<a name="image-builder-cli-list-component-versions"></a>

The following example shows how to list component build versions with a specific semantic version\.

```
aws imagebuilder list-component-build-versions --component-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03
```

## List Distributions<a name="image-builder-cli-list-distributions"></a>

The following example shows how to list all of your distributions\.

```
aws imagebuilder list-distribution-configurations
```

## List Images<a name="image-builder-cli-list-images"></a>

The following example shows how to list all of the image semantic versions that you have access to\.

```
aws imagebuilder list-images
```

## List Image Build Versions<a name="image-builder-cli-list-image-build-versions"></a>

The following example shows how to list image build versions with a specific semantic version\.

```
aws imagebuilder list-image-build-versions --image-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03
```

## List Image Pipeline Images<a name="image-builder-cli-list-image-pipeline-images"></a>

The following example shows how to list all images created by a specific image pipeline\.

```
aws imagebuilder list-image-pipeline-images --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```

## List Image Pipelines<a name="image-builder-cli-list-image-pipelines"></a>

The following example shows how to list all of your image pipelines\.

```
aws imagebuilder list-image-pipelines
```

## List Image Recipes<a name="image-builder-cli-list-image-pipeline-recipes"></a>

The following example shows how to list all of your image recipes\.

```
aws imagebuilder list-image-recipes
```

## List Infrastructure Configurations<a name="image-builder-cli-list-infrastructure-configurations"></a>

The following example shows how to list of all of your infrastructure configurations\.

```
aws imagebuilder list-infrastructure-configurations
```

## List All of the Tags for a Specific Resource<a name="image-builder-cli-list-all-tags-resource"></a>

The following example shows how to list all the tags for a specific resource\.

```
aws imagebuilder list-tags-for-resource —resource-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```
