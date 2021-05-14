# Create and update distribution settings for AMIs<a name="crud-ami-distribution-settings"></a>

This section covers creating and updating distribution settings for an Image Builder AMI\.

**Topics**
+ [Create distribution settings for Image Builder AMIs \(AWS CLI\)](#cli-create-ami-distribution-configuration)
+ [Update AMI distribution settings \(AWS CLI\)](#cli-update-ami-distribution-configuration)

## Create distribution settings for Image Builder AMIs \(AWS CLI\)<a name="cli-create-ami-distribution-configuration"></a>

A distribution configuration allows you to specify the name and description of your output AMI, authorize other AWS accounts to launch the AMI, copy the AMI to other accounts, and replicate the AMI to other AWS Regions\. It also allows you to export the AMI to Amazon Simple Storage Service \(Amazon S3\)\. To make an AMI public, set the launch permission authorized accounts to `all`\. See the examples for making an AMI public at [EC2 ModifyImageAttribute](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyImageAttribute.html)\. 

The following example shows how to use the create\-distribution\-configuration command to create distribution settings for your AMI, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `create-ami-distribution-configuration.json`:

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
                       "KeyName": "Some Value"
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
                   "name": "My {{imagebuilder:buildVersion}} image {{imagebuilder:buildDate}}",
                   "amiTags": {
                       "KeyName": "Some value"
                   },
                   "launchPermission": {
                       "userIds": [
                           "100000000001"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder create-distribution-configuration --cli-input-json file://create-ami-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [create\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-distribution-configuration.html) in the *AWS CLI Command Reference*\.

## Update AMI distribution settings \(AWS CLI\)<a name="cli-update-ami-distribution-configuration"></a>

The following example shows how to use the update\-distribution\-configuration command to update distribution settings for your AMI, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with the keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `update-ami-distribution-configuration.json`\.

   ```
   {
       "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/update-ami-distribution-configuration.json",
       "description": "Copies AMI to eu-west-2 and exports to S3",
       "distributions": [
         {
               "region": "us-west-2",
               "amiDistributionConfiguration": {
                   "name": "Name {{imagebuilder:buildDate}}",
                   "description": "An example image name with parameter references",
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
                   "name": "My {{imagebuilder:buildVersion}} image {{imagebuilder:buildDate}}",
                   "tags": {
                       "KeyName": "Some value"
                   },
                   "launchPermissions": {
                       "userIds": [
                           "100000000001"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder update-distribution-configuration --cli-input-json file://update-ami-distribution-configuration.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [update\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/update-distribution-configuration.html) in the *AWS CLI Command Reference*\. To update tags for your distribution configuration resource, see the [Tag resources](tag-resources.md) section\.