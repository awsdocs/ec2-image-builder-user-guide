# EC2 Image Builder and interface VPC endpoints \(AWS PrivateLink\)<a name="vpc-interface-endpoints"></a>

You can establish a private connection between your VPC and EC2 Image Builder by creating an *interface VPC endpoint*\. Interface endpoints are powered by [AWS PrivateLink](http://aws.amazon.com/privatelink), a technology that enables you to privately access Image Builder APIs without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Instances in your VPC don't need public IP addresses to communicate with Image Builder APIs\. Traffic between your VPC and Image Builder does not leave the Amazon network\.

Each interface endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in your subnets\. When you create a new image, you can specify the VPC subnet\-id in your infrastructure configuration\.

**Note**  
Each service that you access from within a VPC has its own interface endpoint, with its own endpoint policy\. Image Builder downloads the AWSTOE component manager application and accesses managed resources from S3 buckets to create custom images\. To grant access to those buckets, you must update the S3 endpoint policy to allow it\. For more information, see [Custom policies for S3 bucket access](#vpc-endpoint-policy-s3)\.

For more information about VPC endpoints, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

## Considerations for Image Builder VPC endpoints<a name="vpc-endpoint-considerations"></a>

Before you set up an interface VPC endpoint for Image Builder, ensure that you review [Interface endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-limitations) in the *Amazon VPC User Guide*\.

Image Builder supports making calls to all of its API actions from your VPC\. 

## Create an interface VPC endpoint for Image Builder<a name="vpc-endpoint-create"></a>

To create a VPC endpoint for the Image Builder service, you can use either the Amazon VPC console or the AWS Command Line Interface \(AWS CLI\)\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

Create a VPC endpoint for Image Builder using the following service name: 
+ com\.amazonaws\.*region*\.imagebuilder

If you enable private DNS for the endpoint, you can make API requests to Image Builder using its default DNS name for the Region, for example: `imagebuilder.us-east-1.amazonaws.com`\. To look up the endpoint that applies to your target Region, see [EC2 Image Builder endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/imagebuilder.html#imagebuilder_region) in the *Amazon Web Services General Reference*\.

For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\.

## Create a VPC endpoint policy for Image Builder<a name="vpc-endpoint-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to Image Builder\. The policy specifies the following information:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

If you are using Amazon\-managed components in your recipe, the VPC endpoint for Image Builder must allow access to the following serviced\-owned component library:

`arn:aws:imagebuilder:region:aws:component/*`

**Important**  
When a non\-default policy is applied to an interface VPC endpoint for EC2 Image Builder, certain failed API requests, such as those failing from `RequestLimitExceeded`, might not be logged to AWS CloudTrail or Amazon CloudWatch\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

### Custom policies for S3 bucket access<a name="vpc-endpoint-policy-s3"></a>

Image Builder uses a publicly available S3 bucket to store and access managed resources, such as components\. It also downloads the AWSTOE component management application from a separate S3 bucket\. If you use a VPC endpoint for Amazon S3 in your environment, you’ll need to ensure that your S3 VPC endpoint policy allows Image Builder to access the following S3 buckets\. The bucket names are unique per AWS Region \(*region*\) and the application environment \(*environment*\)\. Image Builder and AWSTOE support the following application environments: `prod`, `preprod`, and `beta`\.
+ The AWSTOE component manager bucket:

  ```
  s3://ec2imagebuilder-toe-region-environment
  ```

  **Example:** s3://ec2imagebuilder\-toe\-us\-west\-2\-prod/\*
+ The Image Builder managed resources bucket:

  ```
  s3://ec2imagebuilder-managed-resources-region-environment/components
  ```

  **Example:** s3://ec2imagebuilder\-managed\-resources\-us\-west\-2\-prod/components/\*

### VPC endpoint policy examples<a name="vpc-endpoint-policy-examples"></a>

This section includes examples of custom VPC endpoint policies\.

**General VPC endpoint policy for Image Builder actions**  
The following example endpoint policy for Image Builder denies permission to delete Image Builder images and components\. The example policy also grants permission to perform all other EC2 Image Builder actions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
    {
        "Action": "imagebuilder:*",
        "Effect": "Allow",
        "Resource": "*"
    },
    {
        "Action": [
            "imagebuilder: DeleteImage"
        ],
        "Effect": "Deny",
        "Resource": "*",
    },
    {
        "Action": [
            "imagebuilder: DeleteComponent"
        ],
        "Effect": "Deny",
        "Resource": "*",
    }]
}
```

**Restrict access by organization, allow managed component access**  
The following example endpoint policy shows how to restrict access to identities and resources that belong to your organization and provide access to the Amazon\-managed AWSTOE components\. Replace *region*, *principal\-org\-id*, and *resource\-org\-id* with your organization's values\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRequestsByOrgsIdentitiesToOrgsResources",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgID": "principal-org-id",
          "aws:ResourceOrgID": "resource-org-id"
        }
      }
    },
    {
      "Sid": "AllowAccessToEC2ImageBuilderComponents",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "imagebuilder:GetComponent"
      ],
      "Resource": [
        "arn:aws:imagebuilder:region:aws:component/*"
      ]
    }
  ]
}
```

**VPC endpoint policy for Amazon S3 bucket access**  
The following S3 endpoint policy example shows how to provide access to the S3 buckets that Image Builder uses to build custom images\. Replace *region* and *environment* with your organization's values\. Add any other required permissions to the policy based on your application requirements\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowImageBuilderAccessToAppAndComponentBuckets",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::ec2imagebuilder-toe-region-environment/*",
        "arn:aws:s3:::ec2imagebuilder-managed-resources-region-environment/components/*"
      ]
    }
  ]
}
```