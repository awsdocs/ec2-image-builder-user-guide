# Amazon Inspector integration in Image Builder<a name="integ-inspector"></a>

When you activate security scanning with Amazon Inspector, it continuously scans machine images and running instances in your account for operating system and programming language vulnerabilities\. If activated, security scanning is automatic, and Image Builder can save a snapshot of the findings for your build instances when you create a new image\. Amazon Inspector is a paid service\.

When Amazon Inspector discovers vulnerabilities in your software or network settings, it takes the following actions:
+ Notifies you that there was a finding\.
+ Rates the severity of the finding\. The severity rating categorizes vulnerabilities to help you prioritize your findings, and includes the following values:
  + Untriaged
  + Informational
  + Low
  + Medium
  + High
  + Critical
+ Provides information about the finding, and links to additional resources for more detail\.
+ Offers remediation guidance to help you resolve the issues that generated the finding\.

**Configure security scans**  
If you've activated Amazon Inspector for your account, Amazon Inspector automatically scans the EC2 instances that Image Builder launches to build and test a new image\. Those instances have a short lifespan during the build and test process, and their findings would normally expire as soon as those instances shut down\. To help you investigate and remediate findings for your new image, Image Builder can optionally save any findings that Amazon Inspector identified for your image during the build process as a snapshot\.

To configure security scans for your pipeline, see [Configure security scans for Image Builder images in the AWS Management Console](image-security-findings.md#image-config-security-scans)\.

**Review security findings**  
In the Image Builder console, you can view security findings for all of your Image Builder resources in one place\. The **Security findings** page in the **Security Overview** section shows all findings, or you can group your findings by vulnerability, by image pipeline, or by image\. The console defaults to display all security findings\. The summary panel for the **All security findings** option shows the number of findings that you have for each severity level\. For more information, see [Manage security findings for Image Builder images in the AWS Management Console](image-security-findings.md#image-manage-security-findings)\.

To learn more about Amazon Inspector vulnerability findings, see [Understanding findings in Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/findings-understanding.html) in the *Amazon Inspector User Guide*\.