# Delete resources<a name="image-builder-delete-pipeline"></a>

Your Image Builder environment, just like your home, needs regular maintenance to help you find what you need, and complete your tasks without wading through clutter\. Make sure to regularly clean up temporary resources that you created for testing\. Otherwise, you might forget about those resources, and then later, not remember what they were used for\. By then, it might not be clear if you can safely get rid of them\.

**Tip**  
To prevent dependency errors when you delete resources, make sure to delete your resources in the following order:  
Image pipeline
Image recipe
All remaining resources

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