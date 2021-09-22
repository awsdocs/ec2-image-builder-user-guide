# Troubleshoot EC2 Image Builder<a name="troubleshooting"></a>

EC2 Image Builder integrates with AWS services for monitoring and troubleshooting to help you troubleshoot image build issues\. Image Builder tracks and displays the progress for each step in the image building process\. Additionally, Image Builder can export logs to an Amazon S3 location that you provide\.

For advanced troubleshooting, you can run predefined commands and scripts using [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\.

**Topics**
+ [Troubleshoot pipeline builds](#troubleshooting-pipelines)
+ [Troubleshooting scenarios](#image-builder-troubleshooting-scenarios)

## Troubleshoot pipeline builds<a name="troubleshooting-pipelines"></a>

If an Image Builder pipeline build fails, Image Builder returns an error message that describes the failure\. Image Builder also returns a Systems Manager execution ID in the failure message, such as the one in the following example\.

```
Systems Manager execution 'aaaaaaaa-bbbb-cccc-dddd-example12345' failed with status…
```

Image Builder uses AWS Systems Manager Automation to orchestrate image build actions\. To review additional details to help troubleshoot a build failure, search the Systems Manager Automation console for the execution ID provided by Image Builder and review the Automation execution\.

All build activity is also logged in AWS CloudTrail if it is enabled in your account\. Filter CloudTrail events by the source `imagebuilder.amazonaws.com`, or search for the Amazon EC2 instance ID that is returned in the execution log to see more details about the pipeline execution\.

By default, Image Builder shuts down the Amazon EC2 build or test instance that is running when the pipeline fails\. You can change the instance settings for the infrastructure configuration resource that your pipeline uses, to retain your build or test instance for troubleshooting\.

To change the instance settings in the console, you must clear the **Terminate instance on failure** check box located in the **Troubleshooting settings** section of your infrastructure configuration resource\.

To change the instance settings using the imagebuilder update\-infrastructure\-configuration command in the AWS CLI, set the `terminateInstanceOnFailure` value to `false` in the JSON file that the command references with the `--cli-input-json` parameter\. For details, see [Update an infrastructure configuration \(AWS CLI\)](create-infra-config.md#cli-update-infrastructure-configuration)\.

The logs that you send to your S3 bucket show the steps and error messages for activity on the EC2 instance during the image build process\. The logs include log outputs from the component manager, the definitions of the components that were run, and the detailed output \(in JSON\) of all of the steps taken on the instance\. If you encounter an issue, you should review these files, starting with the `application.log`, to diagnose the cause of the problem on the instance\. 

## Troubleshooting scenarios<a name="image-builder-troubleshooting-scenarios"></a>

This section lists the following detailed troubleshooting scenarios:
+ [Access denied – status code 403](#ts-access-denied)
+ [Build times out while verifying the Systems Manager Agent availability on the build instance](#ts-timeout-ssm-agent)
+ [Windows secondary disk is offline at launch](#ts-win-disk-offline)
+ [Build fails with CIS hardened base image](#ts-cis-base)

To see the details of a scenario, choose the scenario title to expand it\. You can have multiple titles expanded at the same time\.

### Access denied – status code 403<a name="ts-access-denied"></a>

#### Description<a name="ts-access-denied-descr"></a>

The pipeline build fails with "AccessDenied: Access Denied status code: 403"\.

#### Cause<a name="ts-access-denied-cause"></a>

Possible causes include:
+ The instance profile does not have the required [permissions](image-builder-setting-up.md#image-builder-IAM-prereq) to access APIs or component resources\.
+ The instance profile role is missing permissions that are required for logging to Amazon S3\. Most commonly, this occurs when the instance profile role does not have **PutObject** permissions for your S3 buckets\.

#### Solution<a name="ts-access-denied-solution"></a>

Depending on the cause, this issue can be resolved as follows:
+ **Instance profile is missing managed policies** – Add the missing policies to your instance profile role\. Then run the pipeline again\.
+ **Instance profile is missing write permissions for S3 bucket** – Add a policy to your instance profile role that grants **PutObject** permissions to write to your S3 bucket\. Then run the pipeline again\.

### Build times out while verifying the Systems Manager Agent availability on the build instance<a name="ts-timeout-ssm-agent"></a>

#### Description<a name="ts-timeout-ssm-agent-descr"></a>

The pipeline build fails with "status = 'TimedOut'" and "failure message = 'Step timed out while step is verifying the Systems Manager Agent availability on the target instance\(s\)'"\.

#### Cause<a name="ts-timeout-ssm-agent-cause"></a>

Possible causes include:
+ The instance that was launched to perform the build operations and to run components was not able to access the Systems Manager endpoint\.
+ The instance profile does not have the required [permissions](image-builder-setting-up.md#image-builder-IAM-prereq)\.

#### Solution<a name="ts-timeout-ssm-agent-solution"></a>

Depending on the possible cause, this issue can be resolved as follows:
+ **Access issue, private subnet** – If you are building in a private subnet, make sure that you have set up PrivateLink endpoints for Systems Manager, Image Builder, and, if you want logging, Amazon S3/CloudWatch\. For more information about setting up PrivateLink endpoints, see [VPC endpoints concepts \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/privatelink/endpoint-services-overview.html)\.
+ **Missing permissions** – Add the following managed policies to your IAM service\-linked role for Image Builder:
  + EC2InstanceProfileForImageBuilder
  + EC2InstanceProfileForImageBuilderECRContainerBuilds
  + AmazonSSMManagedInstanceCore

  For more information about the Image Builder service\-linked role, see [Using service\-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)\.

### Windows secondary disk is offline at launch<a name="ts-win-disk-offline"></a>

#### Description<a name="ts-win-disk-offline-descr"></a>

When the instance type used to build an Image Builder Windows AMI does not match the instance type that is used to launch from the AMI, an issue can occur where non\-root volumes are offline at launch\. This primarily happens when the build instance is using a newer architecture than the launch instance\.

The following example demonstrates what happens when an Image Builder AMI is built on an EC2 Nitro instance type and launched on an EC2 Xen instance:

**Build instance type**: m5\.large \(Nitro\)

**Launch instance type**: t2\.medium \(Xen\)

```
PS C:\Users\Administrator> get-disk
Number  Friendly Name  Serial Number         Health Status  Operational Status  Total Size  Partition Style
------  -------------  -------------         -------------  ------------------  ----------  ---------------
0       AWS PVDISK     vol0abc12d34e567f8a9  Healthy        Online                   30 GB  MBR
1       AWS PVDISK     vol1bcd23e45f678a9b0  Healthy        Offline                   8 GB  MBR
```

#### Cause<a name="ts-win-disk-offline-cause"></a>

Because of Windows default settings, newly discovered disks are not automatically brought online and formatted\. When the instance type is changed on EC2, Windows treats this as new disks being discovered\. This is because of the underlying driver change\.

#### Solution<a name="ts-win-disk-offline-solution"></a>

We recommend that you use the same system of instance types when building your Windows AMI that you intend to launch from\. Do not include instance types that are built on different systems in your infrastructure configuration\. If any of the instance types you specify use the Nitro system, then they should all use the Nitro system\.

For more information about instances that are built on the Nitro system, see [Instances built on the Nitro System](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-types.html#ec2-nitro-instances) in the *Amazon EC2 User Guide for Windows Instances*\.

### Build fails with CIS hardened base image<a name="ts-cis-base"></a>

#### Description<a name="ts-cis-base-descr"></a>

You are using a CIS hardened base image and the build fails\.

#### Cause<a name="ts-cis-base-cause"></a>

When the `/tmp` directory is classified as `noexec`, it can cause Image Builder to fail\.

#### Solution<a name="ts-cis-base-solution"></a>

Choose a different location for your working directory in the `workingDirectory` field of the image recipe\. For more information, see the [ImageRecipe](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_ImageRecipe.html) data type description\.