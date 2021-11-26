# Action modules supported by AWSTOE component manager<a name="toe-action-modules"></a>

This section contains each action module that is supported by the AWSTOE component management application used by EC2 Image Builder to configure the instance that builds your image\. Also included are the corresponding functionality details and input/output values of each action module\.

AWSTOE components are authored with plaintext YAML documents\. For more information about document syntax, see [Use component documents in AWSTOE](toe-use-documents.md)\.

**Note**  
All action modules are run by using the same account as the Systems Manager agent, which is `root` on Linux and `NT Authority\SYSTEM` on Windows\.

**Topics**
+ [General execution modules](#action-modules-general-execution)
+ [File download and upload modules](#action-modules-download-upload)
+ [File system operation modules](#action-modules-file-system-operations)
+ [Software installation actions](#action-modules-software-install-actions)
+ [System action modules](#action-modules-file-system-actions)

## General execution modules<a name="action-modules-general-execution"></a>

The following section contains details for action modules that perform general execution commands and instructions\.

**Topics**
+ [ExecuteBash](#action-modules-executebash)
+ [ExecuteBinary](#action-modules-executebinary)
+ [ExecutePowerShell](#action-modules-executepowershell)

### ExecuteBash<a name="action-modules-executebash"></a>

 

The **ExecuteBash** action module allows you to run bash scripts with inline shell code/commands\. This module supports Linux\. 

All of the commands and instructions that you specify in the commands block are converted into a file \(for example, `input.sh`\) and run with the bash shell\. The result of running the shell file is the exit code of the step\. 

The **ExecuteBash** module handles system restarts if the script exits with an exit code of `194`\. When initiated, the application performs one of the following actions:
+ The application hands the exit code to the caller if it is run by the Systems Manager Agent\. The Systems Manager Agent handles the system reboot and runs the same step that initiated the restart, as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.
+ The application saves the current `executionstate`, configures a restart trigger to rerun the application, and restarts the system\.

After system restart, the application runs the same step that initiated the restart\. If you require this functionality, you must write idempotent scripts that can handle multiple invocations of the same shell command\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| commands | Contains a list of instructions or commands to run as per bash syntax\. Multi\-line YAML is allowed\. | List | Yes | 

**Input example: install and validate Corretto**

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

**Output example**

```
{
    “stdout”: “This is the standard output from running the shell\n"
}
```

If you start a reboot and return exit code `194` as part of the action module, the build will resume at the same action module step that initiated the reboot\. If you start a reboot without the exit code, the build process may fail\.

### ExecuteBinary<a name="action-modules-executebinary"></a>

The **ExecuteBinary** action module allows you to run binary files with a list of command\-line arguments\.

The **ExecuteBinary** module handles system restarts if the binary file exits with an exit code of `194` \(Linux\) or `3010` \(Windows\)\. When this happens, the application performs one of the following actions:
+ The application hands the exit code to the caller if it is run by the Systems Manager Agent\. The Systems Manager Agent handles restarting the system and runs the same step that initiated the restart, as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.
+ The application saves the current `executionstate`, configures a restart trigger to rerun the application, and restarts the system\.

After the system restarts, the application runs the same step that initiated the restart\. If you require this functionality, you must write idempotent scripts that can handle multiple invocations of the same shell command\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| path | The path to the binary file for execution\. | String | Yes | 
| arguments | Contains a list of command\-line arguments to use when running the binary\. | String List | No | 

**Input example: install \.NET**

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

**Output example**

```
{
      "stdout": "success"
}
```

### ExecutePowerShell<a name="action-modules-executepowershell"></a>

The **ExecutePowerShell** action module allows you to run PowerShell scripts with inline shell code/commands\. This module supports the Windows platform and Windows PowerShell\.

All of the commands/instructions specified in the commands block are converted into a script file \(for example, `input.ps1`\) and run using Windows PowerShell\. The result of running the shell file is the exit code\.

The **ExecutePowerShell** module handles system restarts if the shell command exits with an exit code of `3010`\. When initiated, the application performs one of the following actions: 
+ Hands the exit code to the caller if run by the Systems Manager Agent\. The Systems Manager Agent handles the system reboot and runs the same step that initiated the restart, as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.
+ Saves the current `executionstate`, configures a restart trigger to rerun the application, and reboots the system\.

After system restart, the application runs the same step that initiated the restart\. If you require this functionality, you must write idempotent scripts that can handle multiple invocations of the same shell command\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| commands | Contains a list of instructions or commands to run as per PowerShell syntax\. Multi\-line YAML is allowed\. | String List | Yes\. Must specify `commands` or `file`, not both\.  | 
| file | Contains the path to a PowerShell script file\. PowerShell will run against this file using the \-file command line argument\. The path must point to a \.ps1 file\. | String | Yes\. Must specify `commands` or `file`, not both\.  | 

**Input example: install software using PowerShell commands**

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

**Output example**

```
{
    “stdout”: “This is the standard output from the shell execution\n"
}
```

If you run a reboot and return exit code `3010` as part of the action module, the build will resume at the same action module step that initiated the reboot\. If you run a reboot without the exit code, the build process may fail\.

## File download and upload modules<a name="action-modules-download-upload"></a>

The following section contains details for action modules that perform download and upload commands and instructions\.

**Topics**
+ [S3Download](#action-modules-s3download)
+ [S3Upload](#action-modules-s3upload)
+ [WebDownload](#action-modules-webdownload)

### S3Download<a name="action-modules-s3download"></a>

With the `S3Download` action module, you can download an Amazon S3 object, or a set of objects, to a local file or folder that you specify with the `destination` path\. If any file already exists in the specified location, and the `overwrite` flag is set to true, `S3Download` overwrites the file\.

Your `source` location can point to a specific object in Amazon S3, or you can use a key prefix with an asterisk wildcard \(`*`\) to download a set of objects that match the key prefix path\. When you specify a key prefix in your `source` location, the `S3Download` action module downloads everything that matches the prefix \(files and folders included\)\. Make sure that the key prefix ends with a forward\-slash, followed by an asterisk \(`/*`\), so that you download everything that matches the prefix\. For example:  `s3://my-bucket/my-folder/*`\.

**Note**  
All folders in the destination path must exist prior to download, or the download fails\.

If the `S3Download` action for a specified key prefix fails during a download, the folder content is not rolled back to its state prior to the failure\. The destination folder remains as it was at the time of the failure\.

**Supported use cases**  
The `S3Download` action module supports the following use cases:
+ The Amazon S3 object is downloaded to a local folder, as specified in the download path\.
+ Amazon S3 objects \(with a key prefix in the Amazon S3 file path\) are downloaded to the specified local folder, which recursively copies all Amazon S3 objects that match the key prefix to the local folder\.

**IAM requirements**  
The IAM role that you associate with your instance profile must have permissions to run the `S3Download` action module\. The following IAM policies must be attached to the IAM role that is associated with the instance profile:
+ **Single file**: `s3:GetObject` against the bucket/object \(for example, `arn:aws:s3:::BucketName/*`\)\.
+ **Multiple files**: `s3:ListBucket` against the bucket/object \(for example, `arn:aws:s3:::BucketName`\) and `s3:GetObject` against the bucket/object \(for example, `arn:aws:s3:::BucketName/*`\)\.


**Input**  

|  Primitive  |  Description  |  Type  |  Required  |  Default  | 
| --- | --- | --- | --- | --- | 
|  `source`  |  The Amazon S3 bucket that is the source for your download\. You can specify a path to a specific object, or use a key prefix, that ends with a forward\-slash, followed by an asterisk wildcard \(`/*`\), to download a set of objects that match the key prefix\.   |  String  |  Yes  |  N/A  | 
|  `destination`  |  The local path where the Amazon S3 objects are downloaded\.  |  String  |  Yes  |  N/A  | 
|  `expectedBucketOwner`  |  Expected owner account ID of the bucket provided in the `source` path\. We recommend that you verify the ownership of the Amazon S3 bucket specified in the source\.  |  String  |  No  |  N/A  | 
|  `overwrite`  |  When set to true, if a file of the same name already exists in the destination folder for the specified local path, the download file overwrites the local file\. When set to false, the existing file on the local system is protected from being overwritten, and the action module fails with a download error\. For example, `Error: S3Download: File already exists and "overwrite" property for "destination" file is set to false. Cannot download.`  |  Boolean  |  No  |  true  | 

**Note**  
For the following examples, the Windows folder path can be replaced with a Linux path\. For example, `C:\myfolder\package.zip` can be replaced with `/myfolder/package.zip`\.

**Input example: copy an Amazon S3 object to a local file**  
The following example shows how to copy an Amazon S3 object to a local file\.

```
name: DownloadMyFile
action: S3Download
inputs:
    - source: s3://mybucket/path/to/package.zip
      destination: C:\myfolder\package.zip
      expectedBucketOwner: 123456789022
      overwrite: false
    - source: s3://mybucket/path/to/package.zip
      destination: C:\myfolder\package.zip
      expectedBucketOwner: 123456789022
      overwrite: true
    - source: s3://mybucket/path/to/package.zip
      destination: C:\myfolder\package.zip
      expectedBucketOwner: 123456789022
```

**Input example: copy all Amazon S3 objects in an Amazon S3 bucket with key prefix to a local folder**  
The following example shows how to copy all Amazon S3 objects in an Amazon S3 bucket with the key prefix to a local folder\. Amazon S3 has no concept of a folder, therefore all objects that match the key prefix are copied\. The maximum number of objects that can be downloaded is 1000\.

```
name: MyS3DownloadKeyprefix
action: S3Download
maxAttempts: 3
inputs:
    - source: s3://mybucket/path/to/*
      destination: C:\myfolder\
      expectedBucketOwner: 123456789022
      overwrite: false
    - source: s3://mybucket/path/to/*
      destination: C:\myfolder\
      expectedBucketOwner: 123456789022
      overwrite: true
    - source: s3://mybucket/path/to/*
      destination: C:\myfolder\
      expectedBucketOwner: 123456789022
```

**Output**  
None\.

### S3Upload<a name="action-modules-s3upload"></a>

With the **S3Upload** action module, you can upload a file from a source file or folder to an Amazon S3 location\. You can use a wildcard \(`*`\) in the path specified for your source location to upload all of the files whose path matches the wildcard pattern\.

If the recursive **S3Upload** action fails, any files that have already been uploaded will remain in the destination Amazon S3 bucket\.

**Supported use cases**
+ Local file to Amazon S3 object\.
+ Local files in folder \(with wildcard\) to Amazon S3 key prefix\.
+ Copy local folder \(must have `recurse` set to `true`\) to Amazon S3 key prefix\.

**IAM requirements**  
The IAM role that you associate with your instance profile must have permissions to run the `S3Upload` action module\. The following IAM policy must be attached to the IAM role that is associated with the instance profile\. The policy must grant `s3:PutObject` permissions to the target Amazon S3 bucket\. For example, `arn:aws:s3:::BucketName/*`\)\.


**Input**  

|  Primitive  |  Description  |  Type  |  Required  |  Default  | 
| --- | --- | --- | --- | --- | 
|  `source`  |  The local path where source files/folders originate\. The `source` supports an asterisk wildcard \(`*`\)\.  |  String  |  Yes  |  N/A  | 
|  `destination`  |  The path for the destination Amazon S3 bucket where source files/folders are uploaded\.  |  String  |  Yes  |  N/A  | 
|  `recurse`  |  When set to `true`, performs **S3Upload** recursively\.  |  String  |  No  |  `false`  | 
|  `expectedBucketOwner`  |  The expected owner account ID for the Amazon S3 bucket specified in the destination path\. We recommend that you verify the ownership of the Amazon S3 bucket specified in the destination\.  |  String  |  No  |  N/A  | 

**Input example: copy a local file to an Amazon S3 object**  
The following example shows how to copy a local file to an Amazon S3 object\.

```
name: MyS3UploadFile
action: S3Upload
onFailure: Abort
maxAttempts: 3
inputs:
    - source: C:\myfolder\package.zip
      destination: s3://mybucket/path/to/package.zip
      expectedBucketOwner: 123456789022
```

**Input example: copy all files in a local folder to an Amazon S3 bucket with key prefix**  
The following example shows how to copy all files in the local folder to an Amazon S3 bucket with key prefix\. This example does not copy sub\-folders or their contents because `recurse` is not specified, and it defaults to `false`\.

```
name: MyS3UploadMultipleFiles
action: S3Upload
onFailure: Abort
maxAttempts: 3
inputs:
    - source: C:\myfolder\*
      destination: s3://mybucket/path/to/
      expectedBucketOwner: 123456789022
```

**Input example: copy all files and folders recursively from a local folder to an Amazon S3 bucket**  
The following example shows how to copy all files and folders recursively from a local folder to an Amazon S3 bucket with key prefix\.

```
name: MyS3UploadFolder
action: S3Upload
onFailure: Abort
maxAttempts: 3
inputs:
    - source: C:\myfolder\*
      destination: s3://mybucket/path/to/
      recurse: true
      expectedBucketOwner: 123456789022
```

**Output**  
None\.

### WebDownload<a name="action-modules-webdownload"></a>

The **WebDownload** action module allows you to download files and resources from a remote location over the HTTP/HTTPS protocol \(*HTTPS is recommended*\)\. There are no limits on the number or size of downloads\. This module handles retry and exponential backoff logic\. 

Each download operation is allocated a maximum of 5 attempts to succeed according to user inputs\. These attempts differ from those specified in the `maxAttempts` field of document `steps`, which are related to action module failures\.

This action module implicitly handles redirects\. All HTTP status codes, except for `200`, result in an error\.


**Input**  

| Primitive | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
| source | The valid HTTP/HTTPS URL \(HTTPS is recommended\), which follows the RFC 3986 standard\. Chaining expressions are permitted\. | String |  Yes  | N/A | 
| destination | An absolute or relative file or folder path on the local system\. Folder paths must end with /\. If they do not end with /, they will be treated as file paths\. The module creates any required file or folder for successful downloads\. Chaining expressions are permitted\. | String | Yes | N/A | 
| overwrite | When enabled, overwrites any existing files on the local system with the downloaded file or resource\. When not enabled, any existing files on the local system are not overwritten, and the action module fails with an error\. When overwrite is enabled and checksum and algorithm are specified, then the action module downloads the file only if the checksum and the hash of any pre\-existing files do not match\.  | Boolean | No | true | 
| checksum | When you specify the checksum, it is checked against the hash of the downloaded file that is generated with the supplied algorithm\. For file verification to be enabled, both the checksum and the algorithm must be provided\. Chaining expressions are permitted\.  | String | No | N/A | 
| algorithm | The algorithm used to calculate the checksum\. The options are MD5, SHA1, SHA256, and SHA512\. For file verification to be enabled, both the checksum and the algorithm must be provided\. Chaining expressions are permitted\.  | String | No | N/A | 
| ignoreCertificateErrors | SSL certificate validation is ignored when enabled\. | Boolean | No | false | 


**Output**  

| Primitive | Description | Type | 
| --- | --- | --- | 
| destination | Newline character\-delimited string that specifies the destination path where the downloaded files or resources are stored\. | String | 

**Input example: download remote file to local destination**

```
name: DownloadRemoteFile
action: WebDownload
maxAttempts: 3
inputs:
  - source: https://testdomain/path/to/java14.zip
    destination: C:\testfolder\package.zip


Output:
{
  "destination": "C:\\testfolder\\package.zip"
}
```

**Input example: download more than one remote file to more than one local destination **

```
name: DownloadRemoteFiles
action: WebDownload
maxAttempts: 3
inputs:
  - source: https://testdomain/path/to/java14.zip
    destination: /tmp/java14_renamed.zip
  - source: https://testdomain/path/to/java14.zip
    destination: /tmp/create_new_folder_and_add_java14_as_zip/


Output:
{
  "destination": "/tmp/create_new_folder/java14_renamed.zip\n/tmp/create_new_folder_and_add_java14_as_zip/java14.zip"
}
```

**Input example: download one remote file without overwriting local destination, and download another remote file with file verification**

```
name: DownloadRemoteMultipleProperties
action: WebDownload
maxAttempts: 3
inputs:
  - source: https://testdomain/path/to/java14.zip
    destination: C:\create_new_folder\java14_renamed.zip
    overwrite: false
  - source: https://testdomain/path/to/java14.zip
    destination: C:\create_new_folder_and_add_java14_as_zip\
    checksum: ac68bbf921d953d1cfab916cb6120864
    algorithm: MD5
    overwrite: true


Output:
{
  "destination": "C:\\create_new_folder\\java14_renamed.zip\nC:\\create_new_folder_and_add_java14_as_zip\\java14.zip"
}
```

**Input example: download remote file and ignore SSL certification validation**

```
name: DownloadRemoteIgnoreValidation
action: WebDownload
maxAttempts: 3
inputs:
  - source: https://www.bad-ssl.com/resource
    destination: /tmp/downloads/
    ignoreCertificateErrors: true


Output:
{
  "destination": "/tmp/downloads/resource"
}
```

## File system operation modules<a name="action-modules-file-system-operations"></a>

The following section contains details for action modules that perform file system operation commands and instructions\.

**Topics**
+ [AppendFile](#action-modules-appendfile)
+ [CopyFile](#action-modules-copyfile)
+ [CopyFolder](#action-modules-copyfolder)
+ [CreateFile](#action-modules-createfile)
+ [CreateFolder](#action-modules-createfolder)
+ [CreateSymlink](#action-modules-createsymlink)
+ [DeleteFile](#action-modules-deleteefile)
+ [DeleteFolder](#action-modules-deletefolder)
+ [ListFiles](#action-modules-listfiles)
+ [MoveFile](#action-modules-movefile)
+ [MoveFolder](#action-modules-movefolder)
+ [ReadFile](#action-modules-readfile)
+ [SetFileEncoding](#action-modules-setfileencoding)
+ [SetFileOwner](#action-modules-setfileowner)
+ [SetFolderOwner](#action-modules-setfolderowner)
+ [SetFilePermissions](#action-modules-setfilepermissions)
+ [SetFolderPermissions](#action-modules-setfolderpermissions)

### AppendFile<a name="action-modules-appendfile"></a>

The **AppendFile** action module adds specified content to the preexisting content of a file\.

If the file encoding value is different from the default encoding \(`utf-8`\) value, then you can specify the file encoding value by using the `encoding` option\. By default, `utf-16` and `utf-32` are assumed to use little\-endian encoding\. 

The action module returns an error when the following occurs:
+ The specified file does not exist at runtime\.
+ You don't have write permissions to modify the file content\.
+ The module encounters an error during the file operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Yes | 
| content | The content to be appended to the file\. | String | No | Empty string | N/A | Yes | 
| encoding | The encoding standard\. | String | No | utf8 | utf8, utf\-8, utf16,utf\-16, utf16\-LE, utf\-16\-LE utf16\-BE, utf\-16\-BE, utf32, utf\-32, utf32\-LE,utf\-32\-LE, utf32\-BE, and  utf\-32\-BE\. The value of the encoding option is case insensitive\. | Yes | 

**Input example: append file without encoding \(Linux\)**

```
name: AppendingFileWithOutEncodingLinux
action: AppendFile
inputs:
  - path: ./Sample.txt
    content: "The string to be appended to the file"
```

**Input example: append file without encoding \(Windows\)**

```
name: AppendingFileWithOutEncodingWindows
action: AppendFile
inputs:
  - path: C:\MyFolder\MyFile.txt
    content: "The string to be appended to the file"
```

**Input example: append file with encoding \(Linux\)**

```
name: AppendingFileWithEncodingLinux
action: AppendFile
inputs:
  - path: /FolderName/SampleFile.txt
    content: "The string to be appended to the file"
    encoding: UTF-32
```

**Input example: append file with encoding \(Windows\)**

```
name: AppendingFileWithEncodingWindows
action: AppendFile
inputs:
  - path: C:\MyFolderName\SampleFile.txt
    content: "The string to be appended to the file"
    encoding: UTF-32
```

**Input example: append file with empty string \(Linux\)**

```
name: AppendingEmptyStringLinux
action: AppendFile
inputs:
  - path: /FolderName/SampleFile.txt
```

**Input example: append file with empty string \(Windows\)**

```
name: AppendingEmptyStringWindows
action: AppendFile
inputs:
  - path: C:\MyFolderName\SampleFile.txt
```

**Output**  
None\.

### CopyFile<a name="action-modules-copyfile"></a>

The **CopyFile** action module copies files from the specified source to the specified destination\. By default, the module recursively creates the destination folder if it does not exist at runtime\.

If a file with the specified name already exists in the specified folder, the action module, by default, overwrites the existing file\. You can override this default behavior by setting the overwrite option to `false`\. When the overwrite option is set to `false`, and there is already a file in the specified location with the specified name, the action module will return an error\. This option works the same as the `cp` command in Linux, which overwrites by default\.

The source file name can include a wildcard \(`*`\)\. Wildcard characters are accepted only after the last file path separator \(`/` or `\`\)\. If wildcard characters are included in the source file name, all of the files that match the wildcard are copied to the destination folder\. If you want to move more than one file by using a wildcard character, the input to the `destination` option must end with a file path separator \(`/` or `\`\), which indicates that the destination input is a folder\.

If the destination file name is different from the source file name, you can specify the destination file name using the `destination` option\. If you do not specify a destination file name, the name of the source file is used to create the destination file\. Any text that follows the last file path separator \(`/` or `\`\) is treated as the file name\. If you want to use the same file name as the source file, then the input of the `destination` option must end with a file path separator \(`/` or `\`\)\. 

The action module returns an error when the following occurs:
+ You do not have permission to create a file in the specified folder\.
+ The source files do not exist at runtime\.
+ There is already a folder with the specified file name and the `overwrite` option is set to `false`\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| source | The source file path\. | String | Yes | N/A | N/A | Yes | 
| destination | The destination file path\. | String | Yes | N/A | N/A | Yes | 
| overwrite | When set to false, the destination files will not be replaced when there is already a file in the specified location with the specified name\. | Boolean | No | true | N/A | Yes | 

**Input example: copy a file \(Linux\)**

```
name: CopyingAFileLinux
action: CopyFile
inputs:
  - source: /Sample/MyFolder/Sample.txt
    destination: /MyFolder/destinationFile.txt
```

**Input example: copy a file \(Windows\)**

```
name: CopyingAFileWindows
action: CopyFile
inputs:
  - source: C:\MyFolder\Sample.txt
    destination: C:\MyFolder\destinationFile.txt
```

**Input example: copy a file using the source file name \(Linux\)**

```
name: CopyingFileWithSourceFileNameLinux
action: CopyFile
inputs:
  - source: /Sample/MyFolder/Sample.txt
    destination: /MyFolder/
```

**Input example: copy a file using the source file name \(Windows\)**

```
name: CopyingFileWithSourceFileNameWindows
action: CopyFile
inputs:
  - source: C:\Sample\MyFolder\Sample.txt
    destination: C:\MyFolder\
```

**Input example: copy a file using the wildcard character \(Linux\)**

```
name: CopyingFilesWithWildCardLinux
action: CopyFile
inputs:
  - source: /Sample/MyFolder/Sample*
    destination: /MyFolder/
```

**Input example: copy a file using the wildcard character \(Windows\)**

```
name: CopyingFilesWithWildCardWindows
action: CopyFile
inputs:
  - source: C:\Sample\MyFolder\Sample*
    destination: C:\MyFolder\
```

**Input example: copy a file without overwriting \(Linux\)**

```
name: CopyingFilesWithoutOverwriteLinux
action: CopyFile
inputs:
  - source: /Sample/MyFolder/Sample.txt
    destination: /MyFolder/destinationFile.txt
    overwrite: false
```

**Input example: copy a file without overwriting \(Windows\)**

```
name: CopyingFilesWithoutOverwriteWindows
action: CopyFile
inputs:
  - source: C:\Sample\MyFolder\Sample.txt
    destination: C:\MyFolder\destinationFile.txt
    overwrite: false
```

**Output**  
None\.

### CopyFolder<a name="action-modules-copyfolder"></a>

The **CopyFolder** action module copies a folder from the specified source to the specified destination\. The input for the `source` option is the folder to copy, and the input for the `destination` option is the folder where the contents of the source folder are copied\. By default, the module recursively creates the destination folder if it does not exist at runtime\.

If a folder with the specified name already exists in the specified folder, the action module, by default, overwrites the existing folder\. You can override this default behavior by setting the overwrite option to `false`\. When the overwrite option is set to `false`, and there is already a folder in the specified location with the specified name, the action module will return an error\.

The source folder name can include a wildcard \(`*`\)\. Wildcard characters are accepted only after the last file path separator \(`/` or `\`\)\. If wildcard characters are included in the source folder name, all of the folders that match the wildcard are copied to the destination folder\. If you want to copy more than one folder by using a wildcard character, the input to the `destination` option must end with a file path separator \(`/` or `\`\), which indicates that the destination input is a folder\.

If the destination folder name is different from the source folder name, you can specify the destination folder name using the `destination` option\. If you do not specify a destination folder name, the name of the source folder is used to create the destination folder\. Any text that follows the last file path separator \(`/` or `\`\) is treated as the folder name\. If you want to use the same folder name as the source folder, then the input of the `destination` option must end with a file path separator \(`/` or `\`\)\. 

The action module returns an error when the following occurs:
+ You do not have permission to create a folder in the specified folder\.
+ The source folders do not exist at runtime\.
+ There is already a folder with the specified folder name and the `overwrite` option is set to `false`\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| source | The source folder path\. | String | Yes | N/A | N/A | Yes | 
| destination | The destination folder path\. | String | Yes | N/A | N/A | Yes | 
| overwrite | When set to false, the destination folders will not be replaced when there is already a folder in the specified location with the specified name\. | Boolean | No | true | N/A | Yes | 

**Input example: copy a folder \(Linux\)**

```
name: CopyingAFolderLinux
action: CopyFolder
inputs:
  - source: /Sample/MyFolder/SampleFolder
    destination: /MyFolder/destinationFolder
```

**Input example: copy a folder \(Windows\)**

```
name: CopyingAFolderWindows
action: CopyFolder
inputs:
  - source: C:\Sample\MyFolder\SampleFolder
    destination: C:\MyFolder\destinationFolder
```

**Input example: copy a folder using the source folder name \(Linux\)**

```
name: CopyingFolderSourceFolderNameLinux
action: CopyFolder
inputs:
  - source: /Sample/MyFolder/SourceFolder
    destination: /MyFolder/
```

**Input example: copy a folder using the source folder name \(Windows\)**

```
name: CopyingFolderSourceFolderNameWindows
action: CopyFolder
inputs:
  - source: C:\Sample\MyFolder\SampleFolder
    destination: C:\MyFolder\
```

**Input example: copy a folder using the wildcard character \(Linux\)**

```
name: CopyingFoldersWithWildCardLinux
action: CopyFolder
inputs:
  - source: /Sample/MyFolder/Sample*
    destination: /MyFolder/
```

**Input example: copy a folder using the wildcard character \(Windows\)**

```
name: CopyingFoldersWithWildCardWindows
action: CopyFolder
inputs:
  - source: C:\Sample\MyFolder\Sample*
    destination: C:\MyFolder\
```

**Input example: copy a folder without overwriting \(Linux\)**

```
name: CopyingFoldersWithoutOverwriteLinux
action: CopyFolder
inputs:
  - source: /Sample/MyFolder/SourceFolder
    destination: /MyFolder/destinationFolder
    overwrite: false
```

**Input example: copy a folder without overwriting \(Windows\)**

```
name: CopyingFoldersWithoutOverwrite
action: CopyFolder
inputs:
  - source: C:\Sample\MyFolder\SourceFolder
    destination: C:\MyFolder\destinationFolder
    overwrite: false
```

**Output**  
None\.

### CreateFile<a name="action-modules-createfile"></a>

The **CreateFile** action module creates a file in a specified location\. By default, if required, the module also recursively creates the parent folders\.

If the file already exists in the specified folder, the action module, by default, truncates or overwrites the existing file\. You can override this default behavior by setting the overwrite option to `false`\. When the overwrite option is set to `false`, and there is already a file in the specified location with the specified name, the action module will return an error\.

If the file encoding value is different from the default encoding \(`utf-8`\) value, then you can specify the file encoding value by using the `encoding` option\. By default, `utf-16` and `utf-32` are assumed to use little\-endian encoding\. 

`owner`, `group`, and `permissions` are optional inputs\. The input for `permissions` must be a string value\. Files are created with default values when not provided\. These options are not supported on Windows platforms\. This action module validates and returns an error if the `owner`, `group`, and `permissions` options are used on Windows platforms\.

This action module can create a file the with permissions defined by the default `umask` value of the operating system\. You must set the `umask` value if you want to override the default value\.

The action module returns an error when the following occurs:
+ You do not have permission to create a file or a folder in the specified parent folder\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Yes | 
| content | The content of the file\. | String | No | N/A | N/A | Yes | 
| encoding | The encoding standard\. | String | No | utf8 | utf8, utf\-8, utf16,utf\-16, utf16\-LE, utf\-16\-LE utf16\-BE, utf\-16\-BE, utf32, utf\-32, utf32\-LE,utf\-32\-LE, utf32\-BE, and  utf\-32\-BE\. The value of the encoding option is case insensitive\. | Yes | 
| owner | The user name or ID\. | String | No | N/A | N/A | Not supported on Windows\. | 
| group | The group name or ID\. | String | No | The current user\. | N/A | Not supported on Windows\. | 
| permissions | The file permissions\. | String | No | 0666 | N/A | Not supported on Windows\. | 
| overwrite | If the name of the specified file already exists, setting this value to false prevents the file from being truncated or overwritten by default\. | Boolean | No | true | N/A | Yes | 

**Input example: create a file without overwriting \(Linux\)**

```
name: CreatingFileWithoutOverwriteLinux
action: CreateFile
inputs:
  - path: /home/UserName/Sample.txt
    content: ABCD\nRandom\tvalues
    overwrite: false
```

**Input example: create a file without overwriting \(Windows\)**

```
name: CreatingFileWithoutOverwriteWindows
action: CreateFile
inputs:
  - path: C:\Temp\Sample.txt
    content: ABCD\nRandom\tvalues
    overwrite: false
```

**Input example: create a file with file properties**

```
name: CreatingFileWithFileProperties
action: CreateFile
inputs:
  - path: SampleFolder/Sample.txt
    content: ABCD\nRandom\tvalues
    encoding: UTF-16
    owner: Ubuntu
    group: UbuntuGroup
    permissions: 0777
 - path: SampleFolder/SampleFile.txt
    permissions: 755
  - path: SampleFolder/TextFile.txt
    encoding: UTF-16
    owner: root
    group: rootUserGroup
```

**Input example: create a file without file properties**

```
name: CreatingFileWithoutFileProperties
action: CreateFile
inputs:
  - path: ./Sample.txt
  - path: Sample1.txt
```

**Input example: create an empty file to skip a section in the Linux housekeeping script**

```
name: CreateSkipCleanupfile
action: CreateFile
inputs:
  - path: <skip section file name>
```

For more information, see [Override the Linux housekeeping script](security-best-practices.md#override-linux-housekeeping-script)

**Output**  
None\.

### CreateFolder<a name="action-modules-createfolder"></a>

The **CreateFolder** action module creates a folder in a specified location\. By default, if required, the module also recursively creates the parent folders\.

If the folder already exists in the specified folder, the action module, by default, truncates or overwrites the existing folder\. You can override this default behavior by setting the overwrite option to `false`\. When the overwrite option is set to `false`, and there is already a folder in the specified location with the specified name, the action module will return an error\.

`owner`, `group`, and `permissions` are optional inputs\. The input for `permissions` must be a string value\. These options are not supported on Windows platforms\. This action module validates and returns an error if the `owner`, `group`, and `permissions` options are used on Windows platforms\.

This action module can create a folder the with permissions defined by the default `umask` value of the operating system\. You must set the `umask` value if you want to override the default value\.

The action module returns an error when the following occurs:
+ You do not have permission to create a folder in the specified location\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The folder path\. | String | Yes | N/A | N/A | Yes | 
| owner | The user name or ID\. | String | No | The current user\. | N/A | Not supported on Windows\. | 
| group | The group name or ID\. | String | No | The group of the current user\. | N/A | Not supported on Windows\. | 
| permissions | The folder permissions\. | String | No | 0777 | N/A | Not supported on Windows\. | 
| overwrite | If the name of the specified file already exists, setting this value to false prevents the file from being truncated or overwritten by default\. | Boolean | No | true | N/A | Yes | 

**Input example: create a folder \(Linux\)**

```
name: CreatingFolderLinux
action: CreateFolder
inputs:
  - path: /Sample/MyFolder/
```

**Input example: create a folder \(Windows\)**

```
name: CreatingFolderWindows
action: CreateFolder
inputs:
  - path: C:\MyFolder
```

**Input example: create a folder specifying folder properties**

```
name: CreatingFolderWithFolderProperties
action: CreateFolder
inputs:
  - path: /Sample/MyFolder/Sample/
    owner: SampleOwnerName
    group: SampleGroupName
    permissions: 0777
  - path: /Sample/MyFolder/SampleFoler/
    permissions: 777
```

**Input example: create a folder that overwrites the existing folder, if there is one\.**

```
name: CreatingFolderWithOverwrite
action: CreateFolder
inputs:
  - path: /Sample/MyFolder/Sample/
    overwrite: true
```

**Output**  
None\.

### CreateSymlink<a name="action-modules-createsymlink"></a>

The **CreateSymlink** action module creates symbolic links, or files that contain a reference to another file\. This module is not supported on Windows platforms\. 

The input for the `path` and `target` options can be either an absolute or relative path\. If the input for the `path` option is a relative path, it is replaced with the absolute path when the link is created\.

By default, when a link with the specified name already exists in the specified folder, the action module returns an error\. You can override this default behavior by setting the `force` option to `true`\. When the `force` option is set to `true`, the module will overwrite the existing link\.

If a parent folder does not exist, the action module creates the folder recursively, by default\.

The action module returns an error when the following occurs:
+ The target file does not exist at runtime\.
+ A nonsymbolic link file with the specified name already exists\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| target | The target file path to which the symbolic link points\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| force | Forces the creation of a link when a link with the same name already exists\. | Boolean | No | false | N/A | Not supported on Windows\. | 

**Input example: create symbolic link that forces the creation of a link**

```
name: CreatingSymbolicLinkWithForce
action: CreateSymlink
inputs:
  - path: /Folder2/Symboliclink.txt
    target: /Folder/Sample.txt
    force: true
```

**Input example: create a symbolic link that does not force the creation of a link**

```
name: CreatingSymbolicLinkWithOutForce
action: CreateSymlink
inputs:
  - path: Symboliclink.txt
    target: /Folder/Sample.txt
```

**Output**  
None\.

### DeleteFile<a name="action-modules-deleteefile"></a>

The **DeleteFile** action module deletes a file or files in a specified location\.

The input of `path` should be a valid file path or a file path with a wild card character \(`*`\) in the file name\. When wildcard characters are specified in the file name, all of the files within the same folder that match the wildcard will be deleted\. 

The action module returns an error when the following occurs:
+ You do not have permission to perform delete operations\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Yes | 

**Input example: delete a single file \(Linux\)**

```
name: DeletingSingleFileLinux
action: DeleteFile
inputs:
  - path: /SampleFolder/MyFolder/Sample.txt
```

**Input example: delete a single file \(Windows\)**

```
name: DeletingSingleFileWindows
action: DeleteFile
inputs:
  - path: C:\SampleFolder\MyFolder\Sample.txt
```

**Input example: delete a file that ends with "log" \(Linux\)**

```
name: DeletingFileEndingWithLogLinux
action: DeleteFile
inputs:
  - path: /SampleFolder/MyFolder/*log
```

**Input example: delete a file that ends with "log" \(Windows\)**

```
name: DeletingFileEndingWithLogWindows
action: DeleteFile
inputs:
  - path: C:\SampleFolder\MyFolder\*log
```

**Input example: delete all files in a specified folder \(Linux\)**

```
name: DeletingAllFilesInAFolderLinux
action: DeleteFile
inputs:
  - path: /SampleFolder/MyFolder/*
```

**Input example: delete all files in a specified folder \(Windows\)**

```
name: DeletingAllFilesInAFolderWindows
action: DeleteFile
inputs:
  - path: C:\SampleFolder\MyFolder\*
```

**Output**  
None\.

### DeleteFolder<a name="action-modules-deletefolder"></a>

The **DeleteFolder** action module deletes folders\.

If the folder is not empty, you must set the `force` option to `true` to remove the folder and its contents\. If you do not set the `force` option to `true`, and the folder you are trying to delete is not empty, the action module returns an error\. The default value of the `force` option is `false`\.

The action module returns an error when the following occurs:
+ You do not have permission to perform delete operations\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The folder path\. | String | Yes | N/A | N/A | Yes | 
| force | Removes the folder whether or not the folder is empty\. | Boolean | No | false | N/A | Yes | 

**Input example: delete a folder that is not empty using the `force` option \(Linux\)** 

```
name: DeletingFolderWithForceOptionLinux
action: DeleteFolder
inputs:
  - path: /Sample/MyFolder/Sample/
    force: true
```

**Input example: delete a folder that is not empty using the `force` option \(Windows\)** 

```
name: DeletingFolderWithForceOptionWindows
action: DeleteFolder
inputs:
  - path: C:\Sample\MyFolder\Sample\
    force: true
```

**Input example: delete a folder \(Linux\)** 

```
name: DeletingFolderWithOutForceLinux
action: DeleteFolder
inputs:
  - path: /Sample/MyFolder/Sample/
```

**Input example: delete a folder \(Windows\)** 

```
name: DeletingFolderWithOutForce
action: DeleteFolder
inputs:
  - path: C:\Sample\MyFolder\Sample\
```

**Output**  
None\.

### ListFiles<a name="action-modules-listfiles"></a>

The **ListFiles** action module lists the files in a specified folder\. When the recursive option is set to `true`, it lists the files in subfolders\. This module does not list files in subfolders by default\.

To list all of the files with names that match a specified pattern, use the `fileNamePattern` option to provide the pattern\. The `fileNamePattern` option accepts the wildcard \(`*`\) value\. When the `fileNamePattern` is provided, all of the files that match the specified file name format are returned\. 

The action module returns an error when the following occurs:
+ The specified folder does not exist at runtime\.
+ You do not have permission to create a file or a folder in the specified parent folder\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The folder path\. | String | Yes | N/A | N/A | Yes | 
| fileNamePattern | The pattern to match to list all files with names that match the pattern\. | String | No | N/A | N/A | Yes | 
| recursive | Lists files in the folder recursively\. | Boolean | No | false | N/A | Yes | 

**Input example: list files in specified folder \(Linux\)**

```
name: ListingFilesInSampleFolderLinux
action: ListFiles
inputs:
  - path: /Sample/MyFolder/Sample
```

**Input example: list files in specified folder \(Windows\)**

```
name: ListingFilesInSampleFolderWindows
action: ListFiles
inputs:
  - path: C:\Sample\MyFolder\Sample
```

**Input example: list files that end with "log" \(Linux\)**

```
name: ListingFilesWithEndingWithLogLinux
action: ListFiles
inputs:
  - path: /Sample/MyFolder/
    fileNamePattern: *log
```

**Input example: list files that end with "log" \(Windows\)**

```
name: ListingFilesWithEndingWithLogWindows
action: ListFiles
inputs:
  - path: C:\Sample\MyFolder\
    fileNamePattern: *log
```

**Input example: list files recursively**

```
name: ListingFilesRecursively
action: ListFiles
inputs:
  - path: /Sample/MyFolder/
    recursive: true
```


**Output**  

| Primitive | Description | Type | 
| --- | --- | --- | 
| files | The list of files\. | String | 

**Output example**

```
{
  "files": "/sample1.txt,/sample2.txt,/sample3.txt"
}
```

### MoveFile<a name="action-modules-movefile"></a>

The **MoveFile** action module moves files from the specified source to the specified destination\.

If the file already exists in the specified folder, the action module, by default, overwrites the existing file\. You can override this default behavior by setting the overwrite option to `false`\. When the overwrite option is set to `false`, and there is already a file in the specified location with the specified name, the action module will return an error\. This option works the same as the `mv` command in Linux, which overwrites by default\.

The source file name can include a wildcard \(`*`\)\. Wildcard characters are accepted only after the last file path separator \(`/` or `\`\)\. If wildcard characters are included in the source file name, all of the files that match the wildcard are copied to the destination folder\. If you want to move more than one file by using a wildcard character, the input to the `destination` option must end with a file path separator \(`/` or `\`\), which indicates that the destination input is a folder\.

If the destination file name is different from the source file name, you can specify the destination file name using the `destination` option\. If you do not specify a destination file name, the name of the source file is used to create the destination file\. Any text that follows the last file path separator \(`/` or `\`\) is treated as the file name\. If you want to use the same file name as the source file, then the input of the `destination` option must end with a file path separator \(`/` or `\`\)\. 

The action module returns an error when the following occurs:
+ You do not have permission to create a file in the specified folder\.
+ The source files do not exist at runtime\.
+ There is already a folder with the specified file name and the `overwrite` option is set to `false`\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| source | The source file path\. | String | Yes | N/A | N/A | Yes | 
| destination | The destination file path\. | String | Yes | N/A | N/A | Yes | 
| overwrite | When set to false, the destination files will not be replaced when there is already a file in the specified location with the specified name\. | Boolean | No | true | N/A | Yes | 

**Input example: move a file \(Linux\)**

```
name: MovingAFileLinux
action: MoveFile
inputs:
  - source: /Sample/MyFolder/Sample.txt
    destination: /MyFolder/destinationFile.txt
```

**Input example: move a file \(Windows\)**

```
name: MovingAFileWindows
action: MoveFile
inputs:
  - source: C:\Sample\MyFolder\Sample.txt
    destination: C:\MyFolder\destinationFile.txt
```

**Input example: move a file using the source file name \(Linux\)**

```
name: MovingFileWithSourceFileNameLinux
action: MoveFile
inputs:
  - source: /Sample/MyFolder/Sample.txt
    destination: /MyFolder/
```

**Input example: move a file using the source file name \(Windows\)**

```
name: MovingFileWithSourceFileNameWindows
action: MoveFile
inputs:
  - source: C:\Sample\MyFolder\Sample.txt
    destination: C:\MyFolder
```

**Input example: move a file using a wildcard character \(Linux\)**

```
name: MovingFilesWithWildCardLinux
action: MoveFile
inputs:
  - source: /Sample/MyFolder/Sample*
    destination: /MyFolder/
```

**Input example: move a file using a wildcard character \(Windows\)**

```
name: MovingFilesWithWildCardWindows
action: MoveFile
inputs:
  - source: C:\Sample\MyFolder\Sample*
    destination: C:\MyFolder
```

**Input example: move a file without overwriting \(Linux\)**

```
name: MovingFilesWithoutOverwriteLinux
action: MoveFile
inputs:
  - source: /Sample/MyFolder/Sample.txt
    destination: /MyFolder/destinationFile.txt
    overwrite: false
```

**Input example: move a file without overwriting \(Windows\)**

```
name: MovingFilesWithoutOverwrite
action: MoveFile
inputs:
  - source: C:\Sample\MyFolder\Sample.txt
    destination: C:\MyFolder\destinationFile.txt
    overwrite: false
```

**Output**  
None\.

### MoveFolder<a name="action-modules-movefolder"></a>

The **MoveFolder** action module moves folders from the specified source to the specified destination\. The input for the `source` option is the folder to move, and the input to the `destination` option is the folder where the contents of the source folders are moved\.

If the destination parent folder or the input to the `destination` option does not exist at runtime, the default behavior of the module is to recursively create the folder at the specified destination\.

If a folder with the same as the source folder already exists in the destination folder, the action module, by default, overwrites the existing folder\. You can override this default behavior by setting the overwrite option to `false`\. When the overwrite option is set to `false`, and there is already a folder in the specified location with the specified name, the action module will return an error\.

The source folder name can include a wildcard \(`*`\)\. Wildcard characters are accepted only after the last file path separator \(`/` or `\`\)\. If wildcard characters are included in the source folder name, all of the folders that match the wildcard are copied to the destination folder\. If you want to move more than one folder by using a wildcard character, the input to the `destination` option must end with a file path separator \(`/` or `\`\), which indicates that the destination input is a folder\.

If the destination folder name is different from the source folder name, you can specify the destination folder name using the `destination` option\. If you do not specify a destination folder name, the name of the source folder is used to create the destination folder\. Any text that follows the last file path separator \(`/` or `\`\) is treated as the folder name\. If you want to use the same folder name as the source folder, then the input of the `destination` option must end with a file path separator \(`/` or `\`\)\. 

The action module returns an error when the following occurs:
+ You do not have permission to create a folder in the destination folder\.
+ The source folders do not exist at runtime\.
+ There is already a folder with the specified name and the `overwrite` option is set to `false`\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| source | The source folder path\. | String | Yes | N/A | N/A | Yes | 
| destination | The destination folder path\. | String | Yes | N/A | N/A | Yes | 
| overwrite | When set to false, the destination folders will not be replaced when there is already a folder in the specified location with the specified name\. | Boolean | No | true | N/A | Yes | 

**Input example: move a folder \(Linux\)**

```
name: MovingAFolderLinux
action: MoveFolder
inputs:
  - source: /Sample/MyFolder/SourceFolder
    destination: /MyFolder/destinationFolder
```

**Input example: move a folder \(Windows\)**

```
name: MovingAFolderWindows
action: MoveFolder
inputs:
  - source: C:\Sample\MyFolder\SourceFolder
    destination: C:\MyFolder\destinationFolder
```

**Input example: move a folder using the source folder name \(Linux\)**

```
name: MovingFolderWithSourceFolderNameLinux
action: MoveFolder
inputs:
  - source: /Sample/MyFolder/SampleFolder
    destination: /MyFolder/
```

**Input example: move a folder using the source folder name \(Windows\)**

```
name: MovingFolderWithSourceFolderNameWindows
action: MoveFolder
inputs:
  - source: C:\Sample\MyFolder\SampleFolder
    destination: C:\MyFolder\
```

**Input example: move a folder using a wildcard character \(Linux\)**

```
name: MovingFoldersWithWildCardLinux
action: MoveFolder
inputs:
  - source: /Sample/MyFolder/Sample*
    destination: /MyFolder/
```

**Input example: move a folder using a wildcard character \(Windows\)**

```
name: MovingFoldersWithWildCardWindows
action: MoveFolder
inputs:
  - source: C:\Sample\MyFolder\Sample*
    destination: C:\MyFolder\
```

**Input example: move a folder without overwriting \(Linux\)**

```
name: MovingFoldersWithoutOverwriteLinux
action: MoveFolder
inputs:
  - source: /Sample/MyFolder/SampleFolder
    destination: /MyFolder/destinationFolder
    overwrite: false
```

**Input example: move a folder without overwriting \(Windows\)**

```
name: MovingFoldersWithoutOverwriteWindows
action: MoveFolder
inputs:
  - source: C:\Sample\MyFolder\SampleFolder
    destination: C:\MyFolder\destinationFolder
    overwrite: false
```

**Output**  
None\.

### ReadFile<a name="action-modules-readfile"></a>

The **ReadFile** action module reads the content of a text file of type string\. This module can be used to read the content of a file for use in subsequent steps through chaining or for reading data to the `console.log` file\. If the specified path is a symbolic link, this module returns the content of the target file\. This module only supports text files\.

If the file encoding value is different from the default encoding \(`utf-8`\) value, then you can specify the file encoding value by using the `encoding` option\. By default, `utf-16` and `utf-32` are assumed to use little\-endian encoding\. 

By default, this module cannot print the file content to the `console.log` file\. You can override this setting by setting the `printFileContent` property to `true`\.

This module can return only the content of a file\. It cannot parse files, such as Excel or JSON files\.

The action module returns an error when the following occurs:
+ The file does not exist at runtime\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Yes | 
| encoding | The encoding standard\. | String | No | utf8 | utf8, utf\-8, utf16,utf\-16, utf16\-LE, utf\-16\-LE utf16\-BE, utf\-16\-BE, utf32, utf\-32, utf32\-LE,utf\-32\-LE, utf32\-BE, and  utf\-32\-BE\. The value of the encoding option is case insensitive\. | Yes | 
| printFileContent | Prints the file content to the console\.log file\. | Boolean | No | false | N/A | Yes\. | 

**Input example: read a file \(Linux\)**

```
name: ReadingFileLinux
action: ReadFile
inputs:
  - path: /home/UserName/SampleFile.txt
```

**Input example: read a file \(Windows\)**

```
name: ReadingFileWindows
action: ReadFile
inputs:
  - path: C:\Windows\WindowsUpdate.log
```

**Input example: read a file and specify encoding standard**

```
name: ReadingFileWithFileEncoding
action: ReadFile
inputs:
  - path: /FolderName/SampleFile.txt
    encoding: UTF-32
```

**Input example: read a file and print to the `console.log` file**

```
name: ReadingFileToConsole
action: ReadFile
inputs:
  - path: /home/UserName/SampleFile.txt
    printFileContent: true
```


**Output**  

| Field | Description | Type | 
| --- | --- | --- | 
| content | The file content\. | string | 

**Output example**

```
{
    "content" : "The file content"
}
```

### SetFileEncoding<a name="action-modules-setfileencoding"></a>

The **SetFileEncoding** action module modifies the encoding property of an existing file\. This module can convert file encoding from `utf-8` to a specified encoding standard\. By default, `utf-16` and `utf-32` are assumed to be little\-endian encoding\.

The action module returns an error when the following occurs:
+ You do not have permission to perform the specified modification\.
+ The file does not exist at runtime\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Yes | 
| encoding | The encoding standard\. | String | No | utf8 | utf8, utf\-8, utf16,utf\-16, utf16\-LE, utf\-16\-LE utf16\-BE, utf\-16\-BE, utf32, utf\-32, utf32\-LE,utf\-32\-LE, utf32\-BE, and  utf\-32\-BE\. The value of the encoding option is case insensitive\. | Yes | 

**Input example: set file encoding property**

```
name: SettingFileEncodingProperty
action: SetFileEncoding
inputs:
  - path: /home/UserName/SampleFile.txt
    encoding: UTF-16
```

**Output**  
None\.

### SetFileOwner<a name="action-modules-setfileowner"></a>

The **SetFileOwner** action module modifies the `owner` and`group` owner properties of an existing file\. If the specified file is a symbolic link, the module modifies the `owner` property of the source file\. This module is not supported on Windows platforms\. 

This module accepts user and group names as inputs\. If the group name is not provided, the module assigns the group owner of the file to the group that the user belongs to\.

The action module returns an error when the following occurs:
+ You do not have permission to perform the specified modification\.
+ The specified user or group name does not exist at runtime\.
+ The file does not exist at runtime\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| owner | The user name\. | string | Yes | N/A | N/A | Not supported on Windows\. | 
| group | The name of the user group\. | String | No | The name of the group that the user belongs to\. | N/A | Not supported on Windows\. | 

**Input example: set file owner property without specifying the name of the user group**

```
name: SettingFileOwnerPropertyNoGroup
action: SetFileOwner
inputs:
  - path: /home/UserName/SampleText.txt
    owner: LinuxUser
```

**Input example: set file owner property by specifying the owner and the user group**

```
name: SettingFileOwnerProperty
action: SetFileOwner
inputs:
  - path: /home/UserName/SampleText.txt
    owner: LinuxUser
    group: LinuxUserGroup
```

**Output**  
None\.

### SetFolderOwner<a name="action-modules-setfolderowner"></a>

The **SetFolderOwner** action module recursively modifies the `owner` and`group` owner properties of an existing folder\. By default, the module can modify ownership for all of the contents in a folder\. You can set the `recursive` option to `false` to override this behavior\. This module is not supported on Windows platforms\. 

This module accepts user and group names as inputs\. If the group name is not provided, the module assigns the group owner of the file to the group that the user belongs to\.

The action module returns an error when the following occurs:
+ You do not have permission to perform the specified modification\.
+ The specified user or group name does not exist at runtime\.
+ The folder does not exist at runtime\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The folder path\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| owner | The user name\. | string | Yes | N/A | N/A | Not supported on Windows\. | 
| group | The name of the user group\. | String | No | The name of the group that the user belongs to\. | N/A | Not supported on Windows\. | 
| recursive | Overrides the default behavior of modifying ownership for all of the contents of a folder when set to false\. | Boolean | No | true | N/A | Not supported on Windows\. | 

**Input example: set folder owner property without specifying the name of the user group**

```
name: SettingFolderPropertyWithOutGroup
action: SetFolderOwner
inputs:
  - path: /SampleFolder/
    owner: LinuxUser
```

**Input example: set folder owner property without overriding the ownership of all of the contents in a folder **

```
name: SettingFolderPropertyWithOutRecursively
action: SetFolderOwner
inputs:
  - path: /SampleFolder/
    owner: LinuxUser
    recursive: false
```

**Input example: set file ownership property by specifying the name of the user group**

```
name: SettingFolderPropertyWithGroup
action: SetFolderOwner
inputs:
  - path: /SampleFolder/
    owner: LinuxUser
    group: LinuxUserGroup
```

**Output**  
None\.

### SetFilePermissions<a name="action-modules-setfilepermissions"></a>

The **SetFilePermissions** action module modifies the `permissions` of an existing file\. This module is not supported on Windows platforms\. 

The input for `permissions` must be a string value\.

This action module can create a file the with permissions defined by the default umask value of the operating system\. You must set the `umask` value if you want to override the default value\.

The action module returns an error when the following occurs:
+ You do not have permission to perform the specified modification\.
+ The file does not exist at runtime\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The file path\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| permissions | The file permissions\. | String | Yes | N/A | N/A | Not supported on Windows\. | 

**Input example: modify file permissions**

```
name: ModifyingFilePermissions
action: SetFilePermissions
inputs:
  - path: /home/UserName/SampleFile.txt
    permissions: 766
```

**Output**  
None\.

### SetFolderPermissions<a name="action-modules-setfolderpermissions"></a>

The **SetFolderPermissions** action module recursively modifies the `permissions` of an existing folder and all of its subfiles and subfolders\. By default, this module can modify permissions for all of the contents of the specified folder\. You can set the `recursive` option to `false` to override this behavior\. This module is not supported on Windows platforms\. 

The input for `permissions` must be a string value\. 

This action module can modify permissions according to the default umask value of the operating system\. You must set the `umask` value if you want to override the default value\.

The action module returns an error when the following occurs:
+ You do not have permission to perform the specified modification\.
+ The folder does not exist at runtime\.
+ The action module encounters an error while performing the operation\.


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | Supported on all platforms | 
| --- | --- | --- | --- | --- | --- | --- | 
| path | The folder path\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| permissions | The folder permissions\. | String | Yes | N/A | N/A | Not supported on Windows\. | 
| recursive | Overrides the default behavior of modifying permissions for all of the contents of a folder when set to false\. | Boolean | No | true | N/A | Not supported on Windows\. | 

**Input example: set folder permissions**

```
name: SettingFolderPermissions
action: SetFolderPermissions
inputs:
  - path: SampleFolder/
    permissions: 0777
```

**Input example: set folder permissions without modifying permissions for all of the contents of a folder**

```
name: SettingFolderPermissionsNoRecursive
action: SetFolderPermissions
inputs:
  - path: /home/UserName/SampleFolder/
    permissions: 777
    recursive: false
```

**Output**  
None\.

## Software installation actions<a name="action-modules-software-install-actions"></a>

This section describes action modules that perform software installation action commands and instructions\.

**IAM requirements**  
If your installation download path is an S3 URI, then the IAM role that you associate with your instance profile must have permission to run the `S3Download` action module\. To grant the required permission, attach the `S3:GetObject` IAM policy to the IAM role that is associated with your instance profile, and specify the path for your bucket\. For example, `arn:aws:s3:::BucketName/*`\)\.

**Complex MSI Inputs**  
If your input strings contain double quote characters \(`"`\), you must use one of the following methods to ensure that they are interpreted correctly:
+ You can use single quotes \('\) on the outside of your string, to contain it, and double quotes \("\) inside of your string, as shown in the following example\.

  ```
  properties:
    COMPANYNAME: '"Acme ""Widgets"" and ""Gizmos."""'
  ```

  In this case, if you need to use an apostrophe inside of your string, you must escape it\. This means using another single quote \('\) before the apostrophe\.
+ You can use double quotes \("\) on the outside of your string, to contain it\. And you can escape any double quotes inside of your string, using the backslash character \(`\`\), as shown in the following example\.

  ```
  properties:
    COMPANYNAME: "\"Acme \"\"Widgets\"\" and \"\"Gizmos.\"\"\""
  ```

Both of these methods pass the value `COMPANYNAME="Acme ""Widgets"" and ""Gizmos."""` to the msiexec command\.

**Topics**
+ [InstallMSI](#action-modules-install-msi)
+ [UninstallMSI](#action-modules-uninstall-msi)

### InstallMSI<a name="action-modules-install-msi"></a>

The `InstallMSI` action module installs a Windows application using an MSI file\. You can specify the MSI file using a local path, an S3 object URI, or a web URL\. The reboot option configures the reboot behavior of the system\.

AWSTOE generates the msiexec command based on the input parameters for the action module\. Values for the `path` \(MSI file location\) and `logFile` \(log file location\) input parameters must be enclosed in quotation marks \("\)\.

The following MSI exit codes are considered successful:
+ 0 \(Success\)
+ 1614 \(ERROR\_PRODUCT\_UNINSTALLED\)
+ 1641 \(Reboot Initiated\)
+ 3010 \(Reboot Required\)


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | 
| --- | --- | --- | --- | --- | --- | 
| path |  Specify the MSI file location using one of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html) Chaining expressions are allowed\.  | String | Yes | N/A | N/A | 
| reboot |  Configure the system reboot behavior that follows a successful run of the action module\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | String | No | Allow | Allow, Force, Skip | 
| logOptions |  Specify the options to use for MSI installation logging\. Specified flags are passed to the MSI installer, along with the `/L` command line parameter to enable logging\. If no flags are specified, AWSTOE uses the default value\. For more information about log options for MSI, see [Command Line Options](https://docs.microsoft.com/en-us/windows/win32/msi/command-line-options) in the Microsoft *Windows Installer* product documentation\.  | String | No | \*VX | i,w,e,a,r,u,c,m,o,p,v,x,\+,\!,\* | 
| logFile |  An absolute or relative path to the log file location\. If the log file path does not exist, it is created\. If the log file path is not provided, AWSTOE does not store the MSI installation log\.  | String | No | N/A | N/A | 
| properties |  MSI logging property key\-value pairs , for example: `TARGETDIR: "C:\target\location"`   Note: Modification of the following properties is not allowed: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | Map\[String\]String | No | N/A | N/A | 
| ignoreAuthenticodeSignatureErrors |  Flag to ignore authenticode signature validation errors for the installer specified in path\. The Get\-AuthenticodeSignature command is used to validate installers\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | Boolean | No | false | true, false | 
| allowUnsignedInstaller |  Flag to allow running the unsigned installer specified in the path\. The Get\-AuthenticodeSignature command is used to validate installers\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | Boolean | No | false | true, false | 

**Examples**  
The following examples show variations of the input section for your component document, depending on your installation path\.

**Input example: local document path installation**

```
- name: local-path-install
  steps:
    - name: LocalPathInstaller
      action: InstallMSI
      inputs:
        path: C:\sample.msi
        logFile: C:\msilogs\local-path-install.log
        logOptions: '*VX'
        reboot: Allow
        properties:
          COMPANYNAME: '"Amazon Web Services"'
        ignoreAuthenticodeSignatureErrors: true
        allowUnsignedInstaller: true
```

**Input example: Amazon S3 path installation**

```
- name: s3-path-install
  steps:
    - name: S3PathInstaller
      action: InstallMSI
      inputs:
        path: s3://<bucket-name>/sample.msi
        logFile: s3-path-install.log
        reboot: Force
        ignoreAuthenticodeSignatureErrors: false
        allowUnsignedInstaller: true
```

**Input example: web path installation**

```
- name: web-path-install
  steps:
    - name: WebPathInstaller
      action: InstallMSI
      inputs:
        path: https://<some-path>/sample.msi
        logFile: web-path-install.log
        reboot: Skip
        ignoreAuthenticodeSignatureErrors: true
        allowUnsignedInstaller: false
```

**Output**  
The following is an example of the output from the `InstallMSI` action module\.

```
{
  "logFile": "web-path-install.log",
  "msiExitCode": 0,
  "stdout": ""
}
```

### UninstallMSI<a name="action-modules-uninstall-msi"></a>

The `UninstallMSI` action module allows you to remove a Windows application using an MSI file\. You can specify the MSI file location using a local file path, an S3 object URI, or a web URL\. The reboot option configures the reboot behavior of the system\.

AWSTOE generates the msiexec command based on the input parameters for the action module\. The MSI file location \(`path`\) and log file location \(`logFile`\) are explicitly enclosed in double quotes \("\) while generating the msiexec command\.

The following MSI exit codes are considered successful:
+ 0 \(Success\)
+ 1605 \(ERROR\_UNKNOWN\_PRODUCT\)
+ 1614 \(ERROR\_PRODUCT\_UNINSTALLED\)
+ 1641 \(Reboot Initiated\)
+ 3010 \(Reboot Required\)


**Input**  

| Primitive | Description | Type | Required | Default value | Acceptable values | 
| --- | --- | --- | --- | --- | --- | 
| path |  Specify the MSI file location using one of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html) Chaining expressions are allowed\.  | String | Yes | N/A | N/A | 
| reboot |  Configures the system reboot behavior that follows a successful run of the action module\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | String | No | Allow | Allow, Force, Skip | 
| logOptions |  Specify the options to use for MSI installation logging\. Specified flags are passed to the MSI installer, along with the `/L` command line parameter to enable logging\. If no flags are specified, AWSTOE uses the default value\. For more information about log options for MSI, see [Command Line Options](https://docs.microsoft.com/en-us/windows/win32/msi/command-line-options) in the Microsoft *Windows Installer* product documentation\.  | String | No | \*VX | i,w,e,a,r,u,c,m,o,p,v,x,\+,\!,\* | 
| logFile |  An absolute or relative path to the log file location\. If the log file path does not exist, it is created\. If the log file path is not provided, AWSTOE does not store the MSI installation log\.  | String | No | N/A | N/A | 
| properties |  MSI logging property key\-value pairs , for example: `TARGETDIR: "C:\target\location"`   Note: Modification of the following properties is not allowed: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | Map\[String\]String | No | N/A | N/A | 
| ignoreAuthenticodeSignatureErrors |  Flag to ignore authenticode signature validation errors for the installer specified in path\. The Get\-AuthenticodeSignature command is used to validate installers\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | Boolean | No | false | true, false | 
| allowUnsignedInstaller |  Flag to allow running the unsigned installer specified in the path\. The Get\-AuthenticodeSignature command is used to validate installers\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html)  | Boolean | No | false | true, false | 

**Examples**  
The following examples show variations of the input section for your component document, depending on your installation path\.

**Input example: remove local document path installation**

```
- name: local-path-uninstall
  steps:
    - name: LocalPathUninstaller
      action: UninstallMSI
      inputs:
        path: C:\sample.msi
        logFile: C:\msilogs\local-path-uninstall.log
        logOptions: '*VX'
        reboot: Allow
        properties:
          COMPANYNAME: '"Amazon Web Services"'
        ignoreAuthenticodeSignatureErrors: true
        allowUnsignedInstaller: true
```

**Input example: remove Amazon S3 path installation**

```
- name: s3-path-uninstall
  steps:
    - name: S3PathUninstaller
      action: UninstallMSI
      inputs:
        path: s3://<bucket-name>/sample.msi
        logFile: s3-path-uninstall.log
        reboot: Force
        ignoreAuthenticodeSignatureErrors: false
        allowUnsignedInstaller: true
```

**Input example: remove web path installation**

```
- name: web-path-uninstall
  steps:
    - name: WebPathUninstaller
      action: UninstallMSI
      inputs:
        path: https://<some-path>/sample.msi
        logFile: web-path-uninstall.log
        reboot: Skip
        ignoreAuthenticodeSignatureErrors: true
        allowUnsignedInstaller: false
```

**Output**  
The following is an example of the output from the `UninstallMSI` action module\.

```
{
  "logFile": "web-path-uninstall.log",
  "msiExitCode": 0,
  "stdout": ""
}
```

## System action modules<a name="action-modules-file-system-actions"></a>

The following section describes action modules that perform file system action commands and instructions\.

**Topics**
+ [Reboot](#action-modules-reboot)
+ [SetRegistry](#action-modules-setregistry)
+ [UpdateOS](#action-modules-updateos)

### Reboot<a name="action-modules-reboot"></a>

The **Reboot** action module reboots the instance\. It has a configurable option to delay the start of the reboot\. By default, `delaySeconds` is set to `0`, which means that there is no delay\. Step timeout is not supported for the Reboot action module, as it does not apply when the instance is rebooted\.

If the application is invoked by the Systems Manager Agent, it hands the exit code \(`3010` for Windows, `194` for Linux\) to the Systems Manager Agent\. The Systems Manager Agent handles the system reboot as described in [Rebooting Managed Instance from Scripts](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-reboot.html)\.

If the application is invoked on the host as a standalone process, it saves the current execution state, configures a post\-reboot auto\-run trigger to rerun the application after the reboot, and then reboots the system\.

**Post\-reboot auto\-run trigger:**
+ **Windows**\. AWSTOE creates a Windows Task Scheduler entry with a trigger that runs automatically at `SystemStartup`
+ **Linux**\. AWSTOE adds a job in crontab that runs automatically after the system reboots\.

```
@reboot /download/path/awstoe run --document s3://bucket/key/doc.yaml
```

This trigger is cleaned up when the application starts\.

**Retries**  
By default, the maximum number of retries is set to the SSM `CommandRetryLimit`\. If the number of reboots exceeds the retry limit, the automation fails\. You can change the limit by editing the SSM agent config file \(`Mds.CommandRetryLimit`\)\. See [Runtime Configuration](https://github.com/aws/amazon-ssm-agent/blob/mainline/README.md#runtime-configuration) in the SSM agent open source\.

To use the **Reboot** action module, for steps that contain reboot `exitcode` \(for example, `3010`\), you must run the application binary as `sudo user`\.


**Input**  

| Primitive | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
| delaySeconds | Delays a specific amount of time before initiating a reboot\. | Integer |  No  |  `0`  | 

**Input example: reboot step**

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

### SetRegistry<a name="action-modules-setregistry"></a>

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

**Input example: set registry key values**

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

### UpdateOS<a name="action-modules-updateos"></a>

The **UpdateOS** action module adds support for installing Windows and Linux updates\.

The **UpdateOS** action module installs all available updates by default\. You can override this action by providing a list of one or more updates to include for installation\. You can also include a list of one or more updates to exclude from installation\.

If both "include" and "exclude" lists are provided, the resulting list of updates can include only those listed in the "include" list that are not listed in the "exclude" list\.
+ **Windows**\. Updates are installed from the update source configured on the target machine\.
+ **Linux**\. The application checks for the supported package manager in the Linux platform and uses either `yum` or `apt-get` package manager\. If neither are supported, an error is returned\. You should have `sudo` permissions to run the **UpdateOS** action module\. If you do not have `sudo` permissions an `error.Input` is returned\.


**Input**  

| Primitive | Description | Type | Required | 
| --- | --- | --- | --- | 
| include |  For Windows, you can specify the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html) For Linux, you can specify one or more packages to be included in the list of updates for installation\.  | String List | No | 
| exclude |  For Windows, you can specify the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-action-modules.html) For Linux, you can specify one or more packages to be excluded from the list of updates for installation\.  | String List | No | 

**Input example: add support for installing Linux updates**

```
name: UpdateMyLinux
action: UpdateOS
onFailure: Abort
maxAttempts: 3
inputs:
    exclude:
        - ec2-hibinit-agent
```

**Input example: add support for installing Windows updates**

```
name: UpdateWindowsOperatingSystem
action: UpdateOS
onFailure: Abort
maxAttempts: 3
inputs:
  include:
    - KB1234567
    - '*Security*'
```

**Output**

None\. 