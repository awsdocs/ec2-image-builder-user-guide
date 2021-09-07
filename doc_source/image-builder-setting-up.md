# Prerequisites<a name="image-builder-setting-up"></a>

Verify the following prerequisites to create an image pipeline with EC2 Image Builder\. Unless specifically stated otherwise, prerequisites are required for all types of pipelines\.

## EC2 Image Builder service\-linked role<a name="image-builder-auto-scaling-prereq"></a>

EC2 Image Builder uses a service\-linked role to grant permissions to other AWS services on your behalf\. You don't need to manually create a service\-linked role\. When you create your first Image Builder resource in the AWS Management Console, the AWS CLI, or the AWS API, Image Builder creates the service\-linked role for you\. For more information about the service\-linked role that Image Builder creates in your account, see [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)\. 

## Configuration requirements<a name="image-builder-config"></a>
+ Image Builder supports [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html)\.
+ Image Builder supports EC2\-Classic\.
+ The instances that Image Builder uses to build images and run tests must have access to the Systems Manager service\. All build activity is orchestrated by Systems Manager Automation\. Installation requirements depend on your operating system\.

  To see the installation requirements for your source image, choose the tab that matches your source image operating system\.

------
#### [ Linux ]

  For Amazon EC2 Linux instances, Image Builder installs the Systems Manager Agent on the build instance if it is not already present, and removes it before creating the image\.

------
#### [ Windows ]

  Image Builder does not install the Systems Manager Agent on Amazon EC2 Windows Server build instances\. If your source image did not come pre\-installed with the Systems Manager Agent, you must launch an instance from your source image, manually install Systems Manager on the instance, and create a new source image from your instance\.

  To manually install the Systems Manager agent on your Amazon EC2 Windows Server instance, see [Manually install Systems Manager Agent on EC2 instances for Windows Server](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-win.html) in the *AWS Systems Manager User Guide*\.

------

## Container repository \(*container image pipelines*\)<a name="start-prereq-container"></a>

For container image pipelines, the recipe defines the configuration for the Docker images that are produced and stored in the target container repository\. You must create the target repository before you create the container recipe for your Docker image\.

The default target repository for Image Builder is Amazon ECR\. To create an Amazon ECR repository, follow the steps described in [Creating a repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html) in the *Amazon Elastic Container Registry User Guide*\.

## AWS Identity and Access Management \(IAM\)<a name="image-builder-IAM-prereq"></a>

The IAM role that you associate with your instance profile must have permissions to run the build and test components included in your image\. The following IAM role policies must be attached to the IAM role that is associated with the instance profile:
+ EC2InstanceProfileForImageBuilder
+ EC2InstanceProfileForImageBuilderECRContainerBuilds
+ AmazonSSMManagedInstanceCore

If you configure logging, the instance profile specified in your infrastructure configuration must have `s3:PutObject` permissions for the target bucket \(`arn:aws:s3:::BucketName/*`\)\. For example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::bucket-name/*"
        }
    ]
}
```

**Attach policy**  
The following steps guide you through the process of attaching the IAM policies to an IAM role to grant the preceding permissions\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Policies**\.

1. Filter the list of policies with **EC2InstanceProfileForImageBuilder**

1. Select the bullet next to the policy, and from the **Policy actions** dropdown list, select **Attach**\.

1. Select the name of the IAM role to which to attach the policy\.

1. Choose **Attach policy**\.

1. Repeat steps 3\-6 for the **EC2InstanceProfileForImageBuilderECRContainerBuilds** and **AmazonSSMManagedInstanceCore** policies\.

**Note**  
If you want to copy an image created with Image Builder to another account, you must create the `EC2ImageBuilderDistributionCrossAccountRole` role in all of the target accounts, and attach the [Ec2ImageBuilderCrossAccountDistributionAccess policy](security-iam-awsmanpol.md#sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess) managed policy to the role\. For more information, see [Share EC2 Image Builder resources](manage-shared-resources.md)\.