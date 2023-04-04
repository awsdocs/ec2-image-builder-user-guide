# List and view infrastructure configuration details<a name="infra-config-details"></a>

This section describes the various ways that you can find information and view details for your EC2 Image Builder infrastructure configurations\.

**Topics**
+ [List infrastructure configurations \(AWS CLI\)](#cli-list-infrastructure-configurations)
+ [Get infrastructure configuration details \(AWS CLI\)](#cli-get-infrastructure-configuration-details)

## List infrastructure configurations \(AWS CLI\)<a name="cli-list-infrastructure-configurations"></a>

The following example shows how to list of all of your infrastructure configurations, using the [list\-infrastructure\-configurations](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-infrastructure-configurations.html) command in the AWS CLI\.

```
aws imagebuilder list-infrastructure-configurations
```

## Get infrastructure configuration details \(AWS CLI\)<a name="cli-get-infrastructure-configuration-details"></a>

The following example shows how use the [get\-infrastructure\-configuration](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-infrastructure-configuration.html) command in the AWS CLI to get the details of an infrastructure configuration by specifying its Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-infrastructure-configuration --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration
```