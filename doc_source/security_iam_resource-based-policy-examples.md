# EC2 Image Builder resource\-based policy examples<a name="security_iam_resource-based-policy-examples"></a>

To learn how to create a component, see [Create New Component](managing-image-builder-console.md#image-builder-create-component)\.

## Restricting Image Builder component access to specific IP addresses<a name="security_iam_resource-based-policy-examples-restrict-component-by-ip"></a>

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