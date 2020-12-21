# Manage EC2 Image Builder pipelines using the console<a name="manage-pipelines"></a>

Image Builder image pipelines provide an automation framework for creating and maintaining custom AMIs and container images\. Pipelines deliver the following functionality:
+ Assemble the source image, components for building and testing, infrastructure configuration, and distribution settings\.
+ Facilitate scheduling for automated maintenance processes using the `Schedule builder` in the console wizard, or entering cron expressions for recurring updates to your images\.
+ Enable change detection for the source image and components, to automatically skip scheduled builds when there are no changes\.

**Topics**
+ [List and view pipeline details](pipeline-details.md)
+ [Create and update container image pipelines](container-image-pipelines.md)
+ [Create and update AMI image pipelines](ami-image-pipelines.md)
+ [Use cron expressions in EC2 Image Builder](cron-expressions.md)