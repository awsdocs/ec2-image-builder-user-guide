# Compliance products for your Image Builder images<a name="integ-compliance-products"></a>

With constantly evolving security standards, it can be a challenge to maintain compliance and safeguard your organization from cyber threats\. To help ensure that your custom images are compliant, and stay that way through automatic updates when publishers release new versions, Image Builder integrates with AWS Marketplace compliance products and AWSTOE components\.

Image Builder integrates with the following compliance products:
+ 

**Center for Internet Security \(CIS\) Benchmarks hardening**  
You can use CIS Hardened Images and the related CIS hardening components to build custom images that comply with the latest CIS Benchmarks Level 1 guidelines\. CIS Hardened Images are available in AWS Marketplace\. To learn more about how to set up and use CIS Hardened Images and hardening components, see the [Quick Start Guides](https://cisecurity.atlassian.net/servicedesk/customer/portal/15/article/2671771774) in the CIS website support portal\.
**Note**  
When you subscribe to a CIS Hardened Image, you also get access to the associated build component that runs a script to enforce CIS Benchmark Level 1 guidelines for your configuration\. For more information, see [CIS hardening components](toe-cis.md)\.
+ 

**Security Technical Implementation Guides \(STIG\)**  
For STIG compliance, use can use Amazon\-managed AWS Task Orchestrator and Executor \(AWSTOE\) STIG components in your Image Builder recipes\. STIG components scan your build instance for misconfigurations and run a remediation script to correct issues that they find\. We can't guarantee STIG compliance for the images that you build with Image Builder\. You must work with your organization's compliance team to verify that your final image is compliant\. For a complete list of AWSTOE STIG components that you can use in your Image Builder recipes, see [AWS Task Orchestrator and Executor STIG components](toe-stig.md)\.