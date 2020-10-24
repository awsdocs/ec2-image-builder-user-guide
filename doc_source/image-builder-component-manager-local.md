# Develop Image Builder components locally<a name="image-builder-component-manager-local"></a>

The component management \(AWSTOE\) application is a standalone application that EC2 Image Builder uses to run Image Builder components during the image build workflow\. You can use this application to develop new component definitions or to troubleshoot existing component definitions by running them locally\.

**Topics**
+ [Get started with the AWSTOE application](#image-builder-component-manager-commands)
+ [Set AWS credentials for AWSTOE to connect to other AWS services](#image-builder-component-manager-credentials)
+ [Component management application CLI arguments](#image-builder-component-manager-cli)

## Get started with the AWSTOE application<a name="image-builder-component-manager-commands"></a>

The following steps show you the process of installing the AWSTOE application, developing Image Builder components locally, validating component syntax, and running components locally\.

1. 

**Install the AWSTOE application**

   To install AWSTOE, choose the download link for your architecture and platform\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/image-builder-component-manager-local.html)

   [Verify the signature](awstoe-verify-sig.md) of the download\.

1. 

**Develop Image Builder component documents locally**

   Image Builder components are authored with plaintext YAML documents\. For more information about document syntax, see [Use documents in EC2 Image Builder](image-builder-application-documents.md)\.

   The following are example "hello world" component documents that you can use to develop your documents locally\.

    `hello-world-windows.yml`\.

   ```
   name: Hello World
   description: This is hello world testing document for Windows.
   schemaVersion: 1.0
   phases:
     - name: build
       steps:
         - name: HelloWorldStep
           action: ExecutePowerShell
           inputs:
             commands:
               - Write-Host 'Hello World from the build phase.'
     - name: validate
       steps:
         - name: HelloWorldStep
           action: ExecutePowerShell
           inputs:
             commands:
               - Write-Host 'Hello World from the validate phase.'
     - name: test
       steps:
         - name: HelloWorldStep
           action: ExecutePowerShell
           inputs:
             commands:
               - Write-Host 'Hello World from the test phase.'
   ```

    `hello-world-linux.yml`\.

   ```
   name: Hello World
   description: This is hello world testing document for Linux.
   schemaVersion: 1.0
   phases:
     - name: build
       steps:
         - name: HelloWorldStep
           action: ExecuteBash
           inputs:
             commands:
               - echo 'Hello World from the build phase.'
     - name: validate
       steps:
         - name: HelloWorldStep
           action: ExecuteBash
           inputs:
             commands:
               - echo 'Hello World from the validate phase.'
     - name: test
       steps:
         - name: HelloWorldStep
           action: ExecuteBash
           inputs:
             commands:
               - echo 'Hello World from the test phase.'
   ```

1. 

**Validate Image Builder components**

   You can validate the syntax of Image Builder components locally with the AWSTOE application\. The following examples show the AWSTOE application `validate` command to validate the syntax of a component without running it\. 
**Note**  
The AWSTOE application can validate only the component syntax for the current operating system\. For example, when running `awstoe.exe` on Windows, you cannot validate the syntax for a Linux document that uses the `ExecuteBash` action module\.

   Windows

   ```
   C:\> awstoe.exe validate --documents C:\Users\user\Documents\hello-world.yml
   ```

   Linux

   ```
   $ awstoe validate --documents /home/user/hello-world.yml
   ```

1. 

**Execute Image Builder components**

   The AWSTOE application can run one or more phases of specified documents using the `--phases` command line argument\. Supported values for `--phases` are `build`, `validate`, and `test`\. Multiple phase values can be entered as comma separated values\.

   When you provide a list of phases, the AWSTOE application sequentially runs the specified phases of each document\. For example, AWSTOE runs the `build` and `validate` phases of `document1.yaml`, followed by the `build` and `validate` phases of `document2.yaml`\.

   If a list of phases is not provided, the AWSTOE application runs all phases in the order listed in the YAML document\.

   To run specific phases in single or multiple documents, use the following commands\.

   Single phase

   ```
   awstoe run --documents hello-world.yml --phases build
   ```

   Multiple phases

   ```
   awstoe run --documents hello-world.yml --phases build,test
   ```

**Document run**  
Run all phases in a single document

   ```
   awstoe run --documents documentName.yaml
   ```

   Run all phases in multiple documents

   ```
   awstoe run --documents documentName1.yaml,documentName2.yaml
   ```

   Enter Amazon S3 information to upload AWSTOE logs from a user\-defined local path

   ```
   awstoe run --documents documentName.yaml --log-s3-bucket-name <S3Bucket> --log-s3-key-prefix <S3KeyPrefix> --log-directory <local_path>
   ```

   Run all phases in a single document, and display all logs on the console

   ```
   awstoe run --documents documentName.yaml --trace
   ```

   Example command

   ```
   awstoe run --documents s3://bucket/key/doc.yaml --phases build,validate
   ```

   Run document with unique ID

   ```
   awstoe run --documents <documentName>.yaml --execution-id <user provided id> --phases <comma separated list of phases>
   ```

   Get help with AWSTOE

   ```
   awstoe --help
   ```

## Set AWS credentials for AWSTOE to connect to other AWS services<a name="image-builder-component-manager-credentials"></a>

 AWSTOE requires AWS credentials to connect to other AWS services, such as Amazon S3 and Amazon CloudWatch, when running tasks, such as: 
+ Downloading AWSTOE documents from a user\-provided Amazon S3 path\.
+ Executing `S3Download` or `S3Upload` action modules\.
+ Streaming logs to CloudWatch, when enabled\.

If you are running AWSTOE on an EC2 instance, then running AWSTOE uses the same permissions as the IAM role attached to the EC2 instance\.

For more information about IAM roles for EC2, see [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)\.

The following examples show how to set AWS credentials using the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables\. 

To set these variables on Linux, OS X, or Unix, use `export`\.

```
$ export AWS_ACCESS_KEY_ID=your_access_key_id
```

```
$ export AWS_SECRET_ACCESS_KEY=your_secret_access_key
```

To set these variables on Windows using PowerShell, use `$env`\.

```
C:\> $env:AWS_ACCESS_KEY_ID=your_access_key_id
```

```
C:\> $env:AWS_SECRET_ACCESS_KEY=your_secret_access_key
```

To set these variables on Windows using the command prompt, use `set`\.

```
C:\> set AWS_ACCESS_KEY_ID=your_access_key_id
```

```
C:\> set AWS_SECRET_ACCESS_KEY=your_secret_access_key
```

## Component management application CLI arguments<a name="image-builder-component-manager-cli"></a>

Use the following AWS CLI arguments to perform operations with the component management application\.


| CLI action/flag | Shorthand argument notation | Requirement status | Description | 
| --- | --- | --- | --- | 
|  `validate`  |  N/A  |  Not required  | Validates document syntax and checks for multiple component management application documents\. | 
|  `run`  |  N/A  |  Not required  | Runs multiple component management application documents\. | 
|  `--help`  |  `-h`  |  Not required  | Prints a manual about how to use the component management application options correctly\. Provided by the Golang flag library, by default\. | 
|  `--documents`  |  `-d`  |  Required  | The paths of the component management application documents as a comma\-separated list\. Valid options include local file paths, S3 URIs, or EC2 Image Builder component build version ARNs\. | 
|  `--log-s3-bucket-name`  |  `-b`  |  Not required  | The S3 bucket name to use to upload the execution logs\. | 
|  `--log-s3-key-prefix`  |  `-k`  |  Not required  | An S3 object key prefix to use to upload logs\. | 
|  `--log-directory`  |  `-l`  |  Not required  | All log files from running the application are generated in this path under directory TOE\_<DATETIME>\_<EXECUTIONID>\. If not provided, "\." \(current working directory\) is used\. | 
|  `--phases`  |  `-p`  |  Not required  | The names of the phases to be run from specified documents\. | 
|  `--execution-id`  |  `-i`  |  Not required  | This option takes id in a string which serves as the unique ID when AWSTOE is run\. This unique ID is used to identify log files and detailed outputs pertaining to current AWSTOE execution\. If this is not passed, AWSTOE auto\-generates it as GUID\. | 
|  `--trace`  |  `-t`  |  Not required  | Enables application logs to be logged to the console\. | 
|  `--version`  |  `-v`  |  Not required  | Displays the application version\. | 
| \-\-cw\-log\-group | \-v |  Not required  | CloudWatch Log group name\. | 
| \-\-cw\-log\-stream | \-v |  Not required  | CloudWatch Log stream name, where console\.log file is streamed\. | 
| \-\-cw\-log\-region | \-v |  Not required  | CloudWatch Logs Region\. | 
| \-\-cw\-ignore\-failures | \-v |  Not required  | Ignore CloudWatch logging failures\. | 