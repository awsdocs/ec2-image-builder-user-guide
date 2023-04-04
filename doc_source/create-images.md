# Create images<a name="create-images"></a>

This section shows you how to create Image Builder images and cancel a build that's in progress\.

**Topics**
+ [Create an image](#image-create)
+ [Cancel image creation \(AWS CLI\)](#cli-cancel-image-creation)

## Create an image<a name="image-create"></a>

There are several different ways that you can create a new Image Builder image\. For example, you can use one of the following methods to create an image with the AWS Management Console or AWS CLI\. You can also use the [CreateImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImage.html) API action\. For the associated SDK request, you can refer to the [See Also](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImage.html#API_CreateImage_SeeAlso) link for that command in the *EC2 Image Builder API Reference*\.

------
#### [ AWS Management Console ]

To create an image in the AWS Management Console, you can manually run an existing pipeline, as follows:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Image pipelines** from the navigation pane\.

1. Select the check box next to the **Pipeline name** that you want to run\.

1. To create the image, select **Run pipeline** from the **Actions** menu\. This starts the pipeline\.

You can also specify a schedule to run your pipeline, or use Amazon EventBridge to run your pipeline based on rules that you configure\.

------
#### [ AWS CLI ]

Before you run the [create\-image](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/create-image.html) command in the AWS CLI, you'll need to create the following resources if they don't already exist:

**Required resources**
+ **Recipe** – You must specify exactly one recipe for your image\. Use the `--image-recipe-arn` parameter to specify the Amazon Resource Name \(ARN\) for your image recipe resource, or the `--container-recipe-arn` parameter to specify the ARN for your container recipe resource\.
+ **Infrastructure configuration** – Use the `--infrastructure-configuration-arn` parameter to specify the ARN for your infrastructure configuration resource\.

You can also specify any of the following resources that your image requires:

**Optional resources and configuration**
+ **Distribution configuration** – By default, Image Builder distributes the output image resource to your account in the Region where you run the create\-image command\. To provide additional destinations or configuration for your distribution, use the `--distribution-configuration-arn` parameter to specify the ARN for your distribution configuration resource\.
+ **Image scanning** – Use the `--image-scanning-configuration` parameter to configure snapshots for Amazon Inspector findings on your image or container build instance and to specify a scanning repository in Amazon ECR for container images\.
+ **Image tests** – Use the `--image-tests-configuration` parameter to suppress the Image Builder test stage, or set a timeout for how long it can run\.
+ **Image tags** – Use the `--tags` parameter to add tags to your output image\.

The following command example shows how create an image in the AWS CLI, with the [create\-image](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/create-image.html) command in the *AWS CLI Command Reference*\.

**Example: Create a basic image with default distribution**

```
aws imagebuilder create-image --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/recipe-name/1.0.0 --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/infrastructure-config-name
```

**Output:**

```
{
    "requestId": "1abcd234-e567-8fa9-0123-4567b890cd12",
    "imageVersionList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/recipe-name/1.0.0",
            "name": "recipe-name",
            ...
        }
    ]
}
```

------

## Cancel image creation \(AWS CLI\)<a name="cli-cancel-image-creation"></a>

To cancel an in\-progress image build, use the cancel\-image\-creation command, as follows:

```
aws imagebuilder cancel-image-creation --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-recipe/2019.12.03/1
```