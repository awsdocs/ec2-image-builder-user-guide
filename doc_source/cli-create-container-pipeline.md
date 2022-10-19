# Create a container image pipeline \(AWS CLI\)<a name="cli-create-container-pipeline"></a>

You can create a container image pipeline using a JSON file as input to the [create\-image\-pipeline](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/imagebuilder/create-image-pipeline.html) command in the AWS CLI\.

How often your pipeline builds a new image to incorporate any pending updates from your base image and components depends on the `schedule` that you have configured\. A `schedule` has the following attributes:
+ `scheduleExpression` – Sets the schedule for when your pipeline runs to evaluate the `pipelineExecutionStartCondition` and determine if it should start a build\. The schedule is configured with cron expressions\. For more information on how to format a cron expression in Image Builder, see [Use cron expressions in EC2 Image Builder](cron-expressions.md)\.
+ `pipelineExecutionStartCondition` – Determines if your pipeline should start the build\. Valid values include:
  + `EXPRESSION_MATCH_ONLY` – your pipeline will build a new image every time the cron expression matches the current time\. 
  + `EXPRESSION_MATCH_AND_DEPENDENCY_UPDATES_AVAILABLE` – your pipeline will not start a new image build unless there are pending changes to your base image or components\.

When you run the create\-image\-pipeline command in the AWS CLI, many of the configuration resources are optional\. However, some of the resources have conditional requirements, depending on what type of image the pipeline creates\. The following resources are required for container image pipelines:
+ Container recipe ARN
+ Infrastructure configuration ARN

If you do not include a distribution configuration resource when you run the create\-image\-pipeline command, the output image is stored in the ECR repository that you specify as the target repository in your container recipe in the Region where you run the command\. If you include a distribution configuration resource for your pipeline, the target repository that you have specified for the first Region in the distribution is used\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with the following keys, plus values that are valid for your environment\. This example uses a file named `create-image-pipeline.json`:

   ```
   {
       "name": "MyWindows2019Pipeline",
       "description": "Builds Windows 2019 Images",
       "enhancedImageMetadataEnabled": true,
       "containerRecipeArn": "arn:aws:imagebuilder:us-west-2:123456789012:container-recipe/my-example-recipe/2020.12.03",
       "infrastructureConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration",
       "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration",
       "imageTestsConfiguration": {
           "imageTestsEnabled": true,
           "timeoutMinutes": 60
       },
       "schedule": {
           "scheduleExpression": "cron(0 0 * * SUN *)",
           "pipelineExecutionStartCondition": "EXPRESSION_MATCH_AND_DEPENDENCY_UPDATES_AVAILABLE"
       },
       "status": "ENABLED"
   }
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

1. Run the following command, using the file you created as input\.

   ```
   aws imagebuilder create-image-pipeline --cli-input-json file://create-image-pipeline.json
   ```