# How EC2 Image Builder works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to Image Builder, you should understand what IAM features are available to use with Image Builder\. To get a high\-level view of how Image Builder and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [Image Builder identity\-based policies](#security_iam_id-based-policy-examples)
+ [Image Builder resource\-based policies](#security-iam-service-with-ib-resource-based-policies)
+ [Authorization based on Image Builder tags](#security-iam-service-with-ib-tags)
+ [Image Builder IAM roles](#security-iam-service-with-ib-roles)

## Image Builder identity\-based policies<a name="security_iam_id-based-policy-examples"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources, and also the conditions under which actions are allowed or denied\. Image Builder supports specific actions, resources, and condition keys\. For information about all of the elements that you use in a JSON policy, see [Actions, Resources, and Condition Keys for Amazon EC2 Image Builder](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonec2imagebuilder.html) in the *IAM User Guide*\.

### Actions<a name="sec-iam-ib-id-based-policies-actions"></a>

Policy actions in Image Builder use the following prefix before the action: `imagebuilder:`\. Policy statements must include either an `Action` or `NotAction` element\. Image Builder defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "imagebuilder:action1",
      "imagebuilder:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `List`, include the following action:

```
"Action": "imagebuilder:List*"
```

To see a list of Image Builder actions, see [Actions, Resources, and Condition Keys for AWS Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actions-resources-contextkeys.html) in the *IAM User Guide*\.

### Managing access using policies<a name="security-iam-manage-access"></a>

For detailed information about how to manage access in AWS by creating policies and attaching them to IAM identities or AWS resources, see [Policies and Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\. 

The IAM role that you associate with your instance profile must have permissions to run the build and test components included in your image\. The following IAM role policies must be attached to the IAM role that is associated with the instance profile:
+ EC2InstanceProfileForImageBuilder
+ EC2InstanceProfileForImageBuilderECRContainerBuilds
+ AmazonSSMManagedInstanceCore

### Resources<a name="sec-iam-ib-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```

The Image Builder instance resource has the following Amazon Resource Name \(ARN\)\.

```
arn:aws:imagebuilder:region:account-id:resource:resource-id
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `i-1234567890abcdef0` instance in your statement, use the following ARN\.

```
"Resource": "arn:aws:imagebuilder:us-east-1:123456789012:instance/i-1234567890abcdef0"
```

To specify all instances that belong to a specific account, use the wildcard \(\*\)\.

```
"Resource": "arn:aws:imagebuilder:us-east-1:123456789012:instance/*"
```

Some Image Builder actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

Many EC2 Image Builder API actions involve multiple resources\. To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "resource1",
      "resource2"
```

### Condition keys<a name="sec-iam-ib-id-based-policies-conditionkeys"></a>

Image Builder provides service\-specific condition keys and supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\. The following service\-specific condition keys are provided\.

#### imagebuilder:CreatedResourceTagKeys<a name="image-builder-security-createdresourcetagkeys"></a>

Works with [string operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_String)\.

Use this key to filter access by the presence of tag keys in the request This allows you to manage the resources created by Image Builder through defined tags\.

**Availability \-** This key is available to only the `CreateInfrastrucutreConfiguration` and `UpdateInfrastructureConfiguration` APIs\.

#### imagebuilder:CreatedResourceTag/<key><a name="image-builder-security-createdresourcetag"></a>

Works with [string operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_String)\.

Use this key to filter access by the tag key\-value pairs attached to the resource created by Image Builder\. This allows you to manage the resources created by Image Builder through defined tags\.

**Availability \-** This key is available to only the `CreateInfrastrucutreConfiguration` and `UpdateInfrastructureConfiguration` APIs\.

### Examples<a name="sec-iam-ib-id-based-policies-examples"></a>



To view examples of Image Builder identity\-based policies, see [EC2 Image Builder identity\-based policies](security-iam-identity-based-policies.md)\.

## Image Builder resource\-based policies<a name="security-iam-service-with-ib-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that specify what actions a specified principal can perform on the Image Builder resource and under what conditions\. Image Builder supports resource\-based permissions policies for components, images, and image recipes\. Resource\-based policies let you grant usage permission to other accounts on a per\-resource basis\. You can also use a resource\-based policy to allow an AWS service to access your components, images, and image recipes\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the [principal in a resource\-based policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, you must also grant the principal entity permission to access the resource\. Grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM Roles Differ from Resource\-based Policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)in the *IAM User Guide*\.

For information about how to attach a resource\-based policy to a component, image, or image recipe, see [Share EC2 Image Builder resources](manage-shared-resources.md)\.

**Note**  
When you update a resource policy using Image Builder, the update will appear in the RAM console\.

## Authorization based on Image Builder tags<a name="security-iam-service-with-ib-tags"></a>

You can attach tags to Image Builder resources or pass tags in a request to Image Builder\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `imagebuilder:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging Image Builder resources, see [Tag a resource \(AWS CLI\)](tag-resources.md#cli-tag-resource)\.

## Image Builder IAM roles<a name="security-iam-service-with-ib-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with Image Builder<a name="security-iam-service-with-ib-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

### Service\-linked roles<a name="sec-iam-ib-service-linked-roles"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

Image Builder supports service\-linked roles\. For information about creating or managing Image Builder service\-linked roles, see [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)\.

### Service roles<a name="sec-iam-ib-service-roles"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

### <a name="security-iam-service-with-ib-roles-choose"></a>