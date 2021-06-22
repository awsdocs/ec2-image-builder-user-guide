# Define and reference variables in AWSTOE<a name="image-builder-component-manager-user-defined-variables"></a>

Variables provide a way to label data with meaningful names that can be used throughout a program\. You can define custom variables with simple and readable formats for complex workflows, and reference them in an AWSTOE document\. 

This section provides information to help you define constants that can be referenced from an AWSTOE document, including syntax, name constraints, and examples\.

## Constants<a name="image-builder-component-manager-user-defined-variables-constants"></a>

Constants are immutable variables that cannot be modified or overridden once defined\. Constants can be defined using values in the `constants` section of an AWSTOE document\.

**Rules for constant names**
+ The name must be between 3 and 128 characters in length\.
+ The name can only contain alphanumeric characters \(a\-z, A\-Z, 0\-9\), dashes \(\-\), or underscores \(\_\)\.
+ The name must be unique within the document\.
+ The name must be specified as a YAML string\.

**Syntax**

```
constants:
  - <name>:
      type: <constant type>
      value: <constant value>
```


| Key name | Required | Description | 
| --- | --- | --- | 
|  `name`  |  Yes  | Name of the constant\. Must be unique for the document \(it must not be the same as any other parameter names or constants\)\. | 
| `value` | Yes | Value of the constant\. | 
| `type` | Yes | Type of the constant\. Supported type is string\. | 

**Reference constant values in a document**  
You can reference constants in step or loop inputs inside of your YAML document, as follows:
+ Constant references are case\-sensitive, and the name must match exactly\.
+ The name must be enclosed within double curly braces `{{` *MyConstant* `}}`\.
+ Spaces are allowed within the curly braces, and are automatically trimmed\. For example, all of the following references are valid:

  `{{ MyConstant }}`, `{{ MyConstant}}`, `{{MyConstant }}`, `{{MyConstant}}`
+ The reference in the YAML document must be specified as a string \(enclosed in single or double quotes\)\.

  For example: `- {{ MyConstant }}` is not valid, as it is not identified as a string\.

  However, the following references are both valid: `- '{{ MyConstant }}'` and `- "{{ MyConstant }}"`\.

**Examples**  
Constant referenced in step inputs

```
name: Download AWS CLI version 2
schemaVersion: 1.0
constants:
  - Source:
      type: string
      value: https://awscli.amazonaws.com/AWSCLIV2.msi
phases:
  - name: build
    steps:
      - name: Download
        action: WebDownload
        inputs:
          - source: '{{ Source }}'
            destination: 'C:\Windows\Temp\AWSCLIV2.msi'
```

Constant referenced in loop inputs

```
name: PingHosts
schemaVersion: 1.0
constants:
  - Hosts:
      type: string
      value: 127.0.0.1,amazon.com
phases:
  - name: build
    steps:
      - name: Ping
        action: ExecuteBash
        loop:
          forEach:
            list: '{{ Hosts }}'
            delimiter: ','
        inputs:
          commands:
            - ping -c 4 {{ loop.value }}
```