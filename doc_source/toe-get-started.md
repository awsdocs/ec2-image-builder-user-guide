# Get started with AWSTOE<a name="toe-get-started"></a>

The AWS Task Orchestrator and Executor \(AWSTOE\) application is a standalone application that creates, validates, and runs commands within a component definition framework\. AWS services can use AWSTOE to orchestrate workflows, install software, modify system configurations, and test image builds\.

Follow these steps to install the AWSTOE application and use it for the first time\.

**Topics**
+ [Verify the signature of the AWSTOE installation download](awstoe-verify-sig.md)
+ [Step 1: Install AWSTOE](#toe-start-install)
+ [Step 2: Set AWS credentials](#toe-start-credentials)
+ [Step 3: Develop component documents locally](#toe-start-develop)
+ [Step 4: Validate AWSTOE components](#toe-start-validate)
+ [Step 5: Run AWSTOE components](#toe-start-run)

## Step 1: Install AWSTOE<a name="toe-start-install"></a>

To develop components locally, download and install the AWSTOE application\.

1. 

**Download the AWSTOE application**

   To install AWSTOE, choose the appropriate download link for your architecture and platform\. For the full list of application download links, see [AWSTOE downloads](toe-component-manager.md#toe-downloads)

1. 

**Verify the signature**

   The steps for verifying your download depend on the server platform where you run the AWSTOE application after you install it\. To verify your download on a Linux server, see [Verify the signature on Linux](awstoe-verify-sig.md#awstoe-verify-sig-linux)\. To verify your download on a Windows server, see [Verify the signature on Windows](awstoe-verify-sig.md#awstoe-verify-sig-win)\.

1. 

**Install AWSTOE**

   Install the download using the method that applies to your download location\.

## Step 2: Set AWS credentials<a name="toe-start-credentials"></a>

 AWSTOE requires AWS credentials to connect to other AWS services, such as Amazon S3 and Amazon CloudWatch, when running tasks, such as: 
+ Downloading AWSTOE documents from a user\-provided Amazon S3 path\.
+ Running `S3Download` or `S3Upload` action modules\.
+ Streaming logs to CloudWatch, when enabled\.

If you are running AWSTOE on an EC2 instance, then running AWSTOE uses the same permissions as the IAM role attached to the EC2 instance\.

For more information about IAM roles for EC2, see [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)\.

The following examples show how to set AWS credentials using the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables\. 

To set these variables on Linux, macOS, or Unix, use `export`\.

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

## Step 3: Develop component documents locally<a name="toe-start-develop"></a>

AWSTOE components are authored with plaintext YAML documents\. For more information about document syntax, see [Use component documents in AWSTOE](toe-use-documents.md)\.

The following are example Hello World component documents that you can use to develop your documents locally\.

`hello-world-windows.yml`\.

```
name: Hello World
description: This is Hello World testing document for Windows.
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

## Step 4: Validate AWSTOE components<a name="toe-start-validate"></a>

You can validate the syntax of AWSTOE components locally with the AWSTOE application\. The following examples show the AWSTOE application `validate` command to validate the syntax of a component without running it\. 

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

## Step 5: Run AWSTOE components<a name="toe-start-run"></a>

The AWSTOE application can run one or more phases of specified documents using the `--phases` command line argument\. Supported values for `--phases` are `build`, `validate`, and `test`\. Multiple phase values can be entered as comma separated values\.

When you provide a list of phases, the AWSTOE application sequentially runs the specified phases of each document\. For example, AWSTOE runs the `build` and `validate` phases of `document1.yaml`, followed by the `build` and `validate` phases of `document2.yaml`\.

To ensure that your logs are stored securely and retained for troubleshooting, we recommend configuring log storage in Amazon S3\. In Image Builder, the Amazon S3 location for publishing logs is specified in the infrastructure configuration\. For more information about infrastructure configuration, see [Manage EC2 Image Builder infrastructure configuration](manage-infra-config.md)

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

Enter Amazon S3 information to upload AWSTOE logs from a user\-defined local path \(recommended\)

```
awstoe run --documents documentName.yaml --log-s3-bucket-name <S3Bucket> --log-s3-key-prefix <S3KeyPrefix> --log-s3-bucket-owner <S3BucketOwner> --log-directory <local_path>
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