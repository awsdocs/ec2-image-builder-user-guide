# EC2 Image Builder User Guide

-----
*****Copyright &copy; 2020 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What is EC2 Image Builder?](what-is-image-builder.md)
+ [How EC2 Image Builder works](how-image-builder-works.md)
+ [Get started with EC2 Image Builder](getting-started-image-builder.md)
   + [Prerequisites](image-builder-setting-up.md)
   + [Access EC2 Image Builder](image-builder-accessing-prereq.md)
   + [Create an automated build pipeline using the EC2 Image Builder console wizard](image-builder-image-deployment-console.md)
+ [Manage EC2 Image Builder image pipelines using the console](managing-image-builder-console.md)
   + [Edit an existing Image Builder pipeline](image-builder-configuration-details.md)
   + [Create components](image-builder-create-component.md)
   + [Manage EC2 Image Builder image recipes using the console](image-builder-recipes.md)
   + [Delete resources](image-builder-delete-pipeline.md)
+ [Manage Image Builder image pipelines using the AWS CLI](managing-image-builder-cli.md)
+ [EC2 Image Builder component manager](image-builder-component-manager.md)
   + [Use documents in EC2 Image Builder](image-builder-application-documents.md)
   + [Develop Image Builder components locally](image-builder-component-manager-local.md)
   + [Use looping constructs in the AWSTOE application](image-builder-looping-constructs.md)
   + [Define and reference variables in the AWSTOE application](image-builder-component-manager-user-defined-variables.md)
   + [Component manager supported action modules](image-builder-action-modules.md)
   + [Verify the signature of the AWSTOE installation download](awstoe-verify-sig.md)
   + [EC2 Image Builder STIG components](image-builder-stig.md)
+ [Share EC2 Image Builder resources](image-builder-resource-sharing.md)
+ [Use cron expressions in EC2 Image Builder](image-builder-cron.md)
+ [Security in EC2 Image Builder](image-builder-security.md)
   + [EC2 Image Builder and interface VPC endpoints (AWS PrivateLink)](vpc-interface-endpoints.md)
   + [Data protection in EC2 Image Builder](data-protection.md)
   + [Identity and Access Management for EC2 Image Builder](security-iam.md)
      + [How EC2 Image Builder works with IAM](security_iam_service-with-iam.md)
      + [EC2 Image Builder identity-based policy examples](security_iam_id-based-policy-examples.md)
      + [EC2 Image Builder resource-based policy examples](security_iam_resource-based-policy-examples.md)
      + [Using service-linked roles for EC2 Image Builder](image-builder-service-linked-role.md)
      + [Troubleshooting EC2 Image Builder identity and access](security_iam_troubleshoot.md)
   + [Compliance validation for EC2 Image Builder](compliance.md)
   + [Resilience in EC2 Image Builder](disaster-recovery-resiliency.md)
   + [Infrastructure security in EC2 Image Builder](infrastructure-security.md)
   + [Patch Management in EC2 Image Builder](vulnerability-analysis-and-management.md)
   + [Security best practices for EC2 Image Builder](security-best-practices.md)
+ [Troubleshoot EC2 Image Builder](image-builder-troubleshooting.md)
+ [Document History for EC2 Image Builder User Guide](doc-history.md)
+ [AWS glossary](glossary.md)