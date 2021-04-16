# EC2 Image Builder identity\-based policies<a name="security-iam-identity-based-policies"></a>

**Topics**
+ [Identity\-based policy best practices](#security-iam-service-policy-best-practices)
+ [Using the Image Builder console](#sec-iam-id-based-policies-using-console)

## Identity\-based policy best practices<a name="security-iam-service-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete EC2 Image Builder resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started using AWS managed policies** – To start using EC2 Image Builder quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get started using permissions with AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant least privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for sensitive operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use policy conditions for extra security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Using the Image Builder console<a name="sec-iam-id-based-policies-using-console"></a>

To access the EC2 Image Builder console, you must have a minimum set of permissions\. These permissions allow you to list and view details about the Image Builder resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\.

To ensure that your IAM entities can use the Image Builder console, you must attach one of the following AWS managed policies to them:
+ [AWSImageBuilderReadOnlyAccess policy](security-iam-awsmanpol.md#sec-iam-manpol-AWSImageBuilderReadOnlyAccess)
+ [AWSImageBuilderFullAccess policy](security-iam-awsmanpol.md#sec-iam-manpol-AWSImageBuilderFullAccess)

For more information about Image Builder managed policies, see [Using managed policies for EC2 Image Builder](security-iam-awsmanpol.md)\.

**Important**  
The **AWSImageBuilderFullAccess** policy is required to create the Image Builder service\-linked role\. When you attach this policy to an IAM entity, you must also attach the following custom policy and include the resources you want to use that do not have `imagebuilder` in the resource name:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sns:Publish"
            ],
            "Resource": "sns topic arn"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetInstanceProfile"
            ],
            "Resource": "instance profile role arn"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "instance profile role arn",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "ec2.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "bucket arn"
        }
    ]
}
```

You don't need to allow minimum console permissions for users that are making calls to only the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.