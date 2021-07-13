# List and view component details<a name="component-details"></a>

This section describes the various ways you can find information and view details for the AWS TOE components that you use in your EC2 Image Builder recipes\.

**Topics**
+ [List components \(AWS CLI\)](#cli-list-components)
+ [List component build versions \(AWS CLI\)](#cli-list-component-versions)
+ [Get component details \(AWS CLI\)](#cli-get-component)
+ [Get component policy details \(AWS CLI\)](#cli-get-component-policy)

## List components \(AWS CLI\)<a name="cli-list-components"></a>

The following example shows how to use an imagebuilder CLI command to list all of the component semantic versions that you have access to\.

```
aws imagebuilder list-components
```

You can optionally filter on whether you want to view components owned by you, by Amazon, or those shared with you by other accounts\. By default, this request shows only components owned by your account\.

```
aws imagebuilder list-components --owner Self
```

```
aws imagebuilder list-components --owner Amazon
```

```
aws imagebuilder list-components --owner Shared
```

## List component build versions \(AWS CLI\)<a name="cli-list-component-versions"></a>

The following example shows how to use an imagebuilder CLI command to list component build versions that have a specific semantic version\.

```
aws imagebuilder list-component-build-versions --component-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.03
```

## Get component details \(AWS CLI\)<a name="cli-get-component"></a>

The following example shows how to use an imagebuilder CLI command to get the details of a component by specifying its ARN\.

```
aws imagebuilder get-component --component-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02/1
```

## Get component policy details \(AWS CLI\)<a name="cli-get-component-policy"></a>

The following example shows how to use an imagebuilder CLI command to get the details of a component policy by specifying its Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-component-policy --component-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02
```