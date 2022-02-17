# Configure input for the AWSTOE run command<a name="toe-run-config-input"></a>

To streamline command line input for your AWSTOE run command, you can include settings for command parameters and options in a JSON format input configuration file with a `.json` file extension\. AWSTOE can read your file from one of the following locations:
+ A local file path \(*\./config\.json*\)\.
+ An S3 bucket \(*s3://<bucket\-path>/<bucket\-name>/config\.json*\)\.

When you enter the run command, you can specify the input configuration file using the \-\-config parameter\. For example:

```
awstoe run --config <file-path>/config.json
```

**Input configuration file**  
The input configuration JSON file includes key\-value pairs for all of the settings that you can provide directly through run command parameters and options\. If you specify a setting in both the input configuration file and the run command, as a parameter or option, the following rules of precedence apply:

**Rules of precedence**

1. A setting that is supplied directly to the run command in the AWS CLI, via a parameter or option, overrides any value that is defined in the input configuration file for the same setting\.

1. A setting in the input configuration file overrides a component default value\.

1. If no other settings are passed into the component document, it can apply a default value, if one exists\.

There are two exceptions to this rule – documents and parameters\. These settings work differently in the input configuration and as command parameters\. If you use the input configuration file, you must not specify these parameters directly to the run command\. Doing so will generate an error\.

**Component settings**  
The input configuration file contains the following settings\. To streamline your file, you can leave out any optional settings that aren't needed\. All settings are optional unless otherwise noted\.
+ **cwIgnoreFailures** \(Boolean\) – Ignore logging failures from the CloudWatch Logs\.
+ **cwLogGroup** \(String\) – The `LogGroup` name for the CloudWatch Logs\.
+ **cwLogRegion** \(String\) – The AWS Region that applies to the CloudWatch Logs\.
+ **cwLogStream** \(String\) – The `LogStream` name for the CloudWatch Logs, that directs AWSTOE where to stream the `console.log` file\.
+ **documentS3BucketOwner** \(String\) – The account ID of the bucket owner for S3 URI\-based documents\.
+ **documents** \(array of objects, required\) – An array of JSON objects representing the YAML component documents that the AWSTOE run command is running\. At least one component document must be specified\.

  Each object consists of the following fields:
  + **path** \(String, required\) – The file location of the YAML component document\. This must be one of the following:
    + A local file path \(*\./component\-doc\-example\.yaml*\)\.
    + An S3 URI \(`s3://bucket/key`\)\.
    + An Image Builder component build version ARN \(arn:aws:imagebuilder:us\-west\-*2:123456789012*:component/*my\-example\-component*/2021\.12\.02/1\)\.
  + **parameters** \(array of objects\) – An array of key\-value pair objects, each representing a component\-specific parameter that the run command passes in when it runs the component document\. Parameters are optional for components\. The component document may or may not have parameters defined\.

    Each object consists of the following fields:
    + **name** \(String, required\) – The name of the component parameter\.
    + **value** \(String, required\) – The value to pass in to the component document for the named parameter\.

    To learn more about component parameters, see the **Parameters** section in the [Define and reference variables in AWSTOE](toe-user-defined-variables.md) page\.
+ **executonId** \(String\) – This is the unique ID that applies to the execution of the current run command\. This ID is included in output and log file names, to uniquely identify those files, and link them to the current command execution\. If this setting is left out, AWSTOE generates a GUID\.
+ **logDirectory** \(String\) – The destination directory where AWSTOE stores all of the log files from this command execution\. By default, this directory is located inside of the following parent directory: `TOE_<DATETIME>_<EXECUTIONID>`\. If you do not specify the log directory, AWSTOE uses the current working directory \(`.`\)\.
+ **logS3BucketName** \(String\) – If component logs are stored in Amazon S3 \(recommended\), AWSTOE uploads the component application logs to the S3 bucket named in this parameter\.
+ **logS3BucketOwner** \(String\) – If component logs are stored in Amazon S3 \(recommended\), this is the owner account ID for the bucket where AWSTOE writes the log files\.
+ **logS3KeyPrefix** \(String\) – If component logs are stored in Amazon S3 \(recommended\), this is the S3 object key prefix for the log location in the bucket\.
+ **parameters** \(array of objects\) – An array of key\-value pair objects that represent parameters that apply globally to all of the components that are included in the current run command execution\.
  + **name** \(String, required\) – The name of the global parameter\.
  + **value** \(String, required\) – The value to pass in to all of the component documents for the named parameter\.
+ **phases** \(String\) – A comma\-separated list that specifies which phases to run from the YAML component documents\. If a component document includes additional phases, those will not run\.
+ **stateDirectory** \(String\) – The file path where state tracking files are stored\.
+ **trace** \(Boolean\) – Enables verbose logging to the console\.

**Examples**  
The following example shows an input configuration file that runs the `build` and `test` phases for two component documents: `sampledoc.yaml`, and `conversation-intro.yaml`\. Each component document has a parameter that applies only to itself, and both use one shared parameter\. The `project` parameter applies to both component documents\.

```
{
   "documents": [
     {
       "path": "<file path>/awstoe/sampledoc.yaml>",
       "parameters": [
         {
           "name": "dayofweek",
           "value": "Monday"
         }
       ]
     },
     {
       "path": "<file path>/awstoe/conversation-intro.yaml>",
       "parameters": [
         {
           "name": "greeting",
           "value": "Hello, HAL."
         }
       ]
     }
   ],
   "phases": "build,test",
   "parameters": [
     {
       "name": "project",
       "value": "examples"
     }
   ],
   "cwLogGroup": "<log_group_name>",
   "cwLogStream": "<log_stream_name>",
   "documentS3BucketOwner": "<owner_aws_account_number>",
   "executionId": "<id_number>",
   "logDirectory": "<local_directory_path>",
   "logS3BucketName": "<bucket_name_for_log_files>",
   "logS3KeyPrefix": "<key_prefix_for_log_files>",
   "logS3BucketOwner": "<owner_aws_account_number>"
 }
```