# Create images<a name="create-images"></a>

This section covers creating AMI and container images\.

**Topics**
+ [Create an image \(AWS CLI\)](#cli-create-image)
+ [Cancel an image creation \(AWS CLI\)](#image-builder-cli-cancel-image-creation)

## Create an image \(AWS CLI\)<a name="cli-create-image"></a>

When you have a basic recipe and an infrastructure configuration, you can create an image using the imagebuilder create\-image command\.

```
aws imagebuilder create-image --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03 --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2123456789012:infrastructure-configuration/myexampleinfrastructure
```

For information about the resources that are created when you create an image, see [Resources created](how-image-builder-works.md#image-builder-resources)\.

## Cancel an image creation \(AWS CLI\)<a name="image-builder-cli-cancel-image-creation"></a>

To cancel an in\-progress image build, use the imagebuilder cancel\-image\-creation command\.

```
aws imagebuilder cancel-image-creation --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-recipe/2019.12.03/1
```