# Update container image pipelines \(AWS CLI\)<a name="cli-update-container-pipeline"></a>

You can update a container image pipeline using a JSON file as input to the imagebuilder update\-image\-pipeline command in the AWS CLI\. To configure the JSON file, you must have Amazon Resource Names \(ARNs\) to reference the following existing resources:
+ Image pipeline to update
+ Container recipe
+ Infrastructure configuration
+ Distribution settings

Follow these steps to update a container image pipeline using the imagebuilder update\-image\-pipeline command in the AWS CLI:

**Note**  
UpdateImagePipeline does not support selective updates for the pipeline\. You must specify all of the required properties in the update request, not just the properties that have changed\.

1. 

**Create a CLI input JSON file**

   Use your favorite file editing tool to create a JSON file with the following keys, plus values that are valid for your environment\. This example uses a file named `create-component.json`:

   ```
       {
       "imagePipelineArn": "arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline",
       "containerRecipeArn": "arn:aws:imagebuilder:us-west-2:123456789012:container-recipe/my-example-recipe/2020.12.08",
       "infrastructureConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:infrastructure-configuration/my-example-infrastructure-configuration",
       "distributionConfigurationArn": "arn:aws:imagebuilder:us-west-2:123456789012:distribution-configuration/my-example-distribution-configuration",
       "imageTestsConfiguration": {
           "imageTestsEnabled": true,
           "timeoutMinutes": 120
       },
       "schedule": {
           "scheduleExpression": "cron(0 0 * * MON *)",
           "pipelineExecutionStartCondition": "EXPRESSION_MATCH_AND_DEPENDENCY_UPDATES_AVAILABLE"
       },
       "status": "DISABLED"
   }
   ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

1. Run the following command, using the file you created as input\.

   ```
   aws imagebuilder update-image-pipeline --cli-input-json file://update-image-pipeline.json
   ```