# Update a container image pipeline \(console\)<a name="update-container-pipelines-console"></a>

After you have created an Image Builder container image pipeline for your Docker image, you can make changes to the infrastructure configuration and distribution settings from the Image Builder console\.

To update a container image pipeline with a new container recipe, you must use the AWS CLI\. For more information, see [Update container image pipelines \(AWS CLI\)](cli-update-container-pipeline.md) in this guide\.

**Choose an existing Image Builder Docker image pipeline**

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. To see a list of the image pipelines created under your account, choose **Image pipelines** from the navigation pane\.
**Note**  
The list of image pipelines includes an indicator for the type of output image that is created by the pipeline – AMI or Docker\.

1. To view details or edit a pipeline, choose the **Pipeline name** link\. This opens the detail view for the pipeline\.
**Note**  
You can also choose the check box next to the **Pipeline name**, then choose **View detail**\.

## Pipeline details<a name="container-pipeline-details"></a>

The EC2 Image Builder pipeline details page includes the following sections:

****Summary****  
The section at the top of the page summarizes key details for the pipeline that are visible with any of the detail tabs open\. The details displayed in this section are editable only on their respective detail tabs\.

**Detail tabs**
+ **Output images** – Shows output images that the pipeline has produced\.
+ **Container recipe** – Shows recipe details\. After you create a recipe, you cannot edit it\. You must create a new version of the recipe from the **Container recipes** page\. For more information, see [Create container recipes and versions](create-container-recipes.md)\.
+ **Infrastructure configuration** – Shows editable information for configuring your build pipeline infrastructure\.
+ **Distribution settings** – Shows editable information for Docker image distribution\.

## Edit infrastructure configuration for your pipeline<a name="container-pipelines-edit-infra-config"></a>

Infrastructure configuration includes the following details that you can edit after creating the pipeline:
+ The **Description** for your infrastruction configuration\.
+ The **IAM role** to associate with the instance profile\.
+ **AWS infrastructure**, including the **Instance type** and an **SNS topic** for notifications\.
+ **VPC, subnet, and security groups**\.
+ **Troubleshooting settings**, including **Terminate instance on failure**, the **Key pair** for connecting, and an optional S3 bucket location for instance logs\.

To edit infrastructure configuration from the pipeline details page, follow these steps:

1. Choose the **Infrastructure configuration** tab\.

1. Choose **Edit** from the upper right corner of the **Configuration details** panel\.

1. When you are ready to save updates you've made to your infrastructure configuration, choose **Save changes**\.

## Edit distribution settings for your pipeline<a name="container-pipelines-edit-dist-settings"></a>

Distribution settings include the following details that you can edit after creating the pipeline:
+ The **Description** for your distribution settings\.
+ **Region settings** for the Regions where you distribute your image\. Region 1 defaults to the Region where you created the pipeline\. You can add Regions for distribution with the **Add Region** button, and you can remove all Regions except Region 1\.

  **Region settings** include:
  + Target **Region**
  + The **Service** defaults to "ECR", and is not editable\.
  + **Repository name** – the name of your target repository \(*not including the Amazon ECR location*\)\. For example, the repository name with the location would look like the following pattern:

    `<account-id>.dkr.ecr.<region>.amazonaws.com/<repository-name>`
**Note**  
If you change the **Repository name**, only the images created after the name change will be added under the new name\. Any prior images that your pipeline created remain in their original repository\.

To edit your distribution settings from the pipeline details page, follow these steps:

1. Choose the **Distribution settings** tab\.

1. Choose **Edit** from the upper right corner of the **Distribution details** panel\.

1. When you are ready to save updates you've made to your distribution settings, choose **Save changes**\.

## Edit the build schedule for your pipeline<a name="container-pipelines-edit-build-schedule"></a>

The **Edit pipeline** page includes the following details that you can edit after creating the pipeline:
+ The **Description** for your pipeline\.
+ **Enhanced metadata collection**\. This is turned on by default\. To turn it off, clear the **Enable enhanced metadata collection** check box\.
+ The **Build schedule** for your pipeline\. You can change your **Schedule options** and all of the settings in this section\.

To edit your pipeline from the pipeline details page, follow these steps:

1. In the upper right corner of the pipeline details page, choose **Actions**, and then **Edit pipeline**\.

1. When you are ready to save your updates, choose **Save changes**\.

**Note**  
For more information about scheduling your build using cron expressions, see [Use cron expressions in EC2 Image Builder](cron-expressions.md)\.