# Define and reference variables in the AWSTOE application<a name="image-builder-component-manager-user-defined-variables"></a>

Variables provide a way to label data with meaningful names that can be used throughout a program\. You can define custom variables with simple and readable formats for complex workflows, and reference them in an AWSTOE document\. 

This section provides information to help you define constants that can be referenced from an AWSTOE application document, including syntax, name constraints, and examples\.

## Constants<a name="image-builder-component-manager-user-defined-variables-constants"></a>

Constants are immutable variables that cannot be modified or overridden once defined\. Constants can be defined using values in the `constants` section of an AWSTOE document\.

**Constant name constraints**
+ **Minimum length**: 3 characters\.
+ **Maximum length**: 128 characters\.
+ Must contain at least one English letter \(`a`\-`z`, `A`\-`Z`\)\.
+ Cannot contain special characters, with the exception of "`-`" or "`_`"\.
+ Must be a unique name within the document\.

**Syntax**

```
constants:
  - <name>:
      type: <constant type>
      value: <constant value>
```


| Key name | Required | Description | 
| --- | --- | --- | 
|  `name`  |  Yes  | Name of the constant\. Must be unique name in document \(including names of parameters\)\. | 
| `value` | Yes | Value of the constant\. | 
| `type` | Yes | Type of the constant\. Supported type is string\. | 

**Examples**  
Referencing constants in step inputs

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

Referencing constants in loop inputs

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