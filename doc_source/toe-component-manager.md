# AWS Task Orchestrator and Executor component manager<a name="toe-component-manager"></a>

EC2 Image Builder uses the AWS Task Orchestrator and Executor \(AWSTOE\) application to orchestrate complex workflows, modify system configurations, and test your systems without writing code\. This application uses a declarative document schema\. Because it is a standalone application, it does not require additional server setup\. It can run on any cloud infrastructure and on premises\. 

To install AWSTOE, choose the download link for your architecture and platform\.

**Topics**
+ [AWSTOE downloads](#toe-downloads)
+ [Supported Regions](#toe-supported-regions)
+ [Get started with AWSTOE](toe-get-started.md)
+ [Use component documents in AWSTOE](toe-use-documents.md)
+ [Action modules supported by AWSTOE component manager](toe-action-modules.md)
+ [EC2 Image Builder STIG components](toe-stig.md)
+ [AWSTOE command reference](toe-commands.md)

## AWSTOE downloads<a name="toe-downloads"></a>


| Architecture | Platform | Download link | Example | 
| --- | --- | --- | --- | 
|  386  |  AL2, RHEL 8 and 7, Ubuntu 18 and 16, CentOS 7, SUSE 15  | `https://awstoe-<region>.s3.<region>.amazonaws.com/latest/linux/386/awstoe`  | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/386/awstoe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/386/awstoe) | 
|  AMD64  |  Windows Server 2019/2016/2012 R2/version 1909  |   `https://awstoe-<region>.s3.<region>.amazonaws.com/latest/windows/amd64/awstoe.exe`  | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/windows/amd64/awstoe.exe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/windows/amd64/awstoe.exe) | 
|  AMD64  |  AL2, RHEL 8 and 7, Ubuntu 18 and 16, CentOS 7, SUSE 15  | https://awstoe\-<region>\.s3\.<region>\.amazonaws\.com/latest/linux/amd64/awstoe | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/amd64/awstoe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/amd64/awstoe) | 
| ARM64 | AL2, RHEL 8 and 7, Ubuntu 18 and 16, CentOS 7, SUSE 15 | https://awstoe\-<region>\.s3\.<region>\.amazonaws\.com/latest/linux/arm64/awstoe | [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/arm64/awstoe](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/arm64/awstoe) | 

## Supported Regions<a name="toe-supported-regions"></a>

AWSTOE is supported as a standalone application in the following Regions\.


| Region name | Region | 
| --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  | 
|  US West \(N\. California\)  | us\-west\-1  | 
|  US West \(Oregon\)  | us\-west\-2  | 
|  Africa \(Cape Town\)  | af\-south\-1  | 
|  Asia Pacific \(Hong Kong\)  | ap\-east\-1  | 
|  Asia Pacific \(Mumbai\)  | ap\-south\-1  | 
|  Asia Pacific \(Osaka\)  | ap\-northeast\-3  | 
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