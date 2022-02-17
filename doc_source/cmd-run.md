# awstoe run command<a name="cmd-run"></a>

This command runs the YAML component document scripts in the order in which they are included in the configuration file specified by the `--config` parameter, or the list of component documents specified by the `--documents` parameter\.

**Note**  
You must specify exactly one of the following parameters, never both:  
\-\-config  
\-\-documents

## Syntax<a name="run-syntax"></a>

```
awstoe run [--config <file path>] [--cw-ignore-failures <?>] 
      [--cw-log-group <?>] [--cw-log-region us-west-2] [--cw-log-stream <?>] 
      [--document-s3-bucket-owner <owner>] [--documents <file path,file path,...>] 
      [--execution-id <?>] [--log-directory <file path>] 
      [--log-s3-bucket-name <name>] [--log-s3-bucket-owner <owner>] 
      [--log-s3-key-prefix <?>] [--parameters name1=value1,name2=value2...] 
      [--phases <phase name>] [--state-directory <directory path>] [--version <?>] 
      [--help] [--trace]
```

## Parameters and options<a name="run-parameters"></a>Parameters

**\-\-config *`./config-example.json`***  
Short form: \-c *`./config-example.json`*  
The configuration file *\(conditional\)*\. This parameter contains the file location for the JSON file that contains configuration settings for the components this command is running\. If you specify run command settings in a configuration file, you must not specify the `--documents` parameter\. For more information about input configuration, see [Configure input for the AWSTOE run command](toe-run-config-input.md)\.  
Valid locations include:  
+ A local file path \(*`./config-example.json`*\)
+ An S3 URI \(`s3://bucket/key`\)

**\-\-cw\-ignore\-failures**  
Short form: N/A  
Ignore logging failures from the CloudWatch Logs\.

**\-\-cw\-log\-group**  
Short form: N/A  
The `LogGroup` name for the CloudWatch Logs\.

**\-\-cw\-log\-region**  
Short form: N/A  
The AWS Region that applies to the CloudWatch Logs\.

**\-\-cw\-log\-stream**  
Short form: N/A  
The `LogStream` name for the CloudWatch Logs, that directs AWSTOE where to stream the `console.log` file\.

**\-\-document\-s3\-bucket\-owner**  
Short form: N/A  
The account ID of the bucket owner for S3 URI\-based documents\.

**\-\-documents *`./doc-1.yaml`,`./doc-n.yaml`***  
Short form: \-d *`./doc-1.yaml`*,*`./doc-n`*  
The component documents *\(conditional\)*\. This parameter contains a comma\-separated list of file locations for the YAML component documents to run\. If you specify YAML documents for the run command using the `--documents` parameter, you must not specify the `--config` parameter\.  
Valid locations include:  
+ local file paths \(*\./component\-doc\-example\.yaml*\)\.
+ S3 URIs \(`s3://bucket/key`\)\.
+ Image Builder component build version ARNs \(arn:aws:imagebuilder:us\-west\-*2:123456789012*:component/*my\-example\-component*/2021\.12\.02/1\)\.
There are no spaces between items in the list, only commas\.

**\-\-execution\-id**  
Short form: \-i  
This is the unique ID that applies to the execution of the current run command\. This ID is included in output and log file names, to uniquely identify those files, and link them to the current command execution\. If this setting is left out, AWSTOE generates a GUID\.

**\-\-log\-directory**  
Short form: \-l  
The destination directory where AWSTOE stores all of the log files from this command execution\. By default, this directory is located inside of the following parent directory: `TOE_<DATETIME>_<EXECUTIONID>`\. If you do not specify the log directory, AWSTOE uses the current working directory \(`.`\)\.

**\-\-log\-s3\-bucket\-name**  
Short form: \-b  
If component logs are stored in Amazon S3 \(recommended\), AWSTOE uploads the component application logs to the S3 bucket named in this parameter\.

**\-\-log\-s3\-bucket\-owner**  
Short form: N/A  
If component logs are stored in Amazon S3 \(recommended\), this is the owner account ID for the bucket where AWSTOE writes the log files\.

**\-\-log\-s3\-key\-prefix**  
Short form: \-k  
If component logs are stored in Amazon S3 \(recommended\), this is the S3 object key prefix for the log location in the bucket\.

**\-\-parameters *name1*=*value1*,*name2*=*value2*\.\.\.**  
Short form: N/A  
Parameters are mutable variables that are defined in the component document, with settings that the calling application can provide at runtime\.

**\-\-phases**  
Short form: \-p  
A comma\-separated list that specifies which phases to run from the YAML component documents\. If a component document includes additional phases, those will not run\.

**\-\-state\-directory**  
Short form: \-s  
The file path where state tracking files are stored\.

**\-\-version**  
Short form: \-v  
Specifies the component application version\.Options

**\-\-help**  
Short form: \-h  
Displays a help manual for using the component management application options\.

**\-\-trace**  
Short form: \-t  
Enables verbose logging to the console\.