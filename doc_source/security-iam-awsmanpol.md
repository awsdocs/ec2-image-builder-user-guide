# Using managed policies for EC2 Image Builder<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the `ViewOnlyAccess` AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

## AWSImageBuilderFullAccess policy<a name="sec-iam-manpol-AWSImageBuilderFullAccess"></a>

The **AWSImageBuilderFullAccess** policy grants full access to Image Builder resources for the role it's attached to, allowing the role to list, describe, create, update, and delete Image Builder resources\. The policy also grants targeted permissions to related AWS services that are needed, for example, to verify resources, or to display current resources for the account in the AWS Management Console\.

### Permissions details<a name="sec-iam-manpol-AWSImageBuilderFullAccess-details"></a>

This policy includes the following permissions:
+ **Image Builder** – Administrative access is granted, so that the role can list, describe, create, update, and delete Image Builder resources\.
+ **Amazon EC2** – Access is granted for Amazon EC2 Describe actions that are needed to verify resource existence or get lists of resources belonging to the account\.
+ **IAM** – Access is granted to get and use instance profiles whose name contains "imagebuilder", to verify the existence of the Image Builder service\-linked role via the `iam:GetRole` API action, and to create the Image Builder service\-linked role\.
+ **License Manager** – Access is granted to list license configurations or licenses for a resource\.
+ **Amazon S3** – Access is granted to list buckets belonging to the account, and also Image Builder buckets with "imagebuilder" in their names\. 
+ **Amazon SNS** – Write permissions are granted to Amazon SNS to verify topic ownership for topics containing "imagebuilder"\.

### Policy example<a name="sec-iam-manpol-AWSImageBuilderFullAccess-policy"></a>

The following is an example of the AWSImageBuilderFullAccess policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "imagebuilder:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:ListTopics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:Publish"
            ],
            "Resource": "arn:aws:sns:*:*:*imagebuilder*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "license-manager:ListLicenseConfigurations",
                "license-manager:ListLicenseSpecificationsForResource"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetRole"
            ],
            "Resource": "arn:aws:iam::*:role/aws-service-role/imagebuilder.amazonaws.com/AWSServiceRoleForImageBuilder"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetInstanceProfile"
            ],
            "Resource": "arn:aws:iam::*:instance-profile/*imagebuilder*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:ListInstanceProfiles",
                "iam:ListRoles"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": [
                "arn:aws:iam::*:instance-profile/*imagebuilder*",
                "arn:aws:iam::*:role/*imagebuilder*"
            ],
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "ec2.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3::*:*imagebuilder*"
        },
        {
            "Action": "iam:CreateServiceLinkedRole",
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/aws-service-role/imagebuilder.amazonaws.com/AWSServiceRoleForImageBuilder",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": "imagebuilder.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeImages",
                "ec2:DescribeSnapshots",
                "ec2:DescribeVpcs",
                "ec2:DescribeRegions",
                "ec2:DescribeVolumes",
                "ec2:DescribeSubnets",
                "ec2:DescribeKeyPairs",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeInstanceTypeOfferings",
                "ec2:DescribeLaunchTemplates"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWSImageBuilderReadOnlyAccess policy<a name="sec-iam-manpol-AWSImageBuilderReadOnlyAccess"></a>

The **AWSImageBuilderReadOnlyAccess** policy provides read\-only access to all Image Builder resources\. Permissions are granted to verify that the Image Builder service\-linked role exists via the `iam:GetRole` API action\.

### Permissions details<a name="sec-iam-manpol-AWSImageBuilderReadOnlyAccess-details"></a>

This policy includes the following permissions:
+ **Image Builder** – Access is granted for read\-only access to Image Builder resources\.
+ **IAM** – Access is granted to verify the existence of the Image Builder service\-linked role via the `iam:GetRole` API action\.

### Policy example<a name="sec-iam-manpol-AWSImageBuilderReadOnlyAccess-policy"></a>

The following is an example of the AWSImageBuilderReadOnlyAccess policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "imagebuilder:Get*",
                "imagebuilder:List*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetRole"
            ],
            "Resource": "arn:aws:iam::*:role/aws-service-role/imagebuilder.amazonaws.com/AWSServiceRoleForImageBuilder"
        }
    ]
}
```

## AWSServiceRoleForImageBuilder policy<a name="sec-iam-manpol-AWSServiceRoleForImageBuilder"></a>

The **AWSServiceRoleForImageBuilder** policy allows Image Builder to call AWS services on your behalf\.

### Permissions details<a name="sec-iam-manpol-AWSServiceRoleForImageBuilder-details"></a>

This policy is attached to the Image Builder service\-linked role when the role is created through Systems Manager\. The policy includes the following permissions:
+ **CloudWatch Logs** – Access is granted to create and upload CloudWatch Logs to any log group whose name starts with `/aws/imagebuilder/`\.
+ **Amazon EC2** – Access is granted for Image Builder to create images and launch EC2 instances in your account, using related snapshots, volumes, network interfaces, security groups, and key pairs as required, as long as the image, instance, and volumes that are being created or used are tagged with `CreatedBy: EC2 Image Builder` or `CreatedBy: EC2 Fast Launch`\.

  Image Builder can get information about Amazon EC2 images, instance attributes, instance status, the instance types that are available to your account, launch templates, subnets, and tags on your Amazon EC2 resources\.

  Image Builder can update image settings to enable or disable faster launching of Windows instances in your account, where the image is tagged with `CreatedBy: EC2 Image Builder`\.

  Additionally, Image Builder can start, stop, and terminate instances that are running in your account, share Amazon EBS snapshots, create and update images and launch templates, de\-register existing images, add tags, and replicate images across accounts that you have granted permissions to via the **Ec2ImageBuilderCrossAccountDistributionAccess** policy\. Image Builder tagging is required for all of these actions, as described previously\.

  To review specific permissions that are granted, see the [policy example](#sec-iam-manpol-AWSServiceRoleForImageBuilder-policy) in this section\.
+ **IAM** – Access is granted for Image Builder to pass any role in your account to Amazon EC2, and to VM Import/Export\.
+ **AWS KMS** – Access is granted for Amazon EBS to encrypt, decrypt, or re\-encrypt Amazon EBS volumes\. This is crucial to ensure that encrypted volumes work when Image Builder builds an image\.
+ **License Manager** – Access is granted for Image Builder to update License Manager specifications via `license-manager:UpdateLicenseSpecificationsForResource`\.
+ **Amazon SNS** – Write permissions are granted for any Amazon SNS topic in the account\.
+ **Systems Manager** – Access is granted for Image Builder to list Systems Manager commands and their invocations, instance information, inventory entries and automation execution statuses\. Image Builder can also send automation signals, and stop automation exeuctions for any resource in your account\.

  Image Builder is able to issue run command invocations to any instance that is tagged `"CreatedBy": "EC2 Image Builder"` for the following script files: `AWS-RunPowerShellScript`, `AWS-RunShellScript`, or `AWSEC2-RunSysprep`\. Image Builder is able to start an Systems Manager automation execution in your account for automation documents where the name starts with `ImageBuilder`\.

  Image Builder is also able to create or delete State Manager associations for any instance in your account, as long as the association document is `AWS-GatherSoftwareInventory`, and to create the Systems Manager service\-linked role in your account\. For more information about the Image Builder service\-linked role, see [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)\.
+ **AWS STS** – Access is granted for Image Builder to assume roles named **EC2ImageBuilderDistributionCrossAccountRole** from your account to any account where the Trust policy on the role permits it\. This is used for cross\-account image distribution\.

### Policy example<a name="sec-iam-manpol-AWSServiceRoleForImageBuilder-policy"></a>

The following is an example of the AWSServiceRoleForImageBuilder policy\.

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:RunInstances"
			],
			"Resource": [
				"arn:aws:ec2:*::image/*",
				"arn:aws:ec2:*::snapshot/*",
				"arn:aws:ec2:*:*:subnet/*",
				"arn:aws:ec2:*:*:network-interface/*",
				"arn:aws:ec2:*:*:security-group/*",
				"arn:aws:ec2:*:*:key-pair/*",
				"arn:aws:ec2:*:*:launch-template/*",
				"arn:aws:license-manager:*:*:license-configuration:*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:RunInstances"
			],
			"Resource": [
				"arn:aws:ec2:*:*:volume/*",
				"arn:aws:ec2:*:*:instance/*"
			],
			"Condition": {
				"StringEquals": {
					"aws:RequestTag/CreatedBy": [
						"EC2 Image Builder", 
						"EC2 Fast Launch"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": "iam:PassRole",
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"iam:PassedToService": [
						"ec2.amazonaws.com",
						"ec2.amazonaws.com.cn",
						"vmie.amazonaws.com"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:StartInstances",
				"ec2:StopInstances",
				"ec2:TerminateInstances"
			],
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"ec2:ResourceTag/CreatedBy": "EC2 Image Builder"
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:CopyImage",
				"ec2:CreateImage",
				"ec2:CreateLaunchTemplate",
				"ec2:DeregisterImage",
				"ec2:DescribeExportImageTasks",
				"ec2:DescribeImages",
				"ec2:DescribeImportImageTasks",
				"ec2:DescribeInstanceAttribute",
				"ec2:DescribeInstanceStatus",
				"ec2:DescribeInstances",
				"ec2:DescribeInstanceTypeOfferings",
				"ec2:DescribeInstanceTypes",
				"ec2:DescribeSnapshots",
				"ec2:DescribeSubnets",
				"ec2:DescribeTags",
				"ec2:ModifyImageAttribute"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:ModifySnapshotAttribute"
			],
			"Resource": "arn:aws:ec2:*::snapshot/*",
			"Condition": {
				"StringEquals": {
					"ec2:ResourceTag/CreatedBy": "EC2 Image Builder"
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:CreateTags"
			],
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"ec2:CreateAction": [
						"RunInstances",
						"CreateImage"
					],
					"aws:RequestTag/CreatedBy": [
						"EC2 Image Builder",
						"EC2 Fast Launch"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:CreateTags"
			],
			"Resource": [
				"arn:aws:ec2:*::image/*",
				"arn:aws:ec2:*:*:export-image-task/*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:CreateTags"
			],
			"Resource": [
				"arn:aws:ec2:*::snapshot/*",
				"arn:aws:ec2:*:*:launch-template/*"
			],
			"Condition": {
				"StringEquals": {
					"aws:RequestTag/CreatedBy": [
						"EC2 Image Builder",
						"EC2 Fast Launch"
					]
				}
			}
		},
		{
		    "Effect": "Allow",
		    "Action": [
		        "ec2:EnableFastLaunch"
		    ],
		    "Resource": [
		    	"arn:aws:ec2:*::image/*",
		    	"arn:aws:ec2:*:*:launch-template/*"
		    ],
		    "Condition": {
		        "StringEquals": {
		            "ec2:ResourceTag/CreatedBy": "EC2 Image Builder"
		        }
		    }
		},
		{
			"Effect": "Allow",
			"Action": [
				"license-manager:UpdateLicenseSpecificationsForResource"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"sns:Publish"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"ssm:AddTagsToResource",
				"ssm:DescribeAssociationExecutions",
				"ssm:DescribeInstanceAssociationsStatus",
				"ssm:DescribeInstanceInformation",
				"ssm:GetAutomationExecution",
				"ssm:ListCommands",
				"ssm:ListCommandInvocations",
				"ssm:ListInventoryEntries",
				"ssm:SendAutomationSignal",
				"ssm:StopAutomationExecution"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": "ssm:SendCommand",
			"Resource": [
				"arn:aws:ssm:*:*:document/AWS-RunPowerShellScript",
				"arn:aws:ssm:*:*:document/AWS-RunShellScript",
				"arn:aws:ssm:*:*:document/AWSEC2-RunSysprep",
				"arn:aws:s3:::*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"ssm:SendCommand"
			],
			"Resource": [
				"arn:aws:ec2:*:*:instance/*"
			],
			"Condition": {
				"StringEquals": {
					"ssm:resourceTag/CreatedBy": [
						"EC2 Image Builder"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": "ssm:StartAutomationExecution",
			"Resource": "arn:aws:ssm:*:*:automation-definition/ImageBuilder*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"ssm:CreateAssociation",
				"ssm:DeleteAssociation"
			],
			"Resource": [
				"arn:aws:ssm:*:*:document/AWS-GatherSoftwareInventory",
				"arn:aws:ssm:*:*:association/*",
				"arn:aws:ec2:*:*:instance/*"
			]
		},
		{
			"Effect": "Allow",
			"Action": [
				"kms:Decrypt",
				"kms:Encrypt",
				"kms:GenerateDataKeyWithoutPlaintext",
				"kms:ReEncryptFrom",
				"kms:ReEncryptTo"
			],
			"Resource": "*",
			"Condition": {
				"ForAllValues:StringEquals": {
					"kms:EncryptionContextKeys": [
						"aws:ebs:id"
					]
				},
				"StringLike": {
					"kms:ViaService": [
						"ec2.*.amazonaws.com"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": [
				"kms:DescribeKey"
			],
			"Resource": "*",
			"Condition": {
				"StringLike": {
					"kms:ViaService": [
						"ec2.*.amazonaws.com"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": "kms:CreateGrant",
			"Resource": "*",
			"Condition": {
				"Bool": {
					"kms:GrantIsForAWSResource": true
				},
				"StringLike": {
					"kms:ViaService": [
						"ec2.*.amazonaws.com"
					]
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": "sts:AssumeRole",
			"Resource": "arn:aws:iam::*:role/EC2ImageBuilderDistributionCrossAccountRole"
		},
		{
			"Effect": "Allow",
			"Action": [
				"logs:CreateLogGroup",
				"logs:CreateLogStream",
				"logs:PutLogEvents"
			],
			"Resource": "arn:aws:logs:*:*:log-group:/aws/imagebuilder/*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:CreateLaunchTemplateVersion",
				"ec2:DescribeLaunchTemplates",
				"ec2:DescribeLaunchTemplateVersions",
				"ec2:ModifyLaunchTemplate"
			],
			"Resource": "*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:ExportImage"
			],
			"Resource": "arn:aws:ec2:*::image/*",
			"Condition": {
				"StringEquals": {
					"ec2:ResourceTag/CreatedBy": "EC2 Image Builder"
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:ExportImage"
			],
			"Resource": "arn:aws:ec2:*:*:export-image-task/*"
		},
		{
			"Effect": "Allow",
			"Action": [
				"ec2:CancelExportTask"
			],
			"Resource": "arn:aws:ec2:*:*:export-image-task/*",
			"Condition": {
				"StringEquals": {
					"ec2:ResourceTag/CreatedBy": "EC2 Image Builder"
				}
			}
		},
		{
			"Effect": "Allow",
			"Action": "iam:CreateServiceLinkedRole",
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"iam:AWSServiceName": [
						"ssm.amazonaws.com", 
						"ec2fastlaunch.amazonaws.com"
					]
				}
			}
		}
	]
}
```

## Ec2ImageBuilderCrossAccountDistributionAccess policy<a name="sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess"></a>

The **Ec2ImageBuilderCrossAccountDistributionAccess** policy grants permissions for Image Builder to distribute images across accounts in target Regions\. Additionally, Image Builder can describe, copy, and apply tags to any Amazon EC2 image in the account\. The policy also grants the ability to modify AMI permissions via the `ec2:ModifyImageAttribute` API action\.

### Permissions details<a name="sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess-details"></a>

This policy includes the following permissions:
+ **Amazon EC2** – Access is granted for Amazon EC2 to describe, copy, and modify attributes for an image, and to create tags for any Amazon EC2 images in the account\.

### Policy example<a name="sec-iam-manpol-Ec2ImageBuilderCrossAccountDistributionAccess-policy"></a>

The following is an example of the Ec2ImageBuilderCrossAccountDistributionAccess policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:CreateTags",
            "Resource": "arn:aws:ec2:*::image/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeImages",
                "ec2:CopyImage",
                "ec2:ModifyImageAttribute"
            ],
            "Resource": "*"
        }
    ]
}
```

## EC2InstanceProfileForImageBuilder policy<a name="sec-iam-manpol-EC2InstanceProfileForImageBuilder"></a>

The **EC2InstanceProfileForImageBuilder** policy grants the minimum permissions required for an EC2 instance to work with Image Builder\. This does not include permissions required to use the Systems Manager Agent\.

### Permissions details<a name="sec-iam-manpol-EC2InstanceProfileForImageBuilder-details"></a>

This policy includes the following permissions:
+ **CloudWatch Logs** – Access is granted to create and upload CloudWatch Logs to any log group whose name starts with `/aws/imagebuilder/`\.
+ **Image Builder** – Access is granted to get any Image Builder component\.
+ **AWS KMS** – Access is granted to decrypt an Image Builder component, if it was encrypted via AWS KMS\.
+ **Amazon S3** – Access is granted to get objects stored in an Amazon S3 bucket whose name starts with `ec2imagebuilder-`\.

### Policy example<a name="sec-iam-manpol-EC2InstanceProfileForImageBuilder-policy"></a>

The following is an example of the EC2InstanceProfileForImageBuilder policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "imagebuilder:GetComponent"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringEquals": {
                    "kms:EncryptionContextKeys": "aws:imagebuilder:arn",
                    "aws:CalledVia": [
                        "imagebuilder.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::ec2imagebuilder*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:CreateLogGroup",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/imagebuilder/*"
        }
    ]
}
```

## EC2InstanceProfileForImageBuilderECRContainerBuilds policy<a name="sec-iam-manpol-EC2InstanceProfileForImageBuilderECRContainerBuilds"></a>

The **EC2InstanceProfileForImageBuilderECRContainerBuilds** policy grants the minimum permissions required for an EC2 instance when working with Image Builder to build Docker images and then register and store the images in an Amazon ECR container repository\. This does not include permissions required to use the Systems Manager Agent\.

### Permissions details<a name="sec-iam-manpol-EC2InstanceProfileForImageBuilderECRContainerBuilds-details"></a>

This policy includes the following permissions:
+ **CloudWatch Logs** – Access is granted to create and upload CloudWatch Logs to any log group whose name starts with `/aws/imagebuilder/`\.
+ **Amazon ECR** – Access is granted for Amazon ECR to get, register, and store a container image, and to get an authorization token\.
+ **Image Builder** – Access is granted to get an Image Builder component or container recipe\.
+ **AWS KMS** – Access is granted to decrypt an Image Builder component or container recipe, if it was encrypted via AWS KMS\.
+ **Amazon S3** – Access is granted to get objects stored in an Amazon S3 bucket whose name starts with `ec2imagebuilder-`\.

### Policy example<a name="sec-iam-manpol-EC2InstanceProfileForImageBuilderECRContainerBuilds-policy"></a>

The following is an example of the EC2InstanceProfileForImageBuilderECRContainerBuilds policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "imagebuilder:GetComponent",
                "imagebuilder:GetContainerRecipe",
                "ecr:GetAuthorizationToken",
                "ecr:BatchGetImage",
                "ecr:InitiateLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:CompleteLayerUpload",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:PutImage"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringEquals": {
                    "kms:EncryptionContextKeys": "aws:imagebuilder:arn",
                    "aws:CalledVia": [
                        "imagebuilder.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::ec2imagebuilder*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:CreateLogGroup",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/imagebuilder/*"
        }
    ]
}
```

## Image Builder updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

This section provides information about updates to AWS managed policies for Image Builder since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Image Builder [document history](doc-history.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSServiceRoleForImageBuilder](#sec-iam-manpol-AWSServiceRoleForImageBuilder) – Update to an existing policy  |  Image Builder made the following changes to the service role: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/security-iam-awsmanpol.html)  | March 22, 2022 | 
|  [AWSServiceRoleForImageBuilder](#sec-iam-manpol-AWSServiceRoleForImageBuilder) – Update to an existing policy  |  Image Builder made the following changes to the service role: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/security-iam-awsmanpol.html)  | February 21, 2022 | 
|  [AWSServiceRoleForImageBuilder](#sec-iam-manpol-AWSServiceRoleForImageBuilder) – Update to an existing policy  |  Image Builder made the following changes to the service role: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/security-iam-awsmanpol.html)  | November 20, 2021 | 
|  [AWSServiceRoleForImageBuilder](#sec-iam-manpol-AWSServiceRoleForImageBuilder) – Update to an existing policy  |  Image Builder added new permissions to fix issues where more than one inventory association causes the image build to get stuck\.  | August 11, 2021 | 
|  [AWSImageBuilderFullAccess](#sec-iam-manpol-AWSImageBuilderFullAccess) – Update to an existing policy  |  Image Builder made the following changes to the full access role: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/security-iam-awsmanpol.html)  | April 13, 2021 | 
|  Image Builder started tracking changes  |  Image Builder started tracking changes for its AWS managed policies\.  | April 02, 2021 | 