# Component Manager Supported Action Modules<a name="image-builder-action-modules"></a>

This section contains the list of action modules that are supported by the component management application used by EC2 Image Builder to configure the instance that builds your image\. Also included are the corresponding functionality details and input/output values of the action modules\.

**Note**  
All action modules are executed using the same account as the SSM agent, which is `root` on Linux and `NT Authority\SYSTEM` on Windows\.

**Topics**
+ [ExecuteBinary](#image-builder-action-modules-executebinary)
+ [ExecuteBash](#image-builder-action-modules-executebash)
+ [ExecutePowerShell](#image-builder-action-modules-executepowershell)
+ [Reboot](#image-builder-action-modules-reboot)
+ [UpdateOS](#image-builder-action-modules-updateos)
+ [S3Upload](#image-builder-action-modules-s3upload)
+ [S3Download](#image-builder-action-modules-s3download)
+ [SetRegistry](#image-builder-action-modules-setregistry)

## ExecuteBinary<a name="image-builder-action-modules-executebinary"></a>

The **ExecuteBinary** module allows you to execute binary files with a list of command\-line arguments\.

The **ExecuteBinary** module handles system restarts if the execution exits with an exit code of `194` \(Linux\) or `3010` \(Windows\)\. When triggered, the application performs one of the following actions:
+ The application hands the exit code to the caller if it is executed by the SSM Agent\. The SSM Agent handles the system reboot and re\-invokes the execution as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.
+ The application saves the current `executionstate`, configures a restart trigger to re\-execute the application, and reboots the system\.

After system restart, the application executes the same step that triggered the restart\. If you require this functionality, you must write idempotent scripts that can handle multiple invocations of the same shell command\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| path | The path to the binary file for execution\. | String | Yes | 
| arguments | Contains a list of command\-line arguments to use when executing the binary\. | String List | No | 

**Input Example**

```
name: "InstallDotnet"
action: ExecuteBinary
inputs:
  path: C:\PathTo\dotnet_installer.exe
  arguments:
    - /qb
    - /norestart
```


**Output**  

| Field | Description | Type | 
| --- | --- | --- | 
| stdout | Standard output of command execution\. | string | 

**Output Example**

```
{
      "stdout": "success"
}
```

## ExecuteBash<a name="image-builder-action-modules-executebash"></a>

The **ExecuteBash** module allows you to run bash scripts with inline shell code/commands\. This module supports Linux\. 

All of the commands and instructions that you specify in the commands block are converted into a file \(for example, `input.sh`\) and executed using the bash shell\. The result of the execution of the shell file is the exit code of the step\. 

The **ExecuteBash** module handles system restarts if the execution exits with an exit code of `194`\. When triggered, the application performs one of the following actions:
+ The application hands the exit code to the caller if it is executed by the SSM Agent\. The SSM Agent handles the system reboot and re\-invokes the execution as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.
+ The application saves the current `executionstate`, configures a restart trigger to re\-execute the application, and reboots the system\.

After system restart, the application executes the same step that triggered the restart\. If you require this functionality, you must write idempotent scripts that can handle multiple invocations of the same shell command\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| commands | Contains a list of instructions or commands to execute as per bash syntax\. Multi\-line YAML is allowed\. | List | Yes | 

**Input Example**

```
name: InstallAndValidateCorretto
action: ExecuteBash
inputs:
  commands:
    - sudo yum install java-11-amazon-corretto-headless -y
    - |
      function fail_with_message() {
          1>&2 echo $1
          exit 1
      }

      ARCH=`/usr/bin/arch`

      JAVA_PATH=/usr/lib/jvm/java-11-amazon-corretto.$ARCH/bin/java
      if [ -x $JAVA_PATH ]; then
          echo "Amazon Corretto 11 JRE is installed."
      else
          fail_with_message "Amazon Corretto 11 JRE is not installed. Failing."
      fi

      JAVAC_PATH=/usr/lib/jvm/java-11-amazon-corretto.$ARCH/bin/javac
      if [ -x $JAVAC_PATH ]; then
          echo "Amazon Corretto 11 JDK is installed."
      else
          fail_with_message "Amazon Corretto 11 JDK is not installed. Failing."
      fi
```


**Output**  

| Field | Description | Type | 
| --- | --- | --- | 
| stdout | Standard output of command execution\. | string | 

**Output Example**

```
{
    “stdout”: “This is the standard output from the shell execution\n"
}
```

If you execute a reboot and return exit code 194 as part of the action module, the build will resume at the same action module step that initiated the reboot\. If you execute a reboot without the exit code, the build process may fail\.

## ExecutePowerShell<a name="image-builder-action-modules-executepowershell"></a>

The **ExecutePowerShell **module allows you to run PowerShell scripts with inline shell code/commands\. This module supports the Windows platform and Windows PowerShell\.

All of the commands/instructions specified in the commands block are converted into a script file \(for example, `input.ps1`\) and executed using Windows PowerShell\. The result of the shell file execution is the exit code\.

The **ExecutePowerShell** module handles system restarts if the shell command exits with an exit code of `3010`\. When triggered, the application performs one of the following actions: 
+ Hands the exit code to the caller if executed by the SSM Agent\. The SSM Agent handles the system reboot and re\-invokes the execution as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.
+ Saves the current `executionstate`, configures a restart trigger to re\-execute the application, and reboots the system\.

After system restart, the application executes the same step that triggered the restart\. If you require this functionality, you must write idempotent scripts that can handle multiple invocations of the same shell command\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| commands | Contains a list of instructions or commands to execute as per PowerShell syntax\. Multi\-line YAML is allowed\. | String List | Yes Must specify `commands` or `file`, not both\. | 
| file | Contains the path to a PowerShell script file\. PowerShell will execute against this file using the \-file command line argument\. The path must point to a \.ps1 file\. | String | Yes Must specify `commands` or `file`, not both\. | 

**Input Example**

```
name: InstallMySoftware
action: ExecutePowerShell
inputs:
  commands: 
      - Set-SomeConfiguration -Value 10
      - Write-Host 'Successfully set the configuration.'
      
name: ExecuteMyScript
action: ExecutePowerShell
inputs:
  file: 'C:\PathTo\MyScript.ps1'
```


**Output**  

| Field | Description | Type | 
| --- | --- | --- | 
| stdout | Standard output of command execution\. | string | 

**Output Example**

```
{
    “stdout”: “This is the standard output from the shell execution\n"
}
```

If you execute a reboot and return exit code 3010 as part of the action module, the build will resume at the same action module step that initiated the reboot\. If you execute a reboot without the exit code, the build process may fail\.

## Reboot<a name="image-builder-action-modules-reboot"></a>

The **Reboot** action module reboots the instance\. It has a configurable option to delay the start of the reboot\. It does not support the step timeout value due to the instance getting rebooted\. Default behavior is that `delaySeconds` is `60`, which means that there is no delay\.

If the application is invoked by the SSM Agent, it hands the exit code \(`3010` for Windows, `194` for Linux\) to the SSM Agent\. The SSM Agent handles the system reboot as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.

If the application is invoked on the host as a standalone process, it saves the current execution state, configures a post reboot auto\-run trigger to re\-execute the application, and then reboots the system\.

**Post\-reboot auto\-run trigger:**
+ **Windows**\. Create a Task Scheduler entry with trigger At `SystemStartup`
+ **Linux**\. Add a job in crontab\.

```
@reboot /download/path/awstoe run --document s3://bucket/key/doc.yaml
```

This trigger is cleaned up when the application starts\.

To use the **Reboot** action module, for steps that contain reboot `exitcode` \(for example, `3010`\), you must run the application binary as `sudo user`\.


**Input**  

| Primitive | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
| delaySeconds | Delays a specific amount of time before initiating a reboot\. | Integer |  No  |  `0`  | 

**Input Example**

```
name: RebootStep
action: Reboot
onFailure: Abort
maxAttempts: 2
inputs:
    delaySeconds: 60
```

**Output **

None\.

When the **Reboot** module completes, Image Builder continues to the next step in the build\.

## UpdateOS<a name="image-builder-action-modules-updateos"></a>

The **UpdateOS** action module adds support for installing Windows and Linux updates\.

The **UpdateOS** action module installs all available updates by default\. You can override this action by providing a list of one or more updates to include for installation and/or a list of one or more updates to exclude from installation\.

If both “include" and "exclude" lists are provided, the resulting list of updates can include only those listed in the "include" list that are not listed in the "exclude" list\.
+ **Windows**\. Updates are installed from the update source configured on the target machine\.
+ **Linux**\. The application checks for the supported package manager in the Linux platform and uses either `yum` or `apt-get` package manager\. If neither are supported, an error is returned\. You should have `sudo` permissions to run the **UpdateOS** action module\. If you do not have `sudo` permissions an `error.Input` is returned\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| include |  For Windows, you can specify one or more Microsoft Knowledge Base \(KB\) article IDs to include in the list of updates that may be installed\. Valid formats are: `KB1234567` or `1234567`\.  For Linux, you can specify one or more packages to be included in the list for installation\.  | String List | No | 
| exclude |  For Windows, you can specify one or more Microsoft Knowledge Base \(KB\) article IDs to include in the list of updates to be excluded from the installation\.  For Linux, you can specify one or more packages to be excluded from the installation\.  | String List | No | 

**Input Example**

```
name: UpdateMyLinux
action: UpdateOS
onFailure: Abort
maxAttempts: 3
inputs:
    exclude:
        - ec2-hibinit-agent
```

**Output**

None\. 

## S3Upload<a name="image-builder-action-modules-s3upload"></a>

The **S3Upload** action module allows you to upload a file from a source file/folder to an Amazon S3 location\. Wildcards are permitted for use with the source and are denoted by the character `*`\. 

If the recursive **S3Upload** action fails, Amazon S3 files that have already been uploaded will remain\.

**Supported use cases:**

• Local file to S3 object\.

• Local files in folder \(with wildcard\) to S3 `KeyPrefix`\.

• Copy local folder \(must have `recurse` set to `true`\) to S3 `KeyPrefix`\.

**IAM Requirements**  
The IAM role that you associate with your instance profile must have permissions to run the **S3Upload** action module\. The following IAM role policy must be attached to the IAM role that is associated with the instance profile: `s3:PutObject` against the bucket/object \(for example, `arn:aws:s3:::BucketName/*`\)\.


**Input**  

| Primitive | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
| source | Local path\. Source supports wildcard denoted by a \*\. | String |  Yes  |  | 
| destination | Remote path\. | String |  Yes  |  | 
| recurse | When set to true, performs S3Upload recursively\. | String | No | false | 

**Input Example: Copy Local File to S3 Object**

The following example shows how to copy a local file to an Amazon S3 Object\.

```
name: MyS3UploadFile
action: S3Upload
onFailure: Abort
maxAttempts: 3
inputs:
    - source: C:\myfolder\package.zip
      destination: s3://mybucket/path/to/package.zip
```

**Input Example: Copy All Files in Local Folder to S3 Bucket with ****KeyPrefix**

The following example shows how to copy all files in the local folder to an Amazon S3 bucket with **KeyPrefix**\. This example does not copy sub\-folders or their contents because `recurse` is not specified and it defaults to `false`\. 

```
name: MyS3UploadMultipleFiles
action: S3Upload
onFailure: Abort
maxAttempts: 3
inputs:
    - source: C:\myfolder\*
      destination: s3://mybucket/path/to/
```

**Input Example: Copy All Files and Folders Recursively From a Local Folder to S3 Bucket **

The following example shows how to copy all files and folders recursively from a local folder to an Amazon S3 bucket with **KeyPrefix**\. 

```
name: MyS3UploadFolder
action: S3Upload
onFailure: Abort
maxAttempts: 3
inputs:
    - source: C:\myfolder\*
      destination: s3://mybucket/path/to/
      recurse: true
```

**Output**

None\. 

## S3Download<a name="image-builder-action-modules-s3download"></a>

The **S3Download** action module allows you to download an Amazon S3 object or **KeyPrefix** to a local destination path\. The destination path can be a file or folder\. If the destination path already exists, **S3Download** fails unless `override` is set to `true`\. 

If the **S3Download** action for **S3KeyPrefix** fails, the state of the destination folder remains as it is upon failure\. The folder contents are not rolled back to the contents before failure\.

**Supported use cases:**
+ S3 object to local file\.
+ S3 objects \(with **KeyPrefix** in S3 file path\) to local folder, which recursively copies all S3 files in a **KeyPrefix** to the local folder\.

**IAM Requirements**

The IAM role that you associate with your instance profile must have permissions to run the **S3Download** action module\. The following IAM role policies must be attached to the IAM role that is associated with the instance profile: 
+ **Single file**: `s3:GetObject` against the bucket/object \(for example, `arn:aws:s3:::BucketName/*`\)\.
+ **Multiple files**: `s3:ListBucket` against the bucket/object \(for example, `arn:aws:s3:::BucketName`\) and `s3:GetObject` against the bucket/object \(for example, `arn:aws:s3:::BucketName/*`\)\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| source | Remote path\. Source supports wildcard denoted by a \*\. | String |  Yes  | 
| destination | Local path\. | String | Yes | 

**Note**  
For the following examples, the Windows folder path can be replaced with a Linux path\. For example, `C:\myfolder\package.zip` can be replaced with `/myfolder/package.zip`\.

**Input Example: Copy S3 Object to Local File**

The following example shows how to copy an S3 Object to a local file\.

```
name: DownloadMyFile
action: S3Download
inputs:
    - source: s3://mybucket/path/to/package.zip
      destination: C:\myfolder\package.zip
```

**Input Example: Copy All S3 Objects in S3 Bucket with KeyPrefix to Local Folder**

The following example shows how to copy all S3 Objects in an Amazon S3 Bucket with the **KeyPrefix** to a local folder\. S3 has no concept of a folder, therefore all objects matching the **KeyPrefix** are copied\. The limit for maximum objects is 1000\.

```
name: MyS3DownloadKeyprefix
action: S3Download
maxAttempts: 3
inputs:
    - source: s3://mybucket/path/to/*
      destination: C:\myfolder\
```

**Output **

None\.

## SetRegistry<a name="image-builder-action-modules-setregistry"></a>

The **SetRegistry** action module accepts a list of inputs and allows you to set the value for the specified registry key\. If a registry key does not exist, it is created in the defined path\. This feature applies only to Windows\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| path | Path of registry key\. | String | Yes | 
| name | Name of registry key\. | String | Yes | 
| value | Value of registry key\. | String/Number/Array | Yes | 
| type | Value type of registry key\. | String | Yes | 

**Supported path prefixes**
+ `HKEY_CLASSES_ROOT / HKCR:`
+ `HKEY_USERS / HKU:`
+ `HKEY_LOCAL_MACHINE / HKLM:`
+ `HKEY_CURRENT_CONFIG / HKCC:`
+ `HKEY_CURRENT_USER / HKCU:`

**Supported types**
+ `BINARY`
+ `DWORD`
+ `QWORD`
+ `SZ`
+ `EXPAND_SZ`
+ `MULTI_SZ`

**Input Example**

```
name: SetRegistryKeyValues
action: SetRegistry
maxAttempts: 3
inputs:
    - path: HKLM:\SOFTWARE\MySoftWare
      name: MyName
      value: FirstVersionSoftware
      type: SZ
    - path: HKEY_CURRENT_USER\Software\Test
      name: Version
      value: 1.1
      type: DWORD
```

**Output**

None\.