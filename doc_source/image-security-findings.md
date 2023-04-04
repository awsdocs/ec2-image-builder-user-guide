# Manage security findings for Image Builder images<a name="image-security-findings"></a>

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

## Configure security scans for Image Builder images in the AWS Management Console<a name="image-config-security-scans"></a>

If you've activated Amazon Inspector for your account, Amazon Inspector automatically scans the EC2 instances that Image Builder launches to build and test a new image\. Those instances have a short lifespan during the build and test process, and their findings would normally expire as soon as those instances shut down\. To help you investigate and remediate findings for your new image, Image Builder can optionally save any findings that Amazon Inspector identified for your image during the build process as a snapshot\.

**Step 1: Activate Amazon Inspector security scans for your account**  
To activate Amazon Inspector security scans for your account from the Image Builder console, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Security scanning settings** from the navigation pane\. This opens the **Security scanning** dialog box\.

   The dialog box displays the scanning status for your account\. If Amazon Inspector is already activated for your account, the status shows **Enabled**\.

1. Follow steps 1 and 2 of the instructions to activate Amazon Inspector scanning\.
**Note**  
Amazon Inspector is a paid service\. For more information, see [Amazon Inspector pricing](http://aws.amazon.com/inspector/pricing/)\.

If you've enabled scanning for your pipeline, Image Builder takes a snapshot of the findings for your build instance when you create a new image, so that you have access to the findings after Image Builder terminates the build instance\.

**Step 2: Configure your pipeline to save snapshots for vulnerability findings**  
To configure vulnerability findings snapshots for your pipeline, follow these steps:

1. Open the EC2 Image Builder console at [https://console\.aws\.amazon\.com/imagebuilder/](https://console.aws.amazon.com/imagebuilder/)\.

1. Choose **Image pipelines** from the navigation pane\.

1. Pick one of the following methods to specify pipeline details:

**Create a new pipeline**

   1. From the **Image pipelines** page, choose **Create image pipeline**\. This opens the **Specify pipeline details** page in the pipeline wizard\.

**Update an existing pipeline**

   1. From the **Image pipelines** page, choose the **Pipeline name** link for the pipeline that you want to update\. This opens the pipeline detail page\.
**Note**  
Alternatively, you can select the check box next to the name of the pipeline that you want to update, and then choose **View details**\.

   1. From the pipeline details page, select **Edit pipeline** from the **Action** menu\. This takes you to the **Edit pipeline** page\.

1. In the **General** section from the pipeline wizard or the **Edit pipeline** page, check the box to **Enable security scan**\.
**Note**  
If you want to turn off the snapshots later, you can edit your pipeline to clear the check box\. This doesn't deactivate Amazon Inspector scanning for your account\. To deactivate Amazon Inspector scanning, see [Deactivating Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/deactivating-best-practices.html) in the *Amazon Inspector User Guide*\.

## Manage security findings for Image Builder images in the AWS Management Console<a name="image-manage-security-findings"></a>

The **Security findings** list pages display high level information about the findings for your resources, with views based on several different filters that you can apply\. Each view includes the following options at the top to change your view:
+ **All security findings** – This is the default view if you choose **Security findings** page from the navigation pane in the Image Builder console\.
+ **By vulnerability** – This view shows a high level list of all of the image resources in your account that have findings\. The **Finding ID** is linked to more detailed information about the finding that opens in a panel on the right side of the page\. The panel includes the following information:
  + A detailed description of the finding\.
  + A **Finding details** tab\. This tab includes a finding overview, affected packages, summary remediation advice, vulnerability details, and related vulnerabilities\. The **Vulnerability ID** links to detailed vulnerability information in the National Vulnerability Database\.
  + A **Score breakdown** tab\. This tab includes a side by side comparison of the CVSS and Amazon Inspector scores, so that you can see where Amazon Inspector has modified a score, if applicable\.
+ **By image pipeline** – This view shows the number of findings for each image pipeline in your account\. Image Builder displays counts for medium severity and higher findings, plus a total for all findings\. All data in the list is linked, as follows:
  + The **Image pipeline name** column links to the detail page for the specified image pipeline\.
  + The severity level column links open the **All security findings** view, filtered by the associated image pipeline name and severity level\.

  You can also use search criteria to refine your results\.
+ **By image** – This view shows the number of findings for each image build in your account\. Image Builder displays counts for medium severity and higher findings, plus a total for all findings\. All data in the list is linked, as follows:
  + The **Image name** column links to the image detail page for the specified image build\. For more information, see [View image details](view-image-details.md)\.
  + The severity level column links open the **All security findings** view, filtered by the associated image build name and severity level\.

  You can also use search criteria to refine your results\.

Image Builder shows the following details in the **Findings** list section of the default **All security findings** view\.

**Severity**  
The severity level of the CVE finding\. Values are as follows:  
+ Untriaged
+ Informational
+ Low
+ Medium
+ High
+ Critical

**Finding ID**  
The unique identifier for the CVE finding that Amazon Inspector detected for your image when it scanned the build instance\. The ID is linked to the **Security findings > By vulnerability** page\.

**Image ARN**  
The Amazon Resource Name \(ARN\) for the image with the finding specified in the **Finding ID** column\.

**Pipeline**  
The pipeline that built the image specified in the **Image ARN** column\.

**Description**  
A short description of the finding\.

**Inspector score**  
The score that Amazon Inspector assigned for the CVE finding\.

**Remediation**  
Links to details about the recommended course of action to remediate the finding\.

**Published date**  
The date and time when this vulnerability was first added to the vendor's database\.