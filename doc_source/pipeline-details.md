# List and view pipeline details<a name="pipeline-details"></a>

This section describes the various ways that you can find information and view details for your EC2 Image Builder image pipelines\.

**Topics**
+ [List image pipelines \(AWS CLI\)](#cli-list-image-pipelines)
+ [Get image pipeline details \(AWS CLI\)](#cli-get-image-pipeline-details)

## List image pipelines \(AWS CLI\)<a name="cli-list-image-pipelines"></a>

The following example shows how to use the imagebuilder list\-image\-pipelines command in the AWS CLI to list all of your image pipelines\.

```
aws imagebuilder list-image-pipelines
```

## Get image pipeline details \(AWS CLI\)<a name="cli-get-image-pipeline-details"></a>

The following example shows how to use the imagebuilder get\-image\-pipeline command in the AWS CLI to get the details of an image pipeline by specifying its ARN\.

```
aws imagebuilder get-image-pipeline --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```