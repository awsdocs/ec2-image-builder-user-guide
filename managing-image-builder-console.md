# Managing and Running Images Using the EC2 Image Builder Console<a name="managing-image-builder-console"></a>

This section contains information to help you manage and run images with Amazon Elastic Compute Cloud Image Builder, using the Image Builder console\.

**Topics**
+ [Edit Configuration Details and Additional Settings](#image-builder-configuration-details)
+ [Delete Pipeline](#image-builder-delete-pipeline)
+ [Create New Component](#image-builder-create-component)
+ [Working with Image Recipes](#image-builder-recipes)
+ [Testing](#image-builder-testing)
+ [Distribution](#image-builder-distribution)
+ [Using Documents](#image-builder-application-documents)
+ [Compliance](#image-builder-compliance)
+ [AMI Confidentiality Protection](#image-builder-confidentiality)

## Edit Configuration Details and Additional Settings<a name="image-builder-configuration-details"></a>

After you have created an image pipeline, you can edit its configuration details and additional settings\. To edit the configuration details and additional settings, use the following steps\. 

1. To edit configuration details, including description, build schedule, or infrastructure details, navigate to the **Image pipelines** page and select the check box next to the name of the pipeline that you want to edit\. Then select the **View details** button\. 

1. On the **Image pipeline details** page, note the **Summary** details of the image pipeline, and the **Output image**, **Configuration**, and **Image recipe** tabs\.

1. Under the **Configuration** tab, select **Edit** next to **Configuration details**\. 

1. On the **Edit configuration details** page, you can edit your configuration\. Then select **Save changes**\. 

1. To edit additional settings, including associated licenses, AMI distribution settings, image export settings, and SNS notification settings, navigate to the **Image pipelines** page and select the check box next to the name of the pipeline that you want to edit\. Select **View details**\. 

1. On the **Image pipeline detail** page, under the **Configuration** tab, select **Edit** next to **Additional settings**\. 

1. On the **Edit additional settings** page, you can edit your configuration\. Then select **Save changes**\.

After an image recipe has been created, its components cannot be modified or replaced\. If you want to update the components in an image recipe, create a new recipe or recipe version\. 

## Delete Pipeline<a name="image-builder-delete-pipeline"></a>

To delete an image pipeline, navigate to the **Image pipelines** page and select the check box next to the name of the pipeline that you want to delete\. From the Actions dropdown, select **Delete**\. You will be prompted to confirm deletion of the pipeline by entering **Delete** in the text box and selecting **Delete**\. When you delete your pipeline, its associated resources are disassociated and the configuration is deleted\. 

## Create New Component<a name="image-builder-create-component"></a>

You can create components to add to your image recipes\. Components include build components and test components\. Build components are orchestration documents that define a sequence of steps for downloading, installing, and configuring software packages\. They also define validation and security hardening steps\. Test components are orchestration documents that define tests to run on software packages\. A component is defined using a YAML document format\. After an image recipe has been created, its components cannot be modified or replaced\. To update components after an image recipe has been created you must create a new recipe or recipe version\. To create a new component, use the following steps\.

1. Select **Components** in the left navigation pane\. Then select **Create component**\.

1. On the **Create component** page, under **Component details**, enter the following:
   + **Image Operating system \(OS\)**\. Specify the OS with which the component is compatible\. 
   + **Component category**\. From the dropdown, select the type of build or test component you are creating\. 
   + **Component name**\. Enter a name for the component\.
   + **Component version**\. Enter the version number of the component\. 
   + **Description**\. Provide an optional description to help you identify the component\.
   + **Change description**\. Provide an optional description to help you understand the changes made to this version of the component\. 

1. Under **Definition document**, which is the document that defines the actions that Image Builder performs on your image, enter the document content in YAML format in the provided field\. You can optionally use the AWS\-provided example \(auto\-filled when you select **Use example**\) and edit the content inline\. 

1. After you have entered the component details, select **Create component**\. When you [create a recipe](#create-recipe-version) or create a pipeline, the component will be available in the **Component** dropdown\.

1. To delete a component, from the **Components** page, select the check box next to the component that you want to delete\. From the **Actions** dropdown, select **Delete component**\. 

You can create a new component version by selecting the check box next to the component and, under the **Actions** dropdown, select **Create new version**\. This will take you to the **Create Component** page, where you can create a new component version\. 

## Working with Image Recipes<a name="image-builder-recipes"></a>

After you have created an image recipe, you can manage it from the **Recipes** page in the EC2 Image Builder console\. You can either delete an image recipe or create a new version of a recipe\. 

### Delete an Image Recipe<a name="delete-recipe"></a>

To delete an image recipe, select the check box next to the image recipe and, under the **Actions** dropdown, select **Delete recipe**\. When you delete an image recipe, images and components associated with the image recipe are disassociated, and the configuration is deleted\. A dialog box prompts you to confirm the deletion by entering **delete**\. 

### Create a New Image Recipe Version<a name="create-recipe-version"></a>

To create a new image recipe version, select the check box next to the image recipe and, under the **Actions** dropdown, select **Create new version**\. This takes you to the **Create Recipe **page, where you can create a new image recipe version\. For instructions for creating an image recipe, see the steps under [Build and Automate an OS Image Deployment Using the EC2 Image Builder Console](getting-started-image-builder.md#image-builder-image-deployment-console)\.

## Testing<a name="image-builder-testing"></a>

Generally, each test consists of a test script, a test binary, and test metadata\. The test script contains the orchestration commands to start the test binary, which can be written in any language supported by the OS\. Exit status codes indicate the test outcome\. Test metadata describes the test and its behavior \(for example, the name, description, paths to test binary, and expected duration\)\.

To update the tests in an image recipe using the EC2 Image Builder console, follow the steps to [create a new recipe version](#create-recipe-version), and then update the **Test Components **under **Components**\. 

## Distribution<a name="image-builder-distribution"></a>

EC2 Image Builder integrates with AWS Organizations and AWS Resource Access Manager to enable sharing of AMIs across AWS accounts\. From the Image Builder console, you can choose AMI launch permissions to control which AWS accounts are permitted to launch EC2 instances with the created AMI\. For example, you can make the image private, public, or share with specific accounts\. You can also use your AWS Organizations master account to enforce limitations on member accounts to launch instances only with approved and compliant AMIs\. For more information, see [Managing the AWS Accounts in Your Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts.html)\.

To update your distribution settings using the EC2 Image Builder console, follow the steps to [create a new recipe version](#create-recipe-version), and on the **Configure additional settings** page, update the **AWS Regions** and/or **Launch permissions** under **AMI Distribution settings**\. 

## Using Documents<a name="image-builder-application-documents"></a>

To build a component, you must provide a YAML\-based document, which represents the phases and steps to create the component\.

### Document Sections<a name="document-sections"></a>

The sections of a document are as follows\.
+ **Phases**\. Phases are a logical grouping of steps\.
  + Each phase name must be unique within a document\.
  + You can define many phases in a document\.
  + Image Builder executes phases called build, validate, and test in the image build pipeline\.
+ **Steps**\. Steps are individual units of work that comprise the workflow for each phase\.
  + Each step must define the action to take\.
  + Each step must have a unique name per phase\.
  + Steps are executed sequentially\.
  + Both the input and output of a step can be used as inputs for a subsequent step \(“chaining”\)\. 
  + Each step uses an action module that returns an exit code\. 
+ **Supported Actions**\. Supported actions are the actions that each step must contain in a document\. Each supported action correlates to an action module\. For a complete list of supported action modules, which includes functionality details and input/output values and examples, see [Supported Action Modules](image-builder-action-modules.md)\.
+ **Output Files**\. The configuration management application creates the following output files for each execution:
  + **detailedOutput\.json**: A file that describes all of the detailed information about the orchestration\. Contains information about each phase, step, and the action that is executed\. 
  + **document\.yaml**: The file that is sent to the application for the execution\. Stored as an artifact of the execution\. 
  + **console\.log**: Contains all of the standard out \(stdout\) and standard error \(stderr\) information captured during the execution\. 
  + **application\.log**: Contains the logs generated by debugging executions\. 

### Input and Output Chaining<a name="document-chaining"></a>

The configuration management application provides a feature for chaining inputs and outputs by writing references in the following formats:

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
            destination: 'C:\Temp\sample1.ps1'
          -
            source: 's3://sample-bucket/sample2.ps1'
            destination: 'C:\Temp\sample2.ps1'
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
            - 'echo "Hello"'
```

### Document Schema and Definitions<a name="document-schema"></a>

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
          onFailure: "Abort|Continue"
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
| action | Keyword pertaining to the module that executes the step\. | String |   |  | 
| timeoutSeconds |  Number of seconds for which the step runs before failing/retrying\.  Also, supports \-1 value, which indicates infinite timeout\. 0 and other negative values are not allowed\.  | Integer |  Yes  | 7,200 sec \(120 mins\) | 
| onFailure | Execution decision on failure\. The step can Abort or Continue to the next step\. |  String  |  Yes  | Abort | 
| maxAttempts | Maximum number of attempts allowed before failing the step\. | Integer |  No  | 1 | 
| inputs | Contains parameters required by the action module to execute the step\. | Dict |  Yes  |  | 

### Document Example Schemas<a name="document-example"></a>

The following is an example document schema to install all available Windows updates, execute a configuration script, validate the changes before the AMI is created, and test the changes after the AMI is created\.

```
name: RunConfig_UpdateWindows
description: 'This document will install all available Windows updates and execute a config script.  It will then validate the changes before an AMI is created.  Then after AMI creation, it will test all the changes.'
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
            destination: 'C:\Temp\config.ps1'
      -
        name: RunConfigScript
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          commands:
            - '{{build.DownloadConfigScript.inputs[0].destination}}'
      -
        name: Cleanup
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          commands:
            - 'Remove-Item {{build.DownloadConfigScript.inputs[0].destination}}'
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
            destination: 'C:\Temp\testConfig.ps1'
      -
        name: ValidateConfigScript
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          commands:
            - '{{validate.DownloadTestConfigScript.inputs[0].destination}}'
      -
        name: Cleanup
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          commands:
            - 'Remove-Item {{validate.DownloadTestConfigScript.inputs[0].destination}}'
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
            destination: 'C:\Temp\testConfig.ps1'
      -
        name: ValidateConfigScript
        action: ExecutePowerShell
        timeoutSeconds: 120
        onFailure: Abort
        maxAttempts: 3
        inputs:
          commands:
            - '{{test.DownloadTestConfigScript.inputs[0].destination}}'
```

The following is an example document schema to download and execute a custom Linux binary file\.

```
name: LinuxBin
description: Download and execute a custom Linux binary file.
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
        action: ExecuteBash
        inputs:
          commands:
            - 'rm {{ build.Download.inputs[0].destination }}'
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
        action: ExecutePowerShell
        inputs:
          commands:
            - Remove-Item -Path '{{ build.Download.inputs[0].destination }}' -Force
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
        action: ExecutePowerShell
        inputs:
          commands:
            - Remove-Item -Path '{{ build.Download.inputs[0].destination }}' -Force
```

## Compliance<a name="image-builder-compliance"></a>

For CIS, EC2 Image Builder uses Amazon Inspector to perform automatic assessments for exposure, vulnerabilities, and deviations from best practices and compliance standards\. For example, it assesses unintended network accessibility, unpatched CVEs, public internet connectivity, and remote root login enablement\. Amazon Inspector is offered as a test component that you can choose to add to your image recipe\. For more information about Amazon Inspector, see the [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html) User Guide\. For hardening, EC2 Image Builder validates using STIG\. For more information, see [Center for Internet Security \(CIS\) Benchmarks](url-doc-domain;inspector/latest/userguide/inspector_cis.html) and [Amazon EC2 Windows Server AMIs for STIG Compliance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ami-windows-stig.html)\.

## AMI Confidentiality Protection<a name="image-builder-confidentiality"></a>

AMIs that are backed by Amazon EBS snapshots can take advantage of EBS encryption for confidentiality protection\. Snapshots of both data and root volumes can be encrypted and attached to an AMI\. EC2 instances with encrypted volumes are launched from AMIs in the same way as instances without encrypted volumes\. Snapshots can be encrypted with keys from the AWS Key Management Service\. The customer master key \(CMK\) or a custom key that you specify can be used\. You must have permission to use the selected key\. If you have an AMI with encrypted snapshots, you can choose to re\-encrypt them with a different encryption key\. It is also possible to manually build an AMI with snapshots encrypted to multiple keys\. In addition, full\-disk encryption software can be used to encrypt volumes associated with the AMI\. For more information on EBS encryption, see the [EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html)\. 
