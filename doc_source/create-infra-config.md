# Create and update infrastructure configurations<a name="create-infra-config"></a>

This section covers creating and updating EC2 Image Builder infrastructure configurations\.

**Topics**
+ [Create an infrastructure configuration \(AWS CLI\)](#cli-create-infrastructure-configuration)
+ [Update an infrastructure configuration \(AWS CLI\)](#cli-update-infrastructure-configuration)

## Create an infrastructure configuration \(AWS CLI\)<a name="cli-create-infrastructure-configuration"></a>

Infrastructure configurations allow you to specify the infrastructure within which to build and test your image\. In the infrastructure configuration, you can specify instance types, subnets, and security groups to associate with your instance\. You can also associate an Amazon EC2 key pair with the instance used to build your image\. This allows you to log on to your instance to troubleshoot if your build fails and you set `terminateInstanceOnFailure` to `false`\. If you configure logging, the instance profile specified in your infrastructure configuration must have `s3:PutObject` permissions for the target bucket \(`arn:aws:s3:::BucketName/*`\)\. 

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

The instance profile name is used to provide the instance with the permissions that are required to perform customization activities\. For example, if you have a component that retrieves resources from Amazon S3, the instance profile requires permissions to access those files\. This instance profile also requires a minimal set of permissions for EC2 Image Builder to successfully communicate with the instance\. For more information, see [Prerequisites](image-builder-setting-up.md)\.

Use the JSON file to create the infrastructure configuration\.

```
aws imagebuilder create-infrastructure-configuration --cli-input-json file://create-infrastructure-configuration.json
```

## Update an infrastructure configuration \(AWS CLI\)<a name="cli-update-infrastructure-configuration"></a>

The following example shows an imageguilder update\-infrastructure\-configuration\.json command, followed by the AWS CLI command that allows you to update an infrastructure configuration that references the JSON file\. 

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