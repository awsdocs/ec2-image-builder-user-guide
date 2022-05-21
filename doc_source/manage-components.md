# Manage components with AWSTOE<a name="manage-components"></a>

Image Builder uses the AWS Task Orchestrator and Executor \(AWSTOE\) component management application to orchestrate complex workflows\. AWSTOE components are based on YAML documents that define the scripts to customize or test your image\. For AMI images, Image Builder installs AWSTOE components and the component application on its Amazon EC2 build and test instances\. For container images, the components and component application are installed inside of the running container\. 

Image Builder uses AWSTOE to perform all on\-instance activities\. There is no additional setup required to interact with AWSTOE when you run Image Builder commands or use the Image Builder console\.

**Note**  
When a component that is managed by Amazon reaches the end of its support lifespan, it is no longer maintained\. About four weeks before this occurs, any accounts that are using the component receive notification, and a list of the affected recipes in their account from their AWS Health Dashboard\. To learn more about AWS Health, see [AWS Health User Guide](https://docs.aws.amazon.com/health/latest/ug/)\.

**Workflow stages for building a new image**  
The Image Builder workflow for building new images includes the following two distinct stages\.

1. **Build stage** \(pre\-snapshot\) – During the build stage, you make changes to the Amazon EC2 build instance that's running your base image, to create the baseline for your new image\. For example, your image or container recipe can include components that install an application, or modify the operating system firewall settings\.

   The following component phases run during the build stage:
   + build
   + validate

   After this stage completes successfully, Image Builder creates a snapshot or container image that it uses for the test stage and beyond\.

1. **Test stage** \(post\-snapshot\) – During the test stage, Image Builder launches an EC2 instance from the snapshot or container image that was created as the final step of the build stage\. Tests run on the new instance to validate settings and ensure that the instance is functioning as expected\.

   The following component phase runs for every component that is included in the recipe during the test stage: 
   + test

   This component phase applies to both Build and Test component types\. After this stage completes successfully, Image Builder can create and distribute your final image from the snapshot or the container image\.

**Note**  
While AWSTOE allows you to define many phases in a component document, Image Builder has strict rules about what phases it runs, and during which stages it runs them\. For a component to run during the build stage, the component document must define at least one of these phases: `build` or `validate`\. For a component to run during the test stage, the component document must define the `test` phase, and no other phases\.  
Since Image Builder runs the stages independently, chaining references in AWSTOE documents cannot cross stage boundaries\. You cannot chain a value from a phase that runs in the build stage to a phase that runs in the test stage\. You can, however, define input parameters to the intended target, and pass in values through the command line\. For more information about setting component parameters in your Image Builder recipes, see [Manage AWSTOE component parameters with EC2 Image Builder](manage-component-parameters.md)\.

To assist with troubleshooting on your build or test instance AWSTOE creates a log folder that contains the input document and log files to track what's happening each time a component runs\. If you configured an Amazon S3 bucket in your pipeline configuration, the logs are also written there\. For more information about YAML documents and log output, see [Use component documents in AWSTOE](toe-use-documents.md)\.

**Tip**  
When you have many components to keep track of, tagging helps you to identify a specific component or version based on the tags you've assigned to it\. For more information about tagging your resources using Image Builder commands in the AWS CLI, see the [Tag resources](tag-resources.md) section of this guide\.

This section covers how to list, view, create, and import AWSTOE components, using the Image Builder console, and commands in the AWS CLI\.

**Topics**
+ [List and view component details](component-details.md)
+ [Create a component using the Image Builder console](create-component-console.md)
+ [Create a component \(AWS CLI\)](create-components-cli.md)
+ [Manage AWSTOE component parameters with EC2 Image Builder](manage-component-parameters.md)
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