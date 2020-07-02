# Identity and Access Management for EC2 Image Builder<a name="security-iam"></a>

**Topics**
+ [Audience](#security_iam_audience)
+ [Authenticating with identities](#security_iam_authentication)
+ [Managing access using policies](#security_iam_access-manage)
+ [How EC2 Image Builder works with IAM](security_iam_service-with-iam.md)
+ [EC2 Image Builder identity\-based policy examples](security_iam_id-based-policy-examples.md)
+ [EC2 Image Builder resource\-based policy examples](security_iam_resource-based-policy-examples.md)
+ [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)
+ [Troubleshooting EC2 Image Builder identity and access](security_iam_troubleshoot.md)

## Audience<a name="security_iam_audience"></a>

How you use AWS Identity and Access Management \(IAM\) differs, depending on the work you do in EC2 Image Builder\.

**Service user** – If you use the EC2 Image Builder service to do your job, then your administrator provides you with the credentials and permissions that you need\. As you use more EC2 Image Builder features to do your work, you might need additional permissions\. Understanding how access is managed can help you request the right permissions from your administrator\. If you cannot access a feature in EC2 Image Builder, see [Troubleshooting EC2 Image Builder identity and access](security_iam_troubleshoot.md)\.

**Service administrator** – If you're in charge of EC2 Image Builder resources at your company, you probably have full access to EC2 Image Builder\. It's your job to determine which EC2 Image Builder features and resources your employees should access\. You must then submit requests to your IAM administrator to change the permissions of your service users\. Review the information on this page to understand the basic concepts of IAM\. To learn more about how your company can use IAM with EC2 Image Builder, see [How EC2 Image Builder works with IAM](security_iam_service-with-iam.md)\.

**IAM administrator** – If you're an IAM administrator, you might want to learn details about how you can write policies to manage access to EC2 Image Builder\. To view example EC2 Image Builder identity\-based policies that you can use in IAM, see [EC2 Image Builder identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## Authenticating with identities<a name="security_iam_authentication"></a>

For detailed information about how to provide authentication for people and processes in your AWS account, see [Identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

## Managing access using policies<a name="security_iam_access-manage"></a>

For detailed information about how to manage access in AWS by creating policies and attaching them to IAM identities or AWS resources, see [Policies and Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\. 

The IAM role that you associate with your instance profile must have permissions to run the build and test components included in your image\. The following IAM role policies must be attached to the IAM role that is associated with the instance profile: **EC2InstanceProfileForImageBuilder** and **AmazonSSMManagedInstanceCore**\.