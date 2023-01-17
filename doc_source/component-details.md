# List and view component details<a name="component-details"></a>

This section describes how you can find information and view details for the AWS Task Orchestrator and Executor \(AWSTOE\) components that you use in your EC2 Image Builder recipes\.

**Topics**
+ [List AWSTOE components](#list-components)
+ [List component build versions \(AWS CLI\)](#cli-list-component-versions)
+ [Get component details \(AWS CLI\)](#cli-get-component)
+ [Get component policy details \(AWS CLI\)](#cli-get-component-policy)

## List AWSTOE components<a name="list-components"></a>

You can use one of the following methods to list and filter AWSTOE components\.

------
#### [ AWS Management Console ]

To display a list of components in the AWS Management Console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Select **Components** from the navigation pane\. By default, Image Builder shows a list of components that your account owns\.

1. You can optionally filter on component ownership\. To see components that you don't own, but have access to, expand the owner type dropdown list and select one of the values\. The owner type list is located in the search bar, next to the search text box\. You can select from the following values:
   + **Quick start \(Amazon managed\)** – Publicly available components that Amazon creates and maintains\.
   + **Owned by me** – Components that you created\. This is the default selection\.
   + **Shared with me** – Components that others created and shared with you from their account\.
   + **Third party managed** – Components that a third party owns that you subscribed to in AWS Marketplace\.

------
#### [ AWS CLI ]

The following example shows how to use the [list\-components](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-components.html) command to return a list of AWSTOE components that your account owns\.

```
aws imagebuilder list-components
```

You can optionally filter on component ownership\. The owner attribute defines who owns the components that you want to list\. By default, this request returns a list of components that your account owns\. To filter the results by component owner, specify one of the following values with the `--owner` parameter when you run the list\-components command\.

**Component owner values**
+ Self
+ Amazon
+ ThirdParty
+ Shared

The following examples show the list\-components command with the `--owner` parameter to filter results\.

```
aws imagebuilder list-components --owner Self
{
    "requestId": "012a3456-b789-01cd-e234-fa5678b9012b",
    "componentVersionList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:component/sample-component01/1.0.0",
            "name": "sample-component01",
            "version": "1.0.0",
            "platform": "Linux",
            "type": "BUILD",
            "owner": "123456789012",
            "dateCreated": "2020-09-24T16:58:24.444Z"
        },
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:component/sample-component01/1.0.1",
            "name": "sample-component01",
            "version": "1.0.1",
            "platform": "Linux",
            "type": "BUILD",
            "owner": "123456789012",
            "dateCreated": "2021-07-10T03:38:46.091Z"
        }
    ]
}
```

```
aws imagebuilder list-components --owner Amazon
```

```
aws imagebuilder list-components --owner Shared
```

```
aws imagebuilder list-components --owner ThirdParty
```

------

## List component build versions \(AWS CLI\)<a name="cli-list-component-versions"></a>

The following example shows how to use the [list\-component\-build\-versions](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-component-build-versions.html) command to list component build versions that have a specific semantic version\. To learn more about semantic versioning for Image Builder resources, see [Semantic versioning](ibhow-semantic-versioning.md)\.

```
aws imagebuilder list-component-build-versions --component-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/example-component/1.0.1
{
    "requestId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
    "componentSummaryList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:component/examplecomponent/1.0.1/1",
            "name": "examplecomponent",
            "version": "1.0.1",
            "platform": "Linux",
            "type": "BUILD",
            "owner": "123456789012",
            "description": "An example component that builds, validates and tests an image",
            "changeDescription": "Updated version.",
            "dateCreated": "2020-02-19T18:53:45.940Z",
            "tags": {
                "KeyName": "KeyValue"
            }
        }
    ]
}
```

## Get component details \(AWS CLI\)<a name="cli-get-component"></a>

The following example shows how to use the [get\-component](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-component.html) command to get component details when you specify the component's Amazon Resource Name \(ARN\)\.

```
aws imagebuilder get-component --component-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/example-component/1.0.1/1
			{
    "requestId": "a1b2c3d4-5678-90ab-cdef-EXAMPLE11112",
    "component": {
        "arn": "arn:aws:imagebuilder:us-west-2:123456789012:component/examplecomponent/1.0.1/1",
        "name": "examplecomponent",
        "version": "1.0.1",
        "type": "BUILD",
        "platform": "Linux",
        "owner": "123456789012",
        "data": "name: HelloWorldTestingDocument\ndescription: This is hello world testing document... etc.\"\n",
        "encrypted": true,
        "dateCreated": "2020-09-24T16:58:24.444Z",
        "tags": {}
    }
}
```

## Get component policy details \(AWS CLI\)<a name="cli-get-component-policy"></a>

The following example shows how to use the [get\-component\-policy](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/get-component-policy.html) command to get details of a component policy when you specify the component's ARN\.

```
aws imagebuilder get-component-policy --component-arn arn:aws:imagebuilder:us-west-2:123456789012:component/example-component/1.0.1
```