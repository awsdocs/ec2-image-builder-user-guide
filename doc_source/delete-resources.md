# Delete EC2 Image Builder resources<a name="delete-resources"></a>

Your Image Builder environment, just like your home, needs regular maintenance to help you find what you need, and complete your tasks without wading through clutter\. Make sure to regularly clean up temporary resources that you created for testing\. Otherwise, you might forget about those resources, and then later, not remember what they were used for\. By then, it might not be clear if you can safely get rid of them\.

**Tip**  
To prevent dependency errors when you delete resources, make sure to delete your resources in the following order:  
Image pipeline
Image recipe
All remaining resources

## Delete resources using the AWS Management Console<a name="delete-resources-console"></a>

To delete an image pipeline and its resources, follow these steps:

**Delete the pipeline**

1. To see a list of the build pipelines created under your account, choose **Image pipelines** from the navigation pane\.

1. Select the check box next to **Pipeline name** to select the pipeline that you want to delete\.

1. At the top of the **Image pipelines** panel, on the **Actions** menu, choose **Delete**\.

1. To confirm the deletion, enter `Delete` in the box, and choose **Delete**\.

**Delete the recipe**

1. To see a list of the recipes created under your account, choose **Image recipes** from the navigation pane\.

1. Select the check box next to **Recipe name** to select the recipe that you want to delete\.

1. At the top of the **Image recipes** panel, on the **Actions** menu, choose **Delete recipe**\.

1. To confirm the deletion, enter `Delete` in the box, and choose **Delete**\.

**Delete infrastructure configuration**

1. To see a list of the infrastructure configurations created under your account, choose **Infrastructure configuration** from the navigation pane\.

1. Select the check box next to **Configuration name** to select the infrastructure configuration that you want to delete\.

1. At the top of the **Infrastructure configurations** panel, choose **Delete**\.

1. To confirm the deletion, enter `Delete` in the box, and choose **Delete**\.

**Delete distribution settings**

1. To see a list of the distribution settings created under your account, choose **Distribution settings** from the navigation pane\.

1. Select the check box next to **Configuration name** to select the distribution settings that you created for this tutorial\.

1. At the top of the **Distribution settings** panel, choose **Delete**\.

1. To confirm the deletion, enter `Delete` in the box, and choose **Delete**\.

**Delete an image**

1. To see a list of the images created under your account, choose **Images** from the navigation pane\.

1. Choose the image **Version** for the image that you want to remove\. This opens the **Image build versions** page\.

1. Select the check box next to the **Version** for any image that you want to delete\. You can select more than one image version at a time\.

1. At the top of the **Image build versions** panel, choose **Delete version**\.

1. To confirm the deletion, enter `Delete` in the box, and choose **Delete**\.

## Delete an image pipeline using the AWS CLI<a name="delete-resources-cli"></a>

The following examples show how to delete Image Builder resources using the AWS CLI\. As mentioned previously, resources must be deleted in the following order to avoid dependency errors:

1. Image pipeline

1. Image recipe

1. All remaining resources

**Delete image pipeline \(AWS CLI\)**  
The following example shows how to delete an image pipeline by specifying its ARN\.

```
aws imagebuilder delete-image-pipeline --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```

**Delete image recipe \(AWS CLI\)**  
The following example shows how to delete an image recipe by specifying its ARN\.

```
aws imagebuilder delete-image-recipe --image-recipe-arn arn:aws:imagebuilder:us-west-2:123456789012:image-recipe/my-example-recipe/2019.12.03
```

**Delete an infrastructure configuration**  
The following example shows how to delete an infrastructure configuration resource by specifying its ARN\.

```
aws imagebuilder delete-infrastructure-configuration --infrastructure-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration
```

**Delete distribution settings**  
The following example shows how to delete a distribution settings resource by specifying its ARN\.

```
aws imagebuilder delete-distribution-configuration --distribution-configuration-arn arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration
```

**Delete an image**  
The following example shows how to delete an image build version by specifying its ARN\.

```
aws imagebuilder delete-image --image-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:image/my-example-image/2019.12.02/1
```

**Delete a component**  
The following example shows how to use an imagebuilder CLI command to delete a component build version by specifying its ARN\.

```
aws imagebuilder delete-component --component-build-version-arn arn:aws:imagebuilder:us-west-2:123456789012:component/my-example-component/2019.12.02/1
```

**Important**  
Make sure there are no recipes that reference the component build version in any way before you delete it\. Failing to do so could cause pipeline failures\.