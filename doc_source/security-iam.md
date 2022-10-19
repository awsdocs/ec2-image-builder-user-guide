# Identity and Access Management for EC2 Image Builder<a name="security-iam"></a>

**Topics**
+ [Audience](#security-iam-audience)
+ [Authenticating with identities](#security-iam-authentication)
+ [How EC2 Image Builder works with IAM](security_iam_service-with-iam.md)
+ [EC2 Image Builder identity\-based policies](security-iam-identity-based-policies.md)
+ [EC2 Image Builder resource\-based policies](#security-iam-resource-based-policies)
+ [Using managed policies for EC2 Image Builder](security-iam-awsmanpol.md)
+ [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)
+ [Troubleshooting EC2 Image Builder identity and access](security_iam_troubleshoot.md)

## Audience<a name="security-iam-audience"></a>

How you use AWS Identity and Access Management \(IAM\) differs, depending on the work that you do in EC2 Image Builder\.

**Service user** – If you use the EC2 Image Builder service to do your job, then your administrator provides you with the credentials and permissions that you need\. As you use more EC2 Image Builder features to do your work, you might need additional permissions\. Understanding how access is managed can help you request the right permissions from your administrator\. If you cannot access a feature in EC2 Image Builder, see [Troubleshooting EC2 Image Builder identity and access](security_iam_troubleshoot.md)\.

**Service administrator** – If you're in charge of EC2 Image Builder resources at your company, you probably have full access to EC2 Image Builder\. It's your job to determine which EC2 Image Builder features and resources your service users should access\. You must then submit requests to your IAM administrator to change the permissions of your service users\. Review the information on this page to understand the basic concepts of IAM\. To learn more about how your company can use IAM with EC2 Image Builder, see [How EC2 Image Builder works with IAM](security_iam_service-with-iam.md)\.

**IAM administrator** – If you're an IAM administrator, you might want to learn details about how you can write policies to manage access to EC2 Image Builder\. To view example EC2 Image Builder identity\-based policies that you can use in IAM, see [Image Builder identity\-based policies](security_iam_service-with-iam.md#security_iam_id-based-policy-examples)\.

## Authenticating with identities<a name="security-iam-authentication"></a>

For detailed information about how to provide authentication for people and processes in your AWS account, see [Identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

## EC2 Image Builder resource\-based policies<a name="security-iam-resource-based-policies"></a>

For information about how to create a component, see [Manage components with AWSTOE](manage-components.md)\.

### Restricting Image Builder component access to specific IP addresses<a name="sec-iam-resourcepol-restrict-component-by-ip"></a>

The following example grants permissions to any user to perform any Image Builder operations on components\. However, the request must originate from the range of IP addresses specified in the condition\.

The condition in this statement identifies the 54\.240\.143\.\* range of allowed Internet Protocol version 4 \(IPv4\) IP addresses, with one exception: 54\.240\.143\.188\.

The `Condition` block uses the `IpAddress` and `NotIpAddress` conditions and the `aws:SourceIp` condition key, which is an AWS\-wide condition key\. For more information about these condition keys, see [Specifying Conditions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/amazon-s3-policy-keys.html)\. The`aws:sourceIp` IPv4 values use the standard CIDR notation\. For more information, see [IP Address Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_IPAddress) in the *IAM User Guide*\.

```
{
  "Version": "2012-10-17",
  "Id": "IBPolicyId1",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "imagebuilder.GetComponent:*",
      "Resource": "arn:aws:imagebuilder:::examplecomponent/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "54.240.143.0/24"},
         "NotIpAddress": {"aws:SourceIp": "54.240.143.188/32"} 
      } 
    } 
  ]
}
```