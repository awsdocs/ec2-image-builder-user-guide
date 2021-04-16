# Using service\-linked roles for EC2 Image Builder<a name="image-builder-service-linked-role"></a>

EC2 Image Builder uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Image Builder\. Service\-linked roles are predefined by Image Builder and include all of the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Image Builder more efficient, because you don’t have to add the necessary permissions manually\. Image Builder defines the permissions of its service\-linked roles, and unless defined otherwise, only Image Builder can assume its roles\. The defined permissions include the trust policy and the permissions policy\. The permissions policy cannot be attached to any other IAM entity\. 

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes** in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Image Builder<a name="image-builder-slr-permissions"></a>

Image Builder uses the **AWSServiceRoleForImageBuilder** service\-linked role to allow EC2 Image Builder to access AWS resources on your behalf\. The service\-linked role trusts the *imagebuilder\.amazonaws\.com* service to assume the role\.

You don't need to manually create this service\-linked role\. When you create your first Image Builder resource in the AWS Management Console, the AWS CLI, or the AWS API, Image Builder creates the service\-linked role for you, through SSM\.

**Important**  
If the service\-linked role is deleted from your account, you can use the same process to create it again\. When you create your first EC2 Image Builder resource, Image Builder creates the service\-linked role for you again\.

To see permissions for the **AWSServiceRoleForImageBuilder**, see the [AWSServiceRoleForImageBuilder policy](security-iam-awsmanpol.md#sec-iam-manpol-AWSServiceRoleForImageBuilder) page\. To learn more about configuring permissions for a service\-linked role, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Removing an Image Builder service\-linked role from your account<a name="image-builder-slr-deleting"></a>

You can use the IAM console, the AWS CLI, or the AWS API to manually remove the service\-linked role for Image Builder from your account\. However, before you do this, you must ensure that there are no Image Builder resources enabled that refer to it\.

**Note**  
If the Image Builder service is using the role when you try to delete the resources, the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**Clean up Image Builder resources used by the `AWSServiceRoleForImageBuilder` role**

1. Verify that no pipeline builds are running before you start\. To cancel a running build, use the `cancel-image-creation` command from the AWS CLI\.

   ```
   aws imagebuilder cancel-image-creation --image-build-version-arn arn:aws:imagebuilder:us-east-1:123456789012:image-pipeline/sample-pipeline
   ```

1. Change all pipeline schedules to use a manual build process, or delete them if you won't be using them again\. For more information about deleting resources, see [Delete EC2 Image Builder resources](delete-resources.md)\.

**Delete the service\-linked role using IAM**  
You can use the IAM console, the AWS CLI, or the AWS API to delete the `AWSServiceRoleForImageBuilder` role from your account\. For more information, see [ Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role)in the *IAM User Guide*\.

## Supported Regions for EC2 Image Builder service\-linked roles<a name="image-builder-slr-regions"></a>

Image Builder supports using service\-linked roles in all of the AWS Regions where the service is available\. For the list of supported AWS Regions, see [AWS Regions and Endpoints](how-image-builder-works.md#image-builder-regions)\.