# Manage components with AWS TOE<a name="manage-components"></a>

Image Builder uses a component management application AWS Task Orchestrator and Executor \(AWS TOE\) to orchestrate complex workflows, modify system configurations, and test your systems\. There is no additional server setup required to use the Image Builder console or to use Image Builder commands that interact with AWS TOE component management on your behalf\.

AWS TOE component management uses YAML documents to define the scripts that customize your image\. The documents can include build, validate, and test phases\. For more information about YAML documents, see [Document schema and definitions](image-builder-application-documents.md#document-schema)\.

You can create the following types of components to add to your recipes:

**Build components**  
Software scripts that define a sequence of steps for downloading, installing, and configuring software packages\. These scripts also define validation steps\.

**Test components**  
A sequence of steps to verify that the output image built by your image pipeline functions as expected\.

**Tip**  
When you have many components to keep track of, tagging helps you to identify a specific component or version based on the tags you've assigned to it\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

This section covers how to list, view, create, and import AWS TOE components, using the Image Builder console, and commands in the AWS CLI\.

**Topics**
+ [List and view component details](component-details.md)
+ [Create a component using the Image Builder console](create-component-console.md)
+ [Create a component \(AWS CLI\)](create-components-cli.md)
+ [Manage AWS TOE component parameters with EC2 Image Builder](manage-component-parameters.md)
+ [Import a component \(AWS CLI\)](#import-component-cli)
+ [Clean up resources](#component-cleanup)

## Import a component \(AWS CLI\)<a name="import-component-cli"></a>

For some scenarios, it might be easier to start with a pre\-existing script\. For this scenario, you can use the following example\. 

This example assumes that you have a file called `import-component.json` \(as shown\)\. Note that the file directly references a PowerShell script called `AdminConfig.ps1` that is already uploaded to `my-s3-bucket`\. Currently, `SHELL` is supported for the component `format`\. 

```
{
"name": "MyImportedComponent",
"semanticVersion": "1.0.0",
"description": "An example of how to import a component",
"changeDescription": "First commit message.",
"format": "SHELL",
"platform": "Windows",
"type": "BUILD",
"uri": "s3://my-s3-bucket/AdminConfig.ps1",
"kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/60763706-b131-418b-8f85-3420912f020c"
}
```

To import the component, run the following command\.

```
aws imagebuilder import-component --cli-input-json file://import-component.json
```

## Clean up resources<a name="component-cleanup"></a>

To avoid unexpected charges, make sure to clean up resources and pipelines that you created from the examples in this guide\. For more information about deleting resources in Image Builder, see [Delete EC2 Image Builder resources](delete-resources.md)\.