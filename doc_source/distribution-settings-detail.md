# List and view distribution settings detail<a name="distribution-settings-detail"></a>

This section describes the various ways that you can find information and view details for your EC2 Image Builder distribution settings\.

**Topics**
+ [List distribution configurations \(console\)](#list-distribution-config-console)
+ [View distribution configuration details \(console\)](#view-distribution-config-details-console)
+ [List distributions \(AWS CLI\)](#cli-list-distributions)
+ [Get distribution configuration detail \(AWS CLI\)](#cli-get-distribution-configuration)

## List distribution configurations \(console\)<a name="list-distribution-config-console"></a>

To see a list of the distribution configurations created under your account in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Distribution settings** from the navigation pane\. This shows a list of the distribution configurations that are created under your account\.

1. To view details or create new distribution configuration, choose the **Configuration name** link\. This opens the detail view for the distribution settings\.
**Note**  
You can also select the check box next to the **Configuration name**, then choose **View details**\.

## View distribution configuration details \(console\)<a name="view-distribution-config-details-console"></a>

To view details for a specific distribution configuration using the Image Builder console, select the configuration to review, using the steps described in [List distribution configurations \(console\)](#list-distribution-config-console)\.

On the distribution detail page, you can:
+ **Delete** the distribution configuration\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.
+ **Edit** distribution details\.

## List distributions \(AWS CLI\)<a name="cli-list-distributions"></a>

The following example shows how to use the imagebuilder list\-distribution\-configurations command in the AWS CLI to list all of your distributions\.

```
aws imagebuilder list-distribution-configurations
```

## Get distribution configuration detail \(AWS CLI\)<a name="cli-get-distribution-configuration"></a>

The following example shows how to use the imagebuilder get\-distribution\-configuration command in the AWS CLI to get the details of a distribution configuration by specifying its Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-distribution-configuration --distribution-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration
```