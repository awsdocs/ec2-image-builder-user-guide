# Create and update distribution settings for container images<a name="crud-container-distribution-settings"></a>

This section covers creating and updating distribution settings for Image Builder container images\.

**Topics**
+ [Create distribution settings for Image Builder container images \(AWS CLI\)](#cli-create-container-distribution-configuration)
+ [Update distribution settings for your container image \(AWS CLI\)](#cli-update-container-distribution-configuration)

## Create distribution settings for Image Builder container images \(AWS CLI\)<a name="cli-create-container-distribution-configuration"></a>

A distribution configuration enables you to specify the name and description of your output container image and replicate the container image to other AWS Regions\. You can also apply separate tags to the distribution configuration resource and to the container images within each Region\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with the keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `create-container-distribution-configuration.json`:

   ```
   {
   	"name": "distribution-configuration-name",
   	"description": "Distributes container image to Amazon ECR repository in two regions.",
   	"distributions": [
   	    {
   	        "region": "us-west-2",
   	        "containerDistributionConfiguration": {
   	            "description": "My test image.",
   	            "targetRepository": {
   	                "service": "ECR",
   	                "repositoryName": "testrepo"
   	            },
   	            "containerTags": ["west2", "image1"]
   	        }
   	    },
   	    {
   	        "region": "us-east-1",
   	        "containerDistributionConfiguration": {
   	            "description": "My test image.",
   	            "targetRepository": {
   	                "service": "ECR",
   	                "repositoryName": "testrepo"
   	            },
   	           "containerTags": ["east1", "imagedist"]
   	        }
   	    }
   	],
   	"tags": {
   	   "DistributionConfigurationTestTagKey1": "DistributionConfigurationTestTagValue1",
   	   "DistributionConfigurationTestTagKey2": "DistributionConfigurationTestTagValue2"
   	}
   }
   ```

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder create-distribution-configuration --cli-input-json file://create-container-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [create\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-distribution-configuration.html) in the *AWS CLI Command Reference*\.

## Update distribution settings for your container image \(AWS CLI\)<a name="cli-update-container-distribution-configuration"></a>

The following example shows how to use the update\-distribution\-configuration command to update distribution settings for your container image, using the AWS CLI\. You can also update tags for the container images within each Region\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `update-container-distribution-configuration.json`:

   ```
   {
       "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/update-container-distribution-configuration.json",
   	"description": "Distributes container image to Amazon ECR repository in two regions.",
   	"distributions": [
   	    {
   	        "region": "us-west-2",
   	        "containerDistributionConfiguration": {
   	            "description": "My test image.",
   	            "targetRepository": {
   	                "service": "ECR",
   	                "repositoryName": "testrepo"
   	            },
   	            "containerTags": ["west2", "image1"]
   	        }
   	    },
   	    {
   	        "region": "us-east-2",
   	        "containerDistributionConfiguration": {
   	            "description": "My test image.",
   	            "targetRepository": {
   	                "service": "ECR",
   	                "repositoryName": "testrepo"
   	            },
   	           "containerTags": ["east2", "imagedist"]
   	        }
   	    }
   	]
   }
   ```

1. 

**Run the following command, using the file you created as input:**

   ```
   aws imagebuilder update-distribution-configuration --cli-input-json file://update-container-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [update\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/update-distribution-configuration.html) in the *AWS CLI Command Reference*\. To update tags for your distribution configuration resource, see the [Tag resources](tag-resources.md) section\.