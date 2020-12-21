# Tag EC2 Image Builder resources<a name="tag-resources"></a>

Tagging your resources can be useful for filtering and tracking resource costs, or other categories\. You can also control access based on tags\. For more information about tag\-based authorization, see [Authorization based on Image Builder tags](security_iam_service-with-iam.md#security_iam_service-with-iam-tags)

Image Builder supports the following dynamic tags:
+ `- {{imagebuilder:buildDate}}`

  Resolves to the build date/time at build time\.
+ `- {{imagebuilder:buildVersion}}`

  Resolves to a build version, which is a number that is located at the end of an Image Builder ARN\. For example, `"arn:aws:imagebuilder:us-west-2:123456789012:component/myexample-component/2019.12.02/1"` shows the build version as `1`\.

**Topics**
+ [Tag a resource \(AWS CLI\)](#cli-tag-resource)
+ [Untag a resource \(AWS CLI\)](#cli-untag-resource)
+ [List all of the tags for a specific resource \(AWS CLI\)](#cli-list-tags-for-resource)

## Tag a resource \(AWS CLI\)<a name="cli-tag-resource"></a>

The following example shows how to use an imagebuilder CLI command to to add and tag a resource in EC2 Image Builder\. You must provide the `resourceArn` and the tags to apply to it\.

The example `tag-resource.json` contents are as follows:

```
{
    "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline",
    "tags": {
        "KeyName": "KeyValue"
    }
}
```

Run the following command, which references the preceding `tag-resource.json` file\.

```
aws imagebuilder tag-resource --cli-input-json file://tag-resource.json
```

## Untag a resource \(AWS CLI\)<a name="cli-untag-resource"></a>

The following example shows how to use an imagebuilder CLI command to remove a tag from a resource\. You must provide the `resourceArn` and the keys to remove the tag\.

The example `untag-resource.json` contents are as follows:

```
{
    "resourceArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline",
    "tagKeys": [
        "KeyName"
    ]
}
```

Run the following command, which references the preceding `untag-resource.json` file\.

```
aws imagebuilder untag-resource --cli-input-json file://untag-resource.json
```

## List all of the tags for a specific resource \(AWS CLI\)<a name="cli-list-tags-for-resource"></a>

The following example shows how to use an imagebuilder CLI command to list all the tags for a specific resource\.

```
aws imagebuilder list-tags-for-resource --resource-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```