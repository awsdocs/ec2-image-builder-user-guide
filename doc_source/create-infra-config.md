# Create and update infrastructure configurations<a name="create-infra-config"></a>

This section covers creating and updating EC2 Image Builder infrastructure configurations\.

**Topics**
+ [Create an infrastructure configuration \(AWS CLI\)](#cli-create-infrastructure-configuration)
+ [Update an infrastructure configuration \(AWS CLI\)](#cli-update-infrastructure-configuration)

## Create an infrastructure configuration \(AWS CLI\)<a name="cli-create-infrastructure-configuration"></a>

The following example shows how to use the [create\-infrastructure\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/create-infrastructure-configuration.html) command to configure the infrastructure for your image, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   This infrastructure configuration example specifies two instance types, `m5.large` and `m5.xlarge`\. We recommend specifying more than one instance type because this allows Image Builder to launch an instance from a pool with sufficient capacity\. This can reduce your transient build failures\.

   The `instanceProfileName` specifies the instance profile that provides the instance with the permissions that are required to perform customization activities\. For example, if you have a component that retrieves resources from Amazon S3, the instance profile requires permissions to access those files\. The instance profile also requires a minimal set of permissions for EC2 Image Builder to successfully communicate with the instance\. For more information, see [Prerequisites](image-builder-setting-up.md)\.

   Use your favorite file editing tool to create a JSON file with keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `create-infrastructure-configuration.json`:

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

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder create-infrastructure-configuration --cli-input-json file://create-infrastructure-configuration.json
   ```

## Update an infrastructure configuration \(AWS CLI\)<a name="cli-update-infrastructure-configuration"></a>

The following example shows how to use the [update\-infrastructure\-configuration](https://docs.aws.amazon.com/cli/latest/reference/imagebuilder/update-infrastructure-configuration.html) command to update the infrastructure configuration for your image, using the AWS CLI\.

1. 

**Create a CLI input JSON file**

   This infrastructure configuration example uses the same settings as the create example, except that we have updated the `terminateInstanceOnFailure` setting to `false`\. After we run the update\-infrastructure\-configuration command, pipelines using this infrastructure configuration terminate the build and test instances when the build fails\.

   Use your favorite file editing tool to create a JSON file with keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `update-infrastructure-configuration.json`:

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

1. 

**Run the following command, using the file you created as input\.**

   ```
   aws imagebuilder update-infrastructure-configuration --cli-input-json file://update-infrastructure-configuration.json
   ```