# List and view distribution settings detail<a name="distribution-settings-detail"></a>

This section describes the various ways that you can find information and view details for your EC2 Image Builder distribution settings\.

**Topics**
+ [List distributions \(AWS CLI\)](#cli-list-distributions)
+ [Get distribution settings detail \(AWS CLI\)](#cli-get-distribution-configuration)

## List distributions \(AWS CLI\)<a name="cli-list-distributions"></a>

The following example shows how to use the imagebuilder list\-distribution\-configurations command in the AWS CLI to list all of your distributions\.

```
aws imagebuilder list-distribution-configurations
```

## Get distribution settings detail \(AWS CLI\)<a name="cli-get-distribution-configuration"></a>

The following example shows how to use the imagebuilder get\-distribution\-configuration command in the AWS CLI to get the details of a distribution settings configuration by specifying its Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-distribution-configuration --distribution-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration
```