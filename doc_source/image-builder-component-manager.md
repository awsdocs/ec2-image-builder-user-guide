# EC2 Image Builder Component Manager<a name="image-builder-component-manager"></a>

Image Builder uses a component management application to orchestrate complex workflows, modify system configurations, and test your systems without writing code\. This application uses a declarative document schema\. Because it is a standalone application, it does not require additional server setup\. It can run on any cloud infrastructure and on premises\. 

EC2 Image Builder uses this application to perform all on\-instance activities, such as build, validation, and test\. You define a document that describes how to build, validate, and test your image\. EC2 Image Builder sends the component to your instance and the application interprets and applies it to your instance by executing the defined phases, steps, and actions\. When complete, the application sends a summary to EC2 Image Builder\. It also sends detailed execution outputs to Amazon S3 if you specified an S3 bucket in your pipeline configuration\. EC2 Image Builder then cleans up the application and removes it from the instance using [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements)\. 
+ **Build phase**\. The image is modified\. For example, you can configure your image to install an application or to modify the operating system firewall settings\. The validate phase is executed as part of the build phase, prior to the creation of the image\. 
+ **Test phase**\. Tests are executed against your new image after it is created\.

EC2 Image Builder uses the component management application as follows\.

1. You define an EC2 Image Builder component, which is a document that describes how to build, validate, and test your image\.

1. EC2 Image Builder dispatches the work to be performed by copying the document and application to your instance\.

1. The application executes the phases, steps, and actions defined in the document\. 

**Topics**
+ [Using Documents in EC2 Image Builder](image-builder-application-documents.md)
+ [Component Manager Supported Action Modules](image-builder-action-modules.md)
+ [EC2 Image Builder STIG Components](image-builder-stig.md)