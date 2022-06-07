# Create images<a name="create-images"></a>

This section covers creating AMI and container images\.

**Topics**
+ [Create an image \(AWS CLI\)](#cli-create-image)
+ [Import a VM \(console\)](#import-image-console)
+ [Import a VM \(AWS CLI\)](#import-image-cli)

## Create an image \(AWS CLI\)<a name="cli-create-image"></a>

When you have a basic recipe and an infrastructure configuration, you can use the create\-image command to create an image\.

```
aws imagebuilder create-image --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03 --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2123456789012:infrastructure-configuration/myexampleinfrastructure
```

## Import a VM \(console\)<a name="import-image-console"></a>

To import a VM from disks into an AMI, and create an Image Builder image resource that you can use in your recipes, follow these steps:

**General**

1. Specify a unique **Name** for the base image\.

1. Specify a **Version** for the base image\. Use the following format: `major.minor.patch`\.

1. You can also enter an optional **Description** for the base image\.

**Base image operating system**

1. Select the **Image Operating System \(OS\)** option that matches your VM OS platform\.

1. Select the **OS version** that matches the version for your VM from the list\.

### VM import configuration<a name="import-vm-image-console-config"></a>

When you export your VM from its virtualization environment, that process creates a set of one or more disk container files\. These act as snapshots of your VM’s environment, settings, and data\. You can use these files to import your VM as the base image for your image recipe\. For more information about importing VMs in Image Builder, see [Import and export VM images](vm-import-export.md)\.

To specify the location of your import source, follow these steps:

**Import source**  
Specify the source for the first VM image disk container or snapshot to import in the **Disk container 1** section\.

1. **Source** – This can be either an S3 bucket, or an EBS snapshot\.

1. **Select S3 location of disk** – Enter the location in Amazon S3 where your disk images are stored\. To browse for the location, choose **Browse S3**\.

1. To add a disk container, choose **Add disk container**\.

**IAM role**  
To associate an IAM role with your VM import configuration, select the role from the **IAM role** dropdown list, or choose **Create new role** to create a new one\. If you create a new role, the IAM Roles console page opens in a separate tab\.

#### Advanced settings – *optional*<a name="import-vm-image-console-opt"></a>

The following settings are optional\. With these settings, you can configure encryption, licensing, tags, and more for the base image that the import creates\.

**Base image architecture**  
To specify the architecture of your VM import source, select a value from the **Architecture** list\.

**Encryption**  
If your VM disk images are encrypted, you must provide a key to use for the import process\. To specify a KMS key for the import, select a value from the **Encryption \(KMS key\)** list\. The list contains KMS keys that your account has access to in the current Region\.

**License management**  
When you import a VM, the import process automatically detects the VM OS and applies the appropriate license to the base image\. Depending on your OS platform, the license types are as follows:
+ **License included** – An appropriate AWS license for your platform is applied to your base image\.
+ **Bring your own license \(BYOL\)** – Retains the license from your VM, if applicable\.

To attach license configurations created with AWS License Manager to your base image, select from the **License configuration name** list\. For more information about License Manager, see [Working with AWS License Manager]()

**Note**  
License configurations contain licensing rules based on the terms of your enterprise agreements\.
Linux only supports BYOL licenses\.

**Tags \(base image\)**  
Tags use key\-value pairs to assign searchable text to your Image Builder resource\. To specify tags for the imported base image, enter key\-value pairs using the **Key** and **Value** boxes\.

To add a tag, choose **Add tag**\. To remove a tag, choose **Remove tag**\.

## Import a VM \(AWS CLI\)<a name="import-image-cli"></a>

Image Builder integrates with the Amazon EC2 VM Import/Export API to enable the import process to run asynchronously in the background\. The Image Builder [import\-vm\-image](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/import-vm-image.html) command references the task ID from the VM import to track its progress, and creates an Image Builder image resource as output\. This allows you to reference the Image Builder image resource in your recipes before the VM import finishes\.

To import a VM from disks into an AMI and create an Image Builder image resource that you can reference right away, follow these steps:

1. Initiate a VM import, using the Amazon EC2 VM Import/Export import\-image command in the AWS CLI\. Make note of the task ID that is returned in the command response\. You'll need it for the next step\. For more information, see [Importing a VM as an image using VM Import/Export](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html) in the *VM Import/Export User Guide*\.

1. 

**Create a CLI input JSON file**

   To streamline the Image Builder import\-vm\-image command that is used in the AWS CLI, we create a JSON file that contains all of the import configuration that we want to pass into the command\.
**Note**  
The naming convention for the data points in the JSON file follows the pattern that is specified for the Image Builder API command request parameters\. To review the API command request parameters, see the [ImportVmImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ImportVmImage.html) command in the *EC2 Image Builder API Reference*\.  
Do not use this naming convention for providing these datapoints directly to the Image Builder import\-vm\-image command as options\.

   Here is a summary of the parameters that we specify in this example:
   + **name** \(string, required\) – The name for the Image Builder image resource to create as output from the import\.
   + **semanticVersion** \(string, required\) – The semantic version for the output image that specifies the version in the following format, with numeric values in each position to indicate a specific version: <major>\.<minor>\.<patch>\. For example, `1.0.0`\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.
   + **description** \(string\) – The description of the image recipe\.
   + **platform** \(string, required\) – The operating system platform for the imported VM\.
   + **vmImportTaskId** \(string, required\) – The `ImportTaskId` \(AWS CLI\) from the Amazon EC2 VM import process\. Image Builder monitors the import process to pull in the AMI that it creates and build an Image Builder image resource that can be used in recipes right away\.
   + **clientToken** \(string, required\) – A unique, case\-sensitive identifier you provide to ensure idempotency of the request\. For more information, see [Ensuring idempotency](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/Run_Instance_Idempotency.html) in the *Amazon EC2 API Reference*\.
   + **tags** \(string map\) – Tags are key\-value pairs that are attached to the import resources\. Up to 50 key\-value pairs are allowed\.

   Save the file as `import-vm-image.json`, to use in the Image Builder import\-vm\-image command\.

   ```
   {
       "name": "example-request",
       "semanticVersion": "1.0.0",
       "description": "vm-import-test",
       "platform": "Linux",
       "vmImportTaskId": "import-ami-01ab234567890cd1e",
       "clientToken": "asz1231231234cs3z",
       "tags": {
       	"Usage": "VMIE"
       }
   }
   ```

1. 

**Import the image**

   Run the following command, using the file that you created as input:

   ```
   aws imagebuilder import-vm-image --cli-input-json file://import-vm-image.json
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.