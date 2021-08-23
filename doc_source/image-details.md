# List and view image details<a name="image-details"></a>

This section describes the various ways that you can find information and view details for your EC2 Image Builder images\.

**Topics**
+ [List images \(AWS CLI\)](#cli-list-images)
+ [List image build versions \(AWS CLI\)](#cli-list-image-build-versions)
+ [Get an AMI image \(AWS CLI\)](#cli-get-image)
+ [Get image policy details \(AWS CLI\)](#cli-get-image-policy-details)

## List images \(AWS CLI\)<a name="cli-list-images"></a>

The following example shows how to list all of the image versions that you have access to\.

```
aws imagebuilder list-images
```

## List image build versions \(AWS CLI\)<a name="cli-list-image-build-versions"></a>

The following example shows how to list image build versions with a specific semantic version\.

```
aws imagebuilder list-image-build-versions --image-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.03
```

**Note**  
To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.

## Get an AMI image \(AWS CLI\)<a name="cli-get-image"></a>

To check the progress of your image, use the `get-image` operation\. `get-image` returns details about the image, metadata, current state, and output resources when they are available\. 

```
aws imagebuilder get-image --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-recipe/2019.12.03/1
```

## Get image policy details \(AWS CLI\)<a name="cli-get-image-policy-details"></a>

The following example shows how to get the details of an image policy by specifying its Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-image-policy --image-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.02
```