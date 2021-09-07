# Use component documents in AWSTOE<a name="toe-use-documents"></a>

To build a component using AWS Task Orchestrator and Executor \(AWSTOE\), you must provide a YAML\-based document that represents the phases and steps that apply for the component you create\. AWS services use your component when they create a new Amazon Machine Image \(AMI\) or container image\.

**Topics**
+ [Component document workflow](#component-doc-workflow)
+ [Component logging](#component-logging)
+ [Input and output chaining](#document-chaining)
+ [Document schema and definitions](#document-schema)
+ [Document example schemas](#document-example)
+ [Define and reference variables in AWSTOE](toe-user-defined-variables.md)
+ [Use looping constructs in AWSTOE](image-builder-looping-constructs.md)

## Component document workflow<a name="component-doc-workflow"></a>

The AWSTOE component document uses phases and steps to group related tasks, and organize those tasks into a logical workflow for the component\. Each component can contain phases that run during any stage in the image build process\.

**Tip**  
The service that uses your component to build an image might implement rules about what phases to use for their build process\. This is important to consider when you design your component\.

**Phases**  
Phases represent the progression of your workflow through the image build process\. For example, the Image Builder service uses `build` and `validate` phases during its *build stage* for the images it produces\. It uses the `test` phase during its *test stage* to ensure that the image snapshot or container image produces the expected results before creating the final AMI or distributing the container image\.

When the component runs, the associated commands for each phase are applied in the order that they appear in the component document\.

**Rules for phases**
+ Each phase name must be unique within a document\.
+ You can define many phases in your document\.
+ You must include at least one of the following phases in your document:
  + **build** – generally used during the *build stage*\.
  + **validate** – generally used during the *build stage*\.
  + **test** – generally used during the *test stage*\.

**Steps**  
Steps are individual units of work that define the workflow within each phase\. Steps run in sequential order\. However, input or output for one step can also feed into a subsequent step as input\. This is called "chaining"\.

**Rules for steps**
+ The step name must be unique for the phase\.
+ The step must use a supported action \(action module\) that returns an exit code\.
**Note**  
For a complete list of supported action modules, how they work, input/output values, and examples, see [Action modules supported by AWSTOE component manager](toe-action-modules.md)\.

## Component logging<a name="component-logging"></a>

AWSTOE creates a new log folder on the EC2 instances that are used for building and testing a new image, each time your component runs\. For container images, the log folder is stored in the container\.

To assist with troubleshooting if something goes wrong during the image creation process, the input document and all of the output files AWSTOE creates while running the component are stored in the log folder\.

The log folder name is comprised of the following parts:

1. **Log directory** – when a service runs a AWSTOE component, it passes in the log directory, along with other settings for the command\. For the following examples, we show the log file format that Image Builder uses\.
   + **Linux**: `/var/lib/amazon/toe/`
   + **Windows**: `$env:ProgramFiles\Amazon\TaskOrchestratorAndExecutor\`

1. **File prefix** – This is a standard prefix used for all components: "`TOE_`"\.

1. **Run time** – This is a timestamp in YYYY\-MM\-DD\_HH\-MM\-SS\_UTC\-0 format\.

1. **Execution ID** – This is the GUID that is assigned when AWSTOE runs one or more components\.

Example: `/var/lib/amazon/toe/TOE_2021-07-01_12-34-56_UTC-0_a1bcd2e3-45f6-789a-bcde-0fa1b2c3def4`

AWSTOE stores the following core files in the log folder:

**Input files**
+ **document\.yaml** – The document that is used as input for the command\. After the component runs, this file is stored as an artifact\.

**Output files**
+ **application\.log** – The application log contains timestamped debug level information from AWSTOE about what's happening as the component is running\.
+ **detailedoutput\.json** – This JSON file has detailed information about run status, inputs, outputs, and failures for all documents, phases, and steps that apply for the component as it runs\.
+ **console\.log** – The console log contains all of the standard out \(stdout\) and standard error \(stderr\) information that AWSTOE writes to the console while the component is running\.
+ **chaining\.json** – This JSON file represents optimizations that AWSTOE applied to resolve chaining expressions\.

**Note**  
The log folder might also contain other temporary files that are not covered here\.

## Input and output chaining<a name="document-chaining"></a>

The AWSTOE configuration management application provides a feature for chaining inputs and outputs by writing references in the following formats:

`{{ phase_name.step_name.inputs/outputs.variable }}`

or

`{{ phase_name.step_name.inputs/outputs[index].variable }}`

The chaining feature allows you to recycle code and improve the maintainability of the document\.

The usage requirements of chaining are as follows:
+ Chaining expressions can be used only in the inputs section of each step\.
+ Statements with chaining expressions must be enclosed in quotes\. For example:
  + **Invalid expression**: `echo {{ phase.step.inputs.variable }}`
  + **Valid expression**: `"echo {{ phase.step.inputs.variable }}"`
  + **Valid expression**: `'echo {{ phase.step.inputs.variable }}'`
+ Chaining expressions can reference variables from other steps and phases in the same document\. 
+ Indexes in chaining expressions follow 0\-based indexing \(first index is 0\)\.

**Examples**

To refer to the source variable in the second entry of the following example step, the chaining pattern is `{{ build.SampleS3Download.inputs[1].source }}`\.

```
phases:
  -
    name: 'build'
    steps:
      -
        name: SampleS3Download
        action: S3Download
        timeoutSeconds: 60
        onFailure: Abort
        maxAttempts: 3
        inputs:
          -
            source: 's3://sample-bucket/sample1.ps1'
            destination: 'C:\sample1.ps1'
          -
            source: 's3://sample-bucket/sample2.ps1'
            destination: 'C:\sample2.ps1'
```

To refer to the output variable \(equal to "Hello"\) of the following example step, the chaining pattern is `{{ build.SamplePowerShellStep.outputs.stdout }}`\.

```
phases:
  -
    name: 'build'
    steps:
      -
        name: SamplePowerShellStep
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          commands:
            - 'Write-Host "Hello"'
```

## Document schema and definitions<a name="document-schema"></a>

The following is the YAML schema for a document\.

```
name: (optional)
description: (optional)
schemaVersion: "string"

phases:
  - name: "string"
    steps:
        - name: "string"
          action: "string"
          timeoutSeconds: integer
          onFailure: "Abort|Continue|Ignore"
          maxAttempts: integer
          inputs:
```

The schema definitions for a document are as follows\.


| Field | Description | Type | Required | 
| --- | --- | --- | --- | 
| name | Name of the document\. | String | No | 
| description | Description of the document\. | String |  No  | 
| schemaVersion | Schema version of the document, currently 1\.0\. | String |  Yes  | 
| phases | A list of phases with their steps\. |  List  |  Yes  | 

The schema definitions for a phase are as follows\.


| Field | Description | Type | Required | 
| --- | --- | --- | --- | 
| name | Name of the phase\. | String | Yes | 
| steps | List of the steps in the phase\. | List  |  Yes  | 

The schema definitions for a step are as follows\.


| Field | Description | Type | Required | Default value | 
| --- | --- | --- | --- | --- | 
| name | User\-defined name for the step\. | String |  |  | 
| action | Keyword pertaining to the module that runs the step\. | String |    |  | 
| timeoutSeconds |  Number of seconds that the step runs before failing or retrying\.  Also, supports \-1 value, which indicates infinite timeout\. 0 and other negative values are not allowed\.  | Integer |  Yes  | 7,200 sec \(120 mins\) | 
| onFailure |  Specifies what the step should do in case of failure\. Valid values are as follows:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-use-documents.html)  |  String  |  Yes  | Abort | 
| maxAttempts | Maximum number of attempts allowed before failing the step\. | Integer |  No  | 1 | 
| inputs | Contains parameters required by the action module to run the step\. | Dict |  Yes  |  | 

## Document example schemas<a name="document-example"></a>

The following is an example document schema to install all available Windows updates, run a configuration script, validate the changes before the AMI is created, and test the changes after the AMI is created\.

```
name: RunConfig_UpdateWindows
description: 'This document will install all available Windows updates and run a config script. It will then validate the changes before an AMI is created. Then after AMI creation, it will test all the changes.'
schemaVersion: 1.0
phases:
  -
    name: build
    steps:
      -
        name: DownloadConfigScript
        action: S3Download
        timeoutSeconds: 60
        onFailure: Abort
        maxAttempts: 3
        inputs:
          -
            source: 's3://customer-bucket/config.ps1'
            destination: 'C:\config.ps1'
      -
        name: RunConfigScript
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          file: '{{build.DownloadConfigScript.inputs[0].destination}}'
      -
        name: Cleanup
        action: DeleteFile
        onFailure: Abort
        maxAttempts: 3
        inputs:
          - path: '{{build.DownloadConfigScript.inputs[0].destination}}'
      -
        name: RebootAfterConfigApplied
        action: Reboot
        inputs:
          delaySeconds: 60
      -
        name: InstallWindowsUpdates
        action: UpdateOS
  -
    name: validate
    steps:
      -
        name: DownloadTestConfigScript
        action: S3Download
        timeoutSeconds: 60
        onFailure: Abort
        maxAttempts: 3
        inputs:
          -
            source: 's3://customer-bucket/testConfig.ps1'
            destination: 'C:\testConfig.ps1'
      -
        name: ValidateConfigScript
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          file: '{{validate.DownloadTestConfigScript.inputs[0].destination}}'
      -
        name: Cleanup
        action: DeleteFile
        onFailure: Abort
        maxAttempts: 3
        inputs:
          - path: '{{validate.DownloadTestConfigScript.inputs[0].destination}}'
  -
    name: test
    steps:
      -
        name: DownloadTestConfigScript
        action: S3Download
        timeoutSeconds: 60
        onFailure: Abort
        maxAttempts: 3
        inputs:
          -
            source: 's3://customer-bucket/testConfig.ps1'
            destination: 'C:\testConfig.ps1'
      -
        name: ValidateConfigScript
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          file: '{{test.DownloadTestConfigScript.inputs[0].destination}}'
```

The following is an example document schema to download and run a custom Linux binary file\.

```
name: LinuxBin
description: Download and run a custom Linux binary file.
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: Download
        action: S3Download
        inputs:
          - source: s3://mybucket/myapplication
            destination: /tmp/myapplication
      - name: Enable
        action: ExecuteBash
        onFailure: Continue
        inputs:
          commands:
            - 'chmod u+x {{ build.Download.inputs[0].destination }}'
      - name: Install
        action: ExecuteBinary
        onFailure: Continue
        inputs:
          path: '{{ build.Download.inputs[0].destination }}'
          arguments:
            - '--install'
      - name: Delete
        action: DeleteFile
        inputs:
          - path: '{{ build.Download.inputs[0].destination }}'
```

The following is an example document schema to install the AWS CLI using the setup file\.

```
name: InstallCLISetUp
description: Install AWS CLI using the setup file
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: Download
        action: S3Download
        inputs:
          - source: s3://aws-cli/AWSCLISetup.exe
            destination: C:\Windows\temp\AWSCLISetup.exe
      - name: Install
        action: ExecuteBinary
        onFailure: Continue
        inputs:
          path: '{{ build.Download.inputs[0].destination }}'
          arguments:
            - '/install'
            - '/quiet'
            - '/norestart'
      - name: Delete
        action: DeleteFile
        inputs:
          - path: '{{ build.Download.inputs[0].destination }}'
```

The following is an example document schema to install the AWS CLI using the MSI installer\.

```
name: InstallCLIMSI
description: Install AWS CLI using the MSI installer
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: Download
        action: S3Download
        inputs:
          - source: s3://aws-cli/AWSCLI64PY3.msi
            destination: C:\Windows\temp\AWSCLI64PY3.msi
      - name: Install
        action: ExecuteBinary
        onFailure: Continue
        inputs:
          path: 'C:\Windows\System32\msiexec.exe'
          arguments:
            - '/i'
            - '{{ build.Download.inputs[0].destination }}'
            - '/quiet'
            - '/norestart'
      - name: Delete
        action: DeleteFile
        inputs:
          - path: '{{ build.Download.inputs[0].destination }}'
```