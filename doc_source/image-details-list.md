# List images and build versions<a name="image-details-list"></a>

This section describes the various ways that you can list information about your EC2 Image Builder images\.

**Topics**
+ [List images](#list-images)
+ [List image build versions](#list-image-build-versions)

## List images<a name="list-images"></a>

You can get a summary of the Image Builder image resources that you own or have access to, along with some key details on the **Images** list page in the Image Builder console\. You can also use commands or actions in the Image Builder API, SDKs, or AWS CLI to list images\.

You can use one of the following methods to list Image Builder image resources that you have access to\. For the API action, see [ListImages](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImages.html) in the *EC2 Image Builder API Reference*\. For the associated SDK request, you can refer to the [See Also](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImages.html#API_ListImages_SeeAlso) link on the same page\.

------
#### [ Console ]

**Image details**  
Details that you'll see on the image list page in the Image Builder console include:
+ **Image name** – The name of the current version of the image resource\.
+ **Type** – Specifies whether this image resource produces an AMI or a container image for distribution\.
+ **Version** – The current version of the image resource\. In the Image Builder console, the version links to a list of build versions for the image\.
+ **Image source** – The origin of the base image that Image Builder used to build this image\.
+ **Platform** – The operating system platform of the image resource version, for example "Windows" or "Linux"\.
+ **Date created** – The date and time when Image Builder created the current version of the image resource\.
+ **Owner** – The owner of the current version of the image resource\.
+ **ARN** – The Amazon Resource Name \(ARN\) of the current version of the image resource\.

**List images**  
To list image resources in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Images** from the navigation pane\.

**Filter results**  
On the **Images** list page, you can search for specific images and filter the images that appear by choosing from the available filters located next to the search bar, as follows:

*Ownership*

Next to the search bar is a drop\-down list that shows ownership\. Image Builder displays images that your account owns by default\. You can change the selection to view other image resources that you have access to\.
+ **Owned by me** \(default selection\) – Image resources that your account owns\.
+ **Amazon\-managed** – Image resources that Amazon owns and manages\.
+ **Shared with me** – Image resources that other accounts have shared with you\.

*Operating system*

You can filter by the image operating system \(OS\), or view all types\. The default is to view all operating systems\.
+ **Any** \(default selection\)
+ **Linux**
+ **Windows**

*Image type*

You can filter by the type of image, or view all types\. The default is to view all image types\.
+ **Any type** \(default selection\)
+ **AMI**
+ **Docker**

*Image source*

If you want to see only images that are imported from a virtual machine, you can choose **VMIE**\. The default is to view all image types\. If you see a dash in the **Image source**, it means that the image was created before this detail was available\.

------
#### [ AWS CLI ]

When you run the [list\-images](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-images.html) command in the AWS CLI, you can get a list of images that you own or have access to\.

The following command example shows how to use the list\-images command without filters to list all of the Image Builder image resources that you own\.

**Example: list all images**

```
aws imagebuilder list-images
```

**Output:**

```
{
    "requestId": "1abcd234-e567-8fa9-0123-4567b890cd12",
    "imageVersionList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/image-recipe-name/1.0.0",
            "name": "image-recipe-name",
            "type": "AMI",
            "version": "1.0.0",
            "platform": "Linux",
            "owner": "123456789012",
            "dateCreated": "2022-04-28T01:38:23.286Z"
        },
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/image-recipe-win/1.0.1",
            "name": "image-recipe-win",
            "type": "AMI",
            "version": "1.0.1",
            "platform": "Windows",
            "owner": "123456789012",
            "dateCreated": "2022-04-28T01:38:23.286Z"
        }
    ]
}
```

You can apply filters to streamline the results when you run the list\-images command, as the following example shows\. For more information about filtering your results, see the [list\-images](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-images.html) command in the *AWS CLI Command Reference*\.

**Example: filter for Linux images**

```
aws imagebuilder list-images --filters name="platform",values="Linux"
```

**Output:**

```
{
    "requestId": "1abcd234-e567-8fa9-0123-4567b890cd12",
    "imageVersionList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/image-recipe-name/1.0.0",
            "name": "image-recipe-name",
            "type": "AMI",
            "version": "1.0.0",
            "platform": "Linux",
            "owner": "123456789012",
            "dateCreated": "2022-04-28T01:38:23.286Z"
        }
    ]
}
```

------

## List image build versions<a name="list-image-build-versions"></a>

You can see a list of build versions with some additional details for an image resource that you own on the **Image build versions** page in the Image Builder console\. You can also use commands or actions in the Image Builder API, SDKs, or AWS CLI to list image build versions\.

You can use one of the following methods to list image build versions for image resources that you own\. For the API action, see [ListImageBuildVersions](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImageBuildVersions.html) in the *EC2 Image Builder API Reference*\. For the associated SDK request, you can refer to the [See Also](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ListImageBuildVersions.html#API_ListImageBuildVersions_SeeAlso) link on the same page\.

------
#### [ Console ]

**Version details**  
Details that you'll see on the **Image build versions** page in the Image Builder console include:
+ **Version** – The image resource build version\. In the Image Builder console, the version links to an image detail page\.
+ **Type** – Specifies whether the associated image resource produces an AMI or a container image for distribution\.
+ **Date created** – The date and time when Image Builder created the image build version\.
+ **Image status** – The current status of the image build version\. Status can relate to the image build or disposition\. For example, during the build process, you might see a status of `Building` or `Distributing`\. For disposition of the image, you might see a status of `Deprecated` or `Deleted`\.
+ **Reason for failure** – The reason for the image status\. The Image Builder console only displays the reason when the build fails \(**Image status** equals `Failed`\)\.
+ **Security findings** – The console displays the aggregated image scan findings for the referenced image build version\.
+ **ARN** – The Amazon Resource Name \(ARN\) for the referenced version of the image resource\.
+ **Log stream** – Links to the log stream detail for the referenced image build version\.

**List versions**  
To list image build versions in the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Images** from the navigation pane\. By default, the image list shows the current version of each of the images that you own\.

1. To see a list of all the versions for an image, choose the current version link\. The link opens the **Image build versions** page that lists all of the build versions for a specific image\.

------
#### [ AWS CLI ]

When you run the [list\-image\-build\-versions](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/list-image-build-versions.html) command in the AWS CLI, you'll get a full list of build versions for the specified image resource\. You must own the image to run this command\.

The following command example shows how to use the list\-image\-build\-versions command to list all of the build versions for the specified image\.

**Example: list build versions for a specific image**

```
aws imagebuilder list-image-build-versions --image-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/image-recipe-name/1.0.0
```

**Output:**

The output for this example includes two build versions for the specified image recipe\.

```
{
    "requestId": "12f3e45d-67cb-8901-af23-45ed678c9b01",
    "imageSummaryList": [
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/image-recipe-name/1.0.0/2",
            "name": "image-recipe-name",
            "type": "AMI",
            "version": "1.0.0/2",
            "platform": "Linux",
            "osVersion": "Amazon Linux 2",
            "state": {
                "status": "AVAILABLE"
            },
            "owner": "123456789012",
            "dateCreated": "2023-03-10T01:04:40.609Z",
            "outputResources": {
                "amis": [
                    {
                        "region": "us-west-2",
                        "image": "ami-012b3456789012c3d",
                        "name": "image-recipe-name 2023-03-10T01-05-12.541Z",
                        "description": "First verison of image-recipe-name",
                        "accountId": "123456789012"
                    }
                ]
            },
            "tags": {}
        },
        {
            "arn": "arn:aws:imagebuilder:us-west-2:123456789012:image/image-recipe-name/1.0.0/1",
            "name": "image-recipe-name",
            "type": "AMI",
            "version": "1.0.0/1",
            "platform": "Linux",
            "osVersion": "Amazon Linux 2",
            "state": {
                "status": "AVAILABLE"
            },
            "owner": "123456789012",
            "dateCreated": "2023-03-10T00:07:16.384Z",
            "outputResources": {
                "amis": [
                    {
                        "region": "us-west-2",
                        "image": "ami-0d1e23456789f0a12",
                        "name": "image-recipe-name 2023-03-10T00-07-18.146132Z",
                        "description": "First verison of image-recipe-name",
                        "accountId": "123456789012"
                    }
                ]
            },
            "tags": {}
        }
    ]
}
```

**Note**  
Output from the list\-image\-build\-versions command does not include security findings or log streams at this time\.

------