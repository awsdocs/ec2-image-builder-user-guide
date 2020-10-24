# EC2 Image Builder component manager<a name="image-builder-component-manager"></a>

Image Builder uses a component management application \(AWSTOE\) to orchestrate complex workflows, modify system configurations, and test your systems without writing code\. This application uses a declarative document schema\. Because it is a standalone application, it does not require additional server setup\. It can run on any cloud infrastructure and on premises\. 

To install AWSTOE, choose the download link for your architecture and platform\.


| Architecture | Platform | Download link | Example | 
| --- | --- | --- | --- | 
|  386  |  AL2, RHEL 8 and 7, Ubuntu 18 and 16, CentOS 7, SUSE 15  | `https://awstoe-<region>.s3.<region>.amazonaws.com/latest/linux/386/awstoe`  | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/386/awstoe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/386/awstoe) | 
|  AMD64  |  Windows Server 2019/2016/2012 R2/version 1909  |   `https://awstoe-<region>.s3.<region>.amazonaws.com/latest/windows/amd64/awstoe.exe`  | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/windows/amd64/awstoe.exe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/windows/amd64/awstoe.exe) | 
|  AMD64  |  AL2, RHEL 8 and 7, Ubuntu 18 and 16, CentOS 7, SUSE 15  | https://awstoe\-<region>\.s3\.<region>\.amazonaws\.com/latest/linux/amd64/awstoe | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/amd64/awstoe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/amd64/awstoe) | 
| ARM64 | AL2, RHEL 8 and 7, Ubuntu 18 and 16, CentOS 7, SUSE 15 | https://awstoe\-<region>\.s3\.<region>\.amazonaws\.com/latest/linux/arm64/awstoe | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/arm64/awstoe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/arm64/awstoe) | 

EC2 Image Builder uses this application to perform all on\-instance activities, such as build, validation, and test\. You define a document that describes how to build, validate, and test your image\. EC2 Image Builder sends the component to your instance and the application interprets and applies it to your instance by running the defined phases, steps, and actions\. When complete, the application sends a summary to EC2 Image Builder\. It also sends detailed outputs to Amazon S3 if you specified an S3 bucket in your pipeline configuration\. EC2 Image Builder then cleans up the application and removes it from the instance using [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements)\. 

The AWSTOE application performs the following phases:
+ **Build phase**\. The image is modified\. For example, you can configure your image to install an application or to modify the operating system firewall settings\. The validate phase is run as part of the build phase, prior to the creation of the image\. 
+ **Test phase**\. Tests are run against your new image after it is created\.

EC2 Image Builder uses the component management application, as follows\.

1. Define an EC2 Image Builder component, which is a document that describes how to build, validate, and test your image\.

1. EC2 Image Builder dispatches the work to be performed by copying the document and application to your instance\.

1. The application runs the phases, steps, and actions defined in the document\. 

**Supported Regions**

The AWSTOE application is supported as a standalone application in the following Regions\.


| Region name | Region | 
| --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | 
|  US West \(N\. California\)  | us\-west\-1  | 
|  US West \(Oregon\)  | us\-west\-2  | 
|  Africa \(Cape Town\)  | af\-south\-1  | 
|  Asia Pacific \(Hong Kong\)  | ap\-east\-1  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1  | 
|  Asia Pacific \(Seoul\)  | ap\-northeast\-2  | 
|  Asia Pacific \(Singapore\)  | ap\-southeast\-1  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1  | 
|  Canada \(Central\)  | ca\-central\-1  | 
|  Europe \(Frankfurt\)  | eu\-central\-1  | 
|  Europe \(Ireland\)  | eu\-west\-1  | 
|  Europe \(London\)  | eu\-west\-2  | 
|  Europe \(Milan\)  | eu\-south\-1  | 
|  Europe \(Paris\)  | eu\-west\-3  | 
|  Europe \(Stockholm\)  | eu\-north\-1  | 
|  Middle East \(Bahrain\)  | me\-south\-1  | 
|  South America \(São Paulo\)  | sa\-east\-1  | 

**Topics**
+ [Use documents in EC2 Image Builder](image-builder-application-documents.md)
+ [Develop Image Builder components locally](image-builder-component-manager-local.md)
+ [Use looping constructs in the AWSTOE application](image-builder-looping-constructs.md)
+ [Define and reference variables in the AWSTOE application](image-builder-component-manager-user-defined-variables.md)
+ [Component manager supported action modules](image-builder-action-modules.md)
+ [Verify the signature of the AWSTOE installation download](awstoe-verify-sig.md)
+ [EC2 Image Builder STIG components](image-builder-stig.md)