# Create and update distribution settings for AMIs<a name="crud-ami-distribution-settings"></a>

This section covers creating and updating distribution settings for an Image Builder output AMI\.

**Topics**
+ [Create distribution settings for output AMIs \(AWS CLI\)](#cli-create-ami-distribution-configuration)
+ [Create distribution settings for a faster launching Windows AMI \(AWS CLI\)](#cli-create-ami-dist-config-win-fast-launch)
+ [Create distribution settings for output VM disks \(AWS CLI\)](#cli-create-vm-dist-config)
+ [Update AMI distribution settings \(AWS CLI\)](#cli-update-ami-distribution-configuration)

## Create distribution settings for output AMIs \(AWS CLI\)<a name="cli-create-ami-distribution-configuration"></a>

A distribution configuration allows you to specify the name and description of your output AMI, authorize other AWS accounts to launch the AMI, copy the AMI to other accounts, and replicate the AMI to other AWS Regions\. It also allows you to export the AMI to Amazon Simple Storage Service \(Amazon S3\), or configure faster launching for output Windows AMIs\. To make an AMI public, set the launch permission authorized accounts to `all`\. See the examples for making an AMI public at [EC2 ModifyImageAttribute](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ModifyImageAttribute.html)\.

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

## Create distribution settings for a faster launching Windows AMI \(AWS CLI\)<a name="cli-create-ami-dist-config-win-fast-launch"></a>

The following example shows how to use the create\-distribution\-configuration command to create distribution settings that have faster launching configured for your AMI, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `ami-dist-config-win-faster-launch.json`:

   ```
   {
   "name": "WinFasterLaunchDistribution",
   "description": "An example of Windows AMI faster launching settings in the distribution configuration.",
   "distributions": [
       {
           "region": "us-west-2",
           "amiDistributionConfiguration": {
               "name": "Name {{imagebuilder:buildDate}}",
               "description": "Includes Windows AMI faster launch settings with cross-account distribution.",
               "amiTags": {
                   "KeyName": "Some Value"
               },
               "fastLaunchConfigurations": [{
                   "enabled": true,
                   "snapshotConfiguration": {
                       "targetResourceCount": 5
                   },
                   "maxParallelLaunches": 5,
                   "launchTemplate": {
                       "launchTemplateID": "lt-0ab1234c56d789012",
                       "launchTemplateName": "Launch template for faster launching",
                       "launchTemplateVersion": "1",
                    }
                    "accountId": "123456789012"
               }]
           }
       }
   ]
   }
   ```

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder create-distribution-configuration --cli-input-json file://ami-dist-config-win-faster-launch.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   For more detailed information, see [create\-distribution\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-distribution-configuration.html) in the *AWS CLI Command Reference*\.

## Create distribution settings for output VM disks \(AWS CLI\)<a name="cli-create-vm-dist-config"></a>

The following example shows how to use the create\-distribution\-configuration command to create distribution settings that will export VM image disks to Amazon S3 with every image build\.

1. 

**Create a CLI input JSON file**

   To streamline the imagebuilder create\-distribution\-configuration command that is used in the AWS CLI, we create a JSON file that contains all of the export configuration that we want to pass into the command\.
**Note**  
The naming convention for the data points in the JSON file follows the pattern that is specified for the Image Builder API command request parameters\. To review the API command request parameters, see the [CreateDistributionConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateDistributionConfiguration.html) command in the *EC2 Image Builder API Reference*\.  
Do not use this naming convention for providing these datapoints directly to the imagebuilder create\-distribution\-configuration command as options\.

   Here is a summary of the parameters that we specify in the `s3ExportConfiguration` JSON object for this example:
   + **roleName** \(string, required\) – The name of the role that grants VM Import/Export permission to export images to your S3 bucket\.
   + **diskImageFormat** \(string, required\) – Export the updated disk image to one of the following supported formats:
     + **Virtual Hard Disk \(VHD\)** – Compatible with Citrix Xen and Microsoft Hyper\-V virtualization products\.
     + **Stream\-optimized ESX Virtual Machine Disk \(VMDK\)** – Compatible with VMware ESX and VMware vSphere versions 4, 5, and 6\.
     + **Raw** – Raw format\.
   + **s3Bucket** \(string, required\) – The S3 bucket in which to store the output disk images for your VM\.

   Save the file as `export-vm-disks.json`, to use in the imagebuilder create\-distribution\-configuration command\.

   ```
   {
       "name": "example-distribution-configuration-with-vm-export",
       "description": "example",
       "distributions": [
           {
               "region": "us-west-2",
               "amiDistributionConfiguration": {
               	"description": "example-with-vm-export"
   
               },
               "s3ExportConfiguration": {
                   "roleName": "vmimport",
                   "diskImageFormat": "RAW",
                   "s3Bucket": "vm-bucket-export"
               }
           }],
       "clientToken": "abc123def4567ab"
   }
   ```

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder create-distribution-configuration --cli-input-json file://export-vm-disks.json
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