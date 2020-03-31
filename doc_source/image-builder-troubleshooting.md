# Troubleshooting EC2 Image Builder<a name="image-builder-troubleshooting"></a>

EC2 Image Builder integrates with AWS services for monitoring and troubleshooting to help you troubleshoot image build issues\. EC2 Image Builder tracks and displays the progress for each step in the image building process\. Logs are exported to an Amazon S3 location that you provide\. For advanced troubleshooting, you can run predefined commands and scripts using [AWS Systems Manager \(SSM\) Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\.

## General Troubleshooting<a name="image-builder-general-troubleshooting"></a>

If an Image Builder pipeline fails, Image Builder will return an error message that describes the failure\. Image Builder will also return an SSM execution ID in the failure message, such as the one in the following example\.

```
SSM execution 'aaaaaaaa-bbbb-cccc-dddd-example12345' failed with status…
```

Image Builder uses AWS Systems Manager \(SSM\) Automation to orchestrate actions when an image is built\. To review additional details to help troubleshoot a build failure, search the SSM Automation console for the execution ID provided by Image Builder and review the Automation execution\.

All build activity is also logged in AWS CloudTrail if it is enabled in your account\. Filter CloudTrail events by the source “ssm\.amazonaws\.com", or search for the Amazon EC2 instance ID returned in the execution log to see more details about the pipeline execution\.

By default, the Amazon EC2 instance that is used for build and test activity is terminated when the pipeline completes\. If you encounter pipeline failures, you have the option to retain this instance for troubleshooting\. Unselect the “Terminate instance on failure” option for your pipeline if you experience failures and want to access the instance to debug the issues\.

## Troubleshooting Scenarios<a name="image-builder-troubleshooting-scenarios"></a>
+ **Build fails with "AccessDenied: Access Denied status code: 403"**
  + **Issue**: The instance profile role does not have the required permissions to access APIs or resources used by components, or for logging to S3\. Most commonly, this occurs when the instance profile role does not have **PutObject** permissions for your S3 buckets, or when the instance profile does not have the following role policies associated with it: **EC2InstanceProfileForImageBuilder** and **AmazonSSMManagedInstanceCore**\.
  +  **Resolution**: Add a policy to your instance profile role that grants permission to access APIs and resources used in the recipe, or attach the **EC2InstanceProfileForImageBuilder** and **AmazonSSMManagedInstanceCore** IAM role policies to the instance profile\. 
  + **Issue**: The instance profile role does not have the required permissions to access APIs or resources used by components, or for logging to S3\. Most commonly, this occurs when the instance profile role does not have **PutObject** permissions for your S3 buckets\. 
  +  **Resolution**: Add a policy to your instance profile role that grants permission to access APIs and resources used in the recipe, and run the pipeline again\.
+ **Build fails with "An error occurred \(ValidationError\) when calling the CreateAutoScalingGroup operation: Provided Auto Scaling group launches instances into EC2\-Classic, which is not compatible with a mixed instances policy"**
  +  **Issue**: Image Builder attempted to launch an instance into an Amazon EC2\-Classic network\. Image Builder only supports VPC networking for build operations\. 
  +  **Resolution**: Specify a specific VPC in your pipeline's infrastructure settings\. 
+ **Build fails with "status = 'TimedOut'" and "failure message = 'Step timed out while step is verifying the SSM Agent availability on the target instance\(s\)'"**
  + **Issue**: The instance launched to perform the build operations and execute components was not able to access the Systems Manager endpoint\. 
  + **Resolutions**: Ensure that the subnet used for your image build has access to and routes to the Systems Manager endpoint\. 
