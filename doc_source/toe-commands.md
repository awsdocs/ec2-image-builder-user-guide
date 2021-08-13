# AWSTOE command reference<a name="toe-commands"></a>

Use the following AWS CLI arguments to perform operations with the component management application\.


| CLI action/flag | Shorthand argument notation | Requirement status | Description | 
| --- | --- | --- | --- | 
|  `validate`  |  N/A  |  Not required  | Validates document syntax and checks for multiple component management application documents\. | 
|  `run`  |  N/A  |  Not required  | Runs multiple component management application documents\. | 
|  `--documents`  |  `-d`  |  Required  | The paths of the component management application documents as a comma\-separated list\. Valid options include local file paths, Amazon S3 URIs, or Image Builder component build version ARNs\. | 
|  `--parameters`  |  `N/A`  |  Not required  | Parameters are mutable variables that are defined in the component document, with settings that the calling application can provide at runtime\. | 
|  `--log-s3-bucket-name`  |  `-b`  |  Not required  | The Amazon S3 bucket name to use to upload the execution logs \(recommended\)\. | 
|  `--log-s3-key-prefix`  |  `-k`  |  Not required  | An Amazon S3 object key prefix to use to upload logs \(recommended\)\. | 
|  `--log-s3-bucket-owner`  |  `N/A`  |  Not required  | Account ID of Amazon S3 bucket for storing logs \(recommended\)\. | 
|  `--log-directory`  |  `-l`  |  Not required  | All log files from running the application are generated in this path under directory TOE\_<DATETIME>\_<EXECUTIONID>\. If not provided, "\." \(current working directory\) is used\. | 
|  `--phases`  |  `-p`  |  Not required  | The names of the phases to be run from specified documents\. | 
|  `--execution-id`  |  `-i`  |  Not required  | This option takes id in a string that serves as the unique ID when AWSTOE runs\. This unique ID is used to identify log files and detailed outputs about current AWSTOE execution\. If this is not passed in, AWSTOE auto\-generates a GUID\. | 
|  `--version`  |  `-v`  |  Not required  | Displays the application version\. | 
| \-\-cw\-log\-group | N/A |  Not required  | CloudWatch Log group name\. | 
| \-\-cw\-log\-stream | N/A |  Not required  | CloudWatch Log stream name, where console\.log file is streamed\. | 
| \-\-cw\-log\-region | N/A |  Not required  | CloudWatch Logs Region\. | 
| \-\-cw\-ignore\-failures | N/A |  Not required  | Ignore CloudWatch logging failures\. | 
|  `--help`  |  `-h`  |  Not required  | Prints a manual about how to use the component management application options correctly\. Provided by the Golang flag library, by default\. | 
|  `--trace`  |  `-t`  |  Not required  | Enables application logs to be logged to the console\. | 