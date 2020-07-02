# Using service\-linked roles for EC2 Image Builder<a name="image-builder-service-linked-role"></a>

EC2 Image Builder uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Image Builder\. Service\-linked roles are predefined by Image Builder and include all of the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Image Builder easier because you don’t have to manually add the necessary permissions\. Image Builder defines the permissions of its service\-linked roles, and unless defined otherwise, only Image Builder can assume its roles\. The defined permissions include the trust policy and the permissions policy\. The permissions policy cannot be attached to any other IAM entity\. 

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes** in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Image Builder<a name="image-builder-slr-permissions"></a>

Image Builder uses the service\-linked role named **AWSServiceRoleForImageBuilder** to allow EC2 Image Builder to access AWS resources on your behalf\.

The **AWSServiceRoleForImageBuilder** service\-linked role trusts the following services to assume the role:
+ imagebuilder\.amazonaws\.com
+ ssm\.amazonaws\.com

The role permissions policy allows the Image Builder service to complete the following actions on the specified resources:

**EC2**
+ ec2:CancelExportTask
+ ec2:CopyImage
+ ec2:CreateImage
+ ec2:CreateLaunchTemplate
+ ec2:CreateTags
+ ec2:DeleteLaunchTemplate
+ ec2:DeregisterImage
+ ec2:DescribeImages
+ ec2:DescribeInstanceStatus
+ ec2:DescribeSubnets
+ ec2:DescribeTags
+ ec2:ModifyImageAttribute
+ ec2:RunInstances
+ ec2:StopInstances
+ ec2:TerminateInstances

**IAM**
+ iam:CreateServiceLinkedRole

**License Manager**
+ license\-manager:UpdateLicenseSpecificationsForResource

**SSM**
+ ssm:AddTagsToResource
+ ssm:DescribeInstanceInformation
+ ssm:GetAutomationExecution
+ ssm:SendCommand
+ ssm:StartAutomationExecution
+ ssm:StopAutomationExecution

**SNS**
+ sns:Publish

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions in the IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions)\.

## Creating a service\-linked role for EC2 Image Builder<a name="image-builder-slr-creating"></a>

You don't need to manually create a service\-linked role\. When you create your first Image Builder resource in the AWS Management Console, the AWS CLI, or the AWS API, Image Builder creates the service\-linked role for you\.

**Important**  
If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create your first EC2 Image Builder resource, Image Builder creates the service\-linked role for you again\.

You can also use the IAM console to create a service\-linked role with the EC2 Image Builder use case\. In the AWS CLI or the AWS API, create a service\-linked role with the `imagebuilder.amazonaws.com` service name\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

## Editing a service\-linked role for Image Builder<a name="image-builder-slr-editing"></a>

Use the Image Builder console, the AWS CLI, or the AWS API to change the description, trust policy, or permission policy, including adding additional policies, of the **AWSServiceRoleForImageBuilder** service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit only the description of the role using IAM, the AWS CLI, or the API\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Image Builder<a name="image-builder-slr-deleting"></a>

You can use the IAM console, the AWS CLI, or the AWS API to manually delete the service\-linked role\. To do this, you must first manually clean up the resources for your service\-linked role and then you can manually delete it\.

**Note**  
If the Image Builder service is using the role when you try to delete the resources, the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete Image Builder resources used by the AWSServiceRoleForImageBuilder**

1. Either wait for current image builds to finish, or explicitly cancel them using the cancel\-image\-creation API\. To cancel image builds on the Image Builder console, select the **Stop Pipeline** action button for each pipeline\.

1. Delete all pipelines or change the build schedule for all image pipelines to **Manual** using the Image Builder console or CLI\.

## To manually delete the service\-linked role using IAM<a name="image-builder-slr-delete-manual"></a>

Use the IAM console, the AWS CLI, or the AWS API to delete the **AWSServiceRoleForImageBuilder** service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for EC2 Image Builder service\-linked roles<a name="image-builder-slr-regions"></a>

Image Builder supports using service\-linked roles in all of the AWS Regions where the service is available\. For the list of supported AWS Regions, see [AWS Regions and Endpoints](how-image-builder-works.md#image-builder-regions)\.