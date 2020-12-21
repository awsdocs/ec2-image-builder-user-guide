# Create and update distribution settings<a name="create-distribution-settings"></a>

This section covers creating and updating distribution settings\.

**Topics**
+ [Create distribution settings \(AWS CLI\)](#cli-create-distribution-configuration)
+ [Update distribution settings \(AWS CLI\)](#cli-update-distribution-configuration)

## Create distribution settings \(AWS CLI\)<a name="cli-create-distribution-configuration"></a>

A distribution configuration allows you to specify the name and description of your output AMI, authorize other AWS accounts to launch the AMI, copy the AMI to other accounts, and replicate the AMI to other AWS Regions\. It also allows you to export the AMI to Amazon Simple Storage Service \(Amazon S3\)\. To make an AMI public, set the launch permission authorized accounts to `all`\. See the examples for making an AMI public at [EC2 ModifyImageAttribute](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyImageAttribute.html)\. 

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

Use the JSON file to create the distribution configuration\.

```
aws imagebuilder create-distribution-configuration --cli-input-json file://create-distribution-configuration.json
```

**Copy an AMI created with Image Builder to other accounts**  
You can copy an AMI created with an Image Builder pipeline to other accounts that you specify\. When you run cross\-account distribution, the AMI created with an Image Builder image build is copied from the source account to the destination account using the Amazon EC2 [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CopyImage.html) API\. The destination account can then launch or modify the AMI as needed\. 

To copy an AMI to other accounts, the following AWS Identity and Access Management \(IAM\) prerequisite must be met:

1. Create a new IAM role in all of the destination accounts called `EC2ImageBuilderDistributionCrossAccountRole`\. 

1. Attach the managed policy called `Ec2ImageBuilderCrossAccountDistributionAccess` to the role\. For more information about managed policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies)\.

1. Verify that the source account ID is added to the trust policy attached to the IAM role of the destination account\. For more information about trust policies, see [Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based) in the *AWS Identity and Access Management User Guide*\.

To copy an image created by Image Builder to another account, create a `DistributionConfiguration` JSON that specifies the `targetAccountIds` in the `AmiDistributionConfiguration` settings\. You must specify at least one `AmiDistributionConfiguration` in the source Region\.

Attach an image recipe, an infrastructure configuration, and a distribution configuration to a [Create an image \(AWS CLI\)](create-images.md#cli-create-image) request to create an image or image pipeline resource\.

The contents of the `create-distribution-configuration.json` configured for cross\-account image distribution are as follows\.

```
{
    "name": "cross-account-distribution-example",
    "description": "Cross Account Distribution Configuration Example",
    "distributions": [
        {
            "amiDistributionConfiguration": {
                "targetAccountIds": ["123456789012", "987654321098"],
                "name": "Name {{ imagebuilder:buildDate }}", 
                "description": "ImageCopy Ami Copy Configuration"
            }, 
            "region": "us-west-2"
        }
    ]
}
```

**Limits for cross\-account distribution**
+ The destination account is limited to 50 concurrent AMI copies per destination Region\.
+ You cannot copy a paravirtual \(PV\) virtualization AMI to a Region that does not support PV virtualization AMIs\. For more information, see [Linux AMI virtualization types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html)\.
+ You cannot create an unencrypted copy of an encrypted snapshot\. The default CMK for EBS is used unless you specify a non\-default AWS Key Management Service \(AWS KMS\) CMK using `KmsKeyId`\. For more information, see [Amazon EBS Encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) in the *Amazon Elastic Compute Cloud User Guide*\.

For more information, see [CreateDistributionConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateDistributionConfiguration.html) in the *EC2 Image Builder API Reference*\.

## Update distribution settings \(AWS CLI\)<a name="cli-update-distribution-configuration"></a>

The following example shows an `update-distribution-configuration.json` followed by the AWS CLI command that allows you to update a distribution configuration that references the JSON file\. 

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

Run the following command, which references the preceding `update-distribution-configuration.json` file\. 

```
aws imagebuilder update-distribution-configuration --cli-input-json file://update-distribution-configuration.json
```