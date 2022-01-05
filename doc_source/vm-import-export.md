# Import and export virtual machine \(VM\) images with EC2 Image Builder<a name="vm-import-export"></a>

When you export your VM from its virtualization environment, that process creates a set of one or more disk container files that act as snapshots of your VM’s environment, settings, and data\. You can use these files to import your VM, and use it as the base image for your image recipes\.

Image Builder supports the following file formats for your VM disk containers:
+ Open Virtualization Archive \(OVA\)
+ Virtual Machine Disk \(VMDK\)
+ Virtual Hard Disk \(VHD/VHDX\)
+ Raw

The import uses the disks to create an Amazon Machine Image \(AMI\) and an Image Builder image resource, either of which can serve as the base image for your custom image recipe\. The VM disks must be stored in S3 buckets for the import\. Alternatively, you can import from an existing EBS snapshot\.

In the Image Builder console, you can import the image directly, and then use the output image or AMI in your recipes, or you can specify import parameters when you are creating your recipe or recipe version\. For more information about importing directly, see [Import a VM \(console\)](create-images.md#import-image-console)\. For more information about importing as part of your image recipe, see [VM import configuration](create-image-recipes.md#import-vm-recipe-console-config)\.

## Import a VM into Image Builder \(AWS CLI\)<a name="vmie-import"></a>

Image Builder integrates with the Amazon EC2 VM Import/Export API to enable the import process to run asynchronously in the background\. The Image Builder import\-vm\-image command references the task ID from the VM import to track its progress, and creates an Image Builder image resource as output\. This allows you to reference the Image Builder image resource in your recipes before the VM import finishes\.

To import a VM from disks into an AMI and create an Image Builder image resource that you can reference right away, follow these steps:

1. Initiate a VM import, using the Amazon EC2 VM Import/Export import\-image command in the AWS CLI\. Make note of the task ID that is returned in the command response\. You'll need it for the next step\. For more information, see [Importing a VM as an image using VM Import/Export](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html) in the *VM Import/Export User Guide*\.

1. 

**Create a CLI input JSON file**

   To streamline the imagebuilder import\-vm\-image command that is used in the AWS CLI, we create a JSON file that contains all of the import configuration that we want to pass into the command\.
**Note**  
The naming convention for the data points in the JSON file follows the pattern that is specified for the Image Builder API command request parameters\. To review the API command request parameters, see the [ImportVmImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ImportVmImage.html) command in the *EC2 Image Builder API Reference*\.  
Do not use this naming convention for providing these datapoints directly to the imagebuilder import\-vm\-image command as options\.

   Here is a summary of the parameters that we specify in this example:
   + **name** \(string, required\) – The name for the Image Builder image resource to create as output from the import\.
   + **semanticVersion** \(string, required\) – The semantic version for the output image that specifies the version in the following format, with numeric values in each position to indicate a specific version: <major>\.<minor>\.<patch>\. For example, `1.0.0`\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.
   + **description** \(string\) – The description of the image recipe\.
   + **platform** \(string, required\) – The operating system platform for the imported VM\.
   + **vmImportTaskId** \(string, required\) – The `ImportTaskId` \(AWS CLI\) from the Amazon EC2 VM import process\. Image Builder monitors the import process to pull in the AMI that it creates and build an Image Builder image resource that can be used in recipes right away\.
   + **clientToken** \(string, required\) – A unique, case\-sensitive identifier you provide to ensure idempotency of the request\. For more information, see [Ensuring idempotency](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/Run_Instance_Idempotency.html) in the *Amazon EC2 API Reference*\.
   + **tags** \(string map\) – Tags are key\-value pairs that are attached to the import resources\. Up to 50 key\-value pairs are allowed\.

   Save the file as `import-vm-image.json`, to use in the imagebuilder import\-vm\-image command\.

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

## Distribute VM disks from your image build \(AWS CLI\)<a name="vmie-export"></a>

You can set up distribution of supported VM disk format files to S3 buckets in target Regions as part of your regular image build process, using Image Builder distribution configurations in the AWS CLI\. For more information, see [Create distribution settings for output VM disks \(AWS CLI\)](crud-ami-distribution-settings.md#cli-create-vm-dist-config)\.