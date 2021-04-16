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