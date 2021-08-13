# Troubleshoot EC2 Image Builder<a name="image-builder-troubleshooting"></a>

EC2 Image Builder integrates with AWS services for monitoring and troubleshooting to help you troubleshoot image build issues\. EC2 Image Builder tracks and displays the progress for each step in the image building process\. Image Builder can export logs to an Amazon S3 location that you provide\. For advanced troubleshooting, you can run predefined commands and scripts using [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\.

## General troubleshooting<a name="image-builder-general-troubleshooting"></a>

If an Image Builder pipeline fails, Image Builder will return an error message that describes the failure\. Image Builder will also return an Systems Manager execution ID in the failure message, such as the one in the following example\.

```
Systems Manager execution 'aaaaaaaa-bbbb-cccc-dddd-example12345' failed with status…
```

Image Builder uses AWS Systems Manager Automation to orchestrate actions when an image is built\. To review additional details to help troubleshoot a build failure, search the Systems Manager Automation console for the execution ID provided by Image Builder and review the Automation execution\.

All build activity is also logged in AWS CloudTrail if it is enabled in your account\. Filter CloudTrail events by the source “SYS\.amazonaws\.com", or search for the Amazon EC2 instance ID returned in the execution log to see more details about the pipeline execution\.

By default, the Amazon EC2 instance that is used for build and test activity is terminated when the pipeline completes\. If you encounter pipeline failures, you have the option to retain this instance for troubleshooting\. Unselect the “Terminate instance on failure” option for your pipeline if you experience failures and want to access the instance to debug the issues\.

The logs that you send to your S3 bucket show the steps and error messages for activity on the EC2 instance during the image build process\. The logs include log outputs from the component manager, the definitions of the components that were executed, and the detailed output \(in JSON\) of all of the steps taken on the instance\. If you encounter an issue, you should review these files, starting with the `application.log`, to diagnose the cause of the problem on the instance\. 

## Troubleshooting scenarios<a name="image-builder-troubleshooting-scenarios"></a>
+ **Build fails with "AccessDenied: Access Denied status code: 403"**
  + **Possible cause**: The instance profile role does not have the required permissions to access APIs or resources used by components, or for logging to S3\. Most commonly, this occurs when the instance profile role does not have **PutObject** permissions for your S3 buckets, or when the instance profile does not have the following role policies associated with it: **EC2InstanceProfileForImageBuilder**, **EC2InstanceProfileForImageBuilderECRContainerBuilds**, and **AmazonSSMManagedInstanceCore**\.
  +  **Resolution**: Add a policy to your instance profile role that grants permission to access APIs and resources used in the recipe, or attach the **EC2InstanceProfileForImageBuilder**, **EC2InstanceProfileForImageBuilderECRContainerBuilds**, and **AmazonSSMManagedInstanceCore**  IAM role policies to the instance profile\. 
  + **Possible cause**: The instance profile role does not have the required permissions to access APIs or resources used by components, or for logging to S3\. Most commonly, this occurs when the instance profile role does not have **PutObject** permissions for your S3 buckets\. 
  +  **Resolution**: Add a policy to your instance profile role that grants permission to access APIs and resources used in the recipe, and run the pipeline again\.
+ **Build fails with "status = 'TimedOut'" and "failure message = 'Step timed out while step is verifying the Systems Manager Agent availability on the target instance\(s\)'"**
  + **Possible cause**: The instance launched to perform the build operations and execute components was not able to access the Systems Manager endpoint\. 
  + **Resolutions**: If you are building in a private subnet, make sure you have set up PrivateLink endpoints for Systems Manager, Image Builder, and, if you want logging, Amazon S3/CloudWatch\. For more information about setting up PrivateLink endpoints, see [VPC endpoints concepts \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/privatelink/endpoint-services-overview.html)\.
  + **Possible cause**: The instance profile does not have the required [permissions](image-builder-setting-up.md#image-builder-IAM-prereq)\. 
  + **Resolutions**: Add the following managed policies to your IAM role: : **EC2InstanceProfileForImageBuilder**, **EC2InstanceProfileForImageBuilderECRContainerBuilds**, and **AmazonSSMManagedInstanceCore**\.
+ **You are using a CIS hardened base image and the build fails**
  + **Possible cause**: The `/tmp` directory is classified as `noexec`, which can cause Image Builder to fail\. 
  + **Resolution**: Choose a different location to use as the working directory in the `workingDirectory` field of the image recipe\. For more information, see the [ImageRecipe](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ImageRecipe.html) data type description\.