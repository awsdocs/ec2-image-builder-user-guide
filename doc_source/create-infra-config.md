# Create an infrastructure configuration<a name="create-infra-config"></a>

This section describes how you can use the Image Builder console or imagebuilder commands in the AWS CLI to create an infrastructure configuration,

------
#### [ Console ]

To create an infrastructure configuration resource from the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. From the navigation pane, choose **Infrastructure configuration**\.

1. Choose **Create infrastructure configuration**\.

1. In the **General** section, enter the following required information:
   + Enter the **Name** of your infrastructure configuration resource\.
   + Select an **IAM role** that you want to associate with the instance profile for your build and test instances\. Image Builder uses these permissions to download and run your components, upload logs to CloudWatch, and perform any additional actions that the components in your recipe specify\.

1. In the **AWS infrastructure** panel, you can configure all of the remaining infrastructure settings that are available\. Enter the following required information:
   + **Instance type** – You can specify one or more instance types to use for this build\. The service will pick one of these instance types based on availability\.

   If you do not supply values for the following settings, they use service\-specific defaults, where applicable\.
   + **VPC, subnet, and security groups** – Image Builder uses your default VPC and subnet\. For more information about configuring VPC interface endpoints, see [EC2 Image Builder and interface VPC endpoints \(AWS PrivateLink\)](vpc-interface-endpoints.md)\.
   + In the **Troubleshooting settings** section, you can configure the following values:
     + By default, the **Terminate instance on failure** check box is selected\. However, when a build fails, you can log on to the EC2 instance to troubleshoot\. If you want your instance to continue to run after a build failure, clear the check box\.
     + **Key pair** – If your EC2 instance continues to run after a build failure, you can create a key pair or use an existing key pair to log on to the instance and troubleshoot\.
     + **Logs** – You can specify an S3 bucket where Image Builder can write application logs to help troubleshoot your build and tests\. If you don't specify an S3 bucket, Image Builder writes the application logs to the instance\.
   + In the **Instance metadata settings** section, you can configure the following values to apply to the EC2 instances that Image Builder uses to build and test your image:
     + Select the **Metadata version** to determine if EC2 requires a signed token header for instance metadata retrieval requests\.
       + **V1 and V2 \(token optional\)** – Default value if you don't select anything\.
       + **V2 \(token required\)**
**Note**  
We recommend that you configure all EC2 instances that Image Builder launches from a pipeline build to use IMDSv2 so that instance metadata retrieval requests require a signed token header\.
     + **Metadata token response hop limit** – The number of network hops that the metadata token can travel\. Minimum hops: 1, maximum hops: 64, with a default of one hop\.

------
#### [ AWS CLI ]

The following example shows how to configure the infrastructure for your image with the Image Builder[create\-infrastructure\-configuration](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/create-infrastructure-configuration.html) command in the AWS CLI\.

1. 

**Create a CLI input JSON file**

   This infrastructure configuration example specifies two instance types, `m5.large` and `m5.xlarge`\. We recommend that you specify more than one instance type because this allows Image Builder to launch an instance from a pool with sufficient capacity\. This can reduce your transient build failures\.

   The `instanceProfileName` specifies the instance profile that provides the instance with the permissions that the profile requires to perform customization activities\. For example, if you have a component that retrieves resources from Amazon S3, the instance profile requires permissions to access those files\. The instance profile also requires a minimal set of permissions for EC2 Image Builder to successfully communicate with the instance\. For more information, see [Prerequisites](image-builder-setting-up.md)\.

   Use a file editing tool to create a JSON file with keys shown in the following example, plus values that are valid for your environment\. This example uses a file named `create-infrastructure-configuration.json`:

   ```
   {
       "name": "MyExampleInfrastructure",
       "description": "An example that will retain instances of failed builds",
       "instanceTypes": [
           "m5.large", "m5.xlarge"
       ],
       "instanceProfileName": "myIAMInstanceProfileName",
       "securityGroupIds": [
           "sg-12345678"
       ],
       "subnetId": "sub-12345678",
       "logging": {
           "s3Logs": {
               "s3BucketName": "my-logging-bucket",
               "s3KeyPrefix": "my-path"
           }
       },
       "keyPair": "myKeyPairName",
       "terminateInstanceOnFailure": false,
       "snsTopicArn": "arn:aws:sns:us-west-2:123456789012:MyTopic"
   }
   ```

1. 

**Use the file you created as input when you run the following command\.**

   ```
   aws imagebuilder create-infrastructure-configuration --cli-input-json file://create-infrastructure-configuration.json
   ```

------