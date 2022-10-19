# Manage AWSTOE component parameters with EC2 Image Builder<a name="manage-component-parameters"></a>

You can manage AWSTOE components, including creating and setting component parameters, directly from the EC2 Image Builder console, or by using AWS CLI commands, or one of the Image Builder SDKs\. In this section, we'll cover creating and using parameters in your component, and setting component parameters through the Image Builder console and AWS CLI commands\.

**Important**  
Component parameters are plain text values, and are logged in AWS CloudTrail\. We recommend that you use AWS Secrets Manager or the AWS Systems Manager Parameter Store to store your secrets\. For more information about Secrets Manager, see [What is Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\. For more information about AWS Systems Manager Parameter Store, see [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) in the *AWS Systems Manager User Guide*\.

## Use parameters in your YAML component document<a name="component-params-yaml"></a>

To build a component, you must provide a YAML application component document, which represents the phases and steps to create the component\. The recipe that references the component can set the parameters to customize the values at runtime, with default values that take effect if the parameter is not set to a specific value\.

**Create a component document with input parameters**  
This section shows you how to define and use input parameters in your YAML component document\.

To create a YAML application component document that uses parameters and runs commands in your Image Builder build or test instances, follow the steps that match your image operating system:

------
#### [ Linux ]

**Create a YAML component document**  
Use a file editing tool to create a file named `hello-world-test.yaml`\. Include the following content:

```
# Document Start
# 
name: "HelloWorldTestingDocument-Linux"
description: "Hello world document to demonstrate parameters."
schemaVersion: 1.0
parameters:
  - MyInputParameter:
      type: string
      default: "It's me!"
      description: This is an input parameter.
phases:
  - name: build
    steps:
      - name: HelloWorldStep
        action: ExecuteBash
        inputs:
          commands:
            - echo "Hello World! Build phase. My input parameter value is {{ MyInputParameter }}"

  - name: validate
    steps:
      - name: HelloWorldStep
        action: ExecuteBash
        inputs:
          commands:
            - echo "Hello World! Validate phase. My input parameter value is {{ MyInputParameter }}"

  - name: test
    steps:
      - name: HelloWorldStep
        action: ExecuteBash
        inputs:
          commands:
            - echo "Hello World! Test phase. My input parameter value is {{ MyInputParameter }}"
# Document End
```

**Tip**  
Use a tool like this online [YAML Validator](https://jsonformatter.org/yaml-validator), or a YAML lint extension in your code environment to verify that your YAML is well\-formed\.

------
#### [ Windows ]

**Create a YAML component document**  
Use a file editing tool to create a file named `hello-world-test.yaml`\. Include the following content:

```
# Document Start
# 
name: "HelloWorldTestingDocument-Windows"
description: "Hello world document to demonstrate parameters."
schemaVersion: 1.0
parameters:
  - MyInputParameter:
      type: string
      default: "It's me!"
      description: This is an input parameter.
phases:
  - name: build
    steps:
      - name: HelloWorldStep
        action: ExecutePowerShell
        inputs:
          commands:
            - Write-Host "Hello World! Build phase. My input parameter value is {{ MyInputParameter }}"

  - name: validate
    steps:
      - name: HelloWorldStep
        action: ExecutePowerShell
        inputs:
          commands:
            - Write-Host "Hello World! Validate phase. My input parameter value is {{ MyInputParameter }}"

  - name: test
    steps:
      - name: HelloWorldStep
        action: ExecutePowerShell
        inputs:
          commands:
            - Write-Host "Hello World! Test phase. My input parameter value is {{ MyInputParameter }}"
# Document End
```

**Tip**  
Use a tool like this online [YAML Validator](https://jsonformatter.org/yaml-validator), or a YAML lint extension in your code environment to verify that your YAML is well\-formed\.

------

For more information about the phases, steps, and syntax for AWSTOE YAML application component documents, see [Use documents in AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-use-documents.html)\. For more information about parameters and their requirements, see the [Parameters](toe-user-defined-variables.md#user-defined-vars-parameters) section of the **Define and reference variables in AWSTOE** page\.

**Create a component from the YAML component document**  
Whatever method you use to create an AWSTOE component, the YAML application component document is always required as a baseline\.
+ To use the Image Builder console to create a component directly from your YAML document, see [Create a component using the Image Builder console](create-component-console.md)\.
+ To use Image Builder commands in the AWS CLI to create your component, see [Create AWSTOE components using Image Builder \(AWS CLI\)](create-components-cli.md#create-component-cli)\. Replace the YAML document name in those examples with the name of your Hello World YAML document \(`hello-world-test.yaml`\)\.

## Set component parameters in an Image Builder recipe \(console\)<a name="recipe-set-component-params"></a>

Setting component parameters works the same for image recipes and container recipes\. When you create a new recipe, or a new version of a recipe, you choose which components to include from the **Build components** and **Test components** lists\. The component lists include components that are applicable for the base operating system you chose for your image\.

After you select a component, it is displayed in the **Selected components** section, directly under the component lists\. Configuration options are shown for each component that is selected\. If your component has input parameters defined, they are displayed as an expandable section called **Input parameters**\.

The following parameter settings are shown for each parameter that's defined for your component:
+ **Parameter name** \(*not editable*\) – The name of the parameter\.
+ **Description** \(*not editable*\) – The parameter description
+ **Type** \(*not editable*\) – The data type for the parameter value\.
+ **Value** – The value for the parameter\. If you are using this component for the first time in this recipe, and a default value was defined for the component, the default value appears in the **Value** box with greyed\-out text\. If no other value is entered, AWSTOE uses the default value\.