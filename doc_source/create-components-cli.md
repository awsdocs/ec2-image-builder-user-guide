# Create a component \(AWS CLI\)<a name="create-components-cli"></a>

To create an AWSTOE application component using imagebuilder CLI commands, follow these steps:

1. 

**Create a YAML component document**

   To create a YAML application component document and upload dependencies using the AWS CLI, follow these steps:

   1. 

**Create a YAML component file**

      Use your favorite file editing tool to create a file named `component.yaml`, that has the following content:

      ```
      name: 'An_Example_Document'
      description: 'This document has a build, validate and test phase'
      schemaVersion: 1.0
      phases:
        - name: build
          steps:
            - name: Download_Scripts
              action: S3Download
              inputs:
                - source: 's3://my-s3-bucket/my-path/my_zip_archive.zip'
                  destination: 'c:\mydirectory\my_zip_archive.zip'
            - name: Extract_Tools
              action: ExecutePowerShell
              inputs:
                commands:
                  - 'Expand-Archive -LiteralPath {{build.Download_Scripts.inputs[0].destination}}'
            - name: 'DisableHibernation'
              action: ExecutePowerShell
              inputs:
                commands:
                  - c:\ec2amibuild\scripts\windows\Disable-Hibernation.ps1
        - name: validate
          steps:
            - name: DiskPercentageFree
              action: ExecutePowerShell
              inputs:
                commands:
                  - |
                    Function DiskPercentFree {
                      [CmdletBinding()]
                      Param (
                          [Parameter(Mandatory=$true)]
                          [ValidateNotNullOrEmpty()]
                          [string]$driveLetter,
      
                          [Parameter(Mandatory=$true)]
                          [ValidateNotNullOrEmpty()]
                          [string]$threshHold
                      )
                      $disk = Get-PSDrive $driveLetter | Select-Object Used,Free
                      $percentage_free = [Math]::round($disk.free/($disk.free+$disk.used) * 100,2)
                      if($percentage_free -ge $threshHold) {
                          return 0
                      }
                      return -1
                    }
                    DiskPercentFree -driveLetter C -threshHold 10
        - name: test
          steps:
            - name: 'RunTests'
              action: ExecutePowerShell
              inputs:
                commands:
                  - c:\ec2amibuild\scripts\windows\TestAMI.ps1
      ```

   1. 

**Upload the document to Amazon S3**

      *If your document is smaller than 64 KB, you can skip this step\.* Documents that are 64 KB or larger in size must be stored in Amazon S3\.

      ```
      aws s3 cp component.yaml s3://my-s3-bucket/my-path/component.yaml
      ```

   1. 

**Upload resource dependencies for the YAML document to Amazon S3**

      You must upload any resources that your document references, or your document will fail at runtime\. In this example, we upload the zip archive that the `component.yaml` document references\.

      ```
      aws s3 cp my_zip_archive.zip s3://my-s3-bucket/my-path/my_zip_archive.zip
      ```

1. 

**Create a component from the document**

   To streamline the imagebuilder create\-component command that is used in the AWS CLI, we create a JSON file that contains all of the component attributes we want to pass into the command, including the location of the `component.yaml` document created in the prior steps\. The `uri` key value pair contains the file reference\.

   1. 

**Create a CLI input JSON file**

      Use your favorite file editing tool to create a file named `create-component.json`, that has the following content:

      ```
      {
      "name": "MyExampleComponent",
      "semanticVersion": "2019.12.02",
      "description": "An example component that builds, validates and tests an image",
      "changeDescription": "Initial version.",
      "platform": "Windows",
      "uri": "s3://my-s3-bucket/my-path/component.yaml",
      "kmsKeyId": "arn:aws:kms:us-west-2:123456789012:key/60763706-b131-418b-8f85-3420912f020c",
      "tags": {
          "MyTagKey": "Some Value"
      }
      }
      ```
**Note**  
You must include the `file://` notation at the beginning of the JSON file path\.
The path for the JSON file should follow the appropriate convention for the base operating system where you are running the command\. For example, Windows uses the backslash \(\\\) to refer to the directory path, and Linux uses the forward slash \(/\)\.

   1. 

**Create the component**

      Use the following command to create the component:

      ```
      aws imagebuilder create-component --cli-input-json file://create-component.json
      ```