# Define and reference variables in AWS TOE<a name="image-builder-component-manager-user-defined-variables"></a>

Variables provide a way to label data with meaningful names that can be used throughout an application\. You can define custom variables with simple and readable formats for complex workflows, and reference them in the YAML application component document for an AWS TOE component\.

This section provides information to help you define variables for your AWS TOE component in the YAML application component document, including syntax, name constraints, and examples\.

## Parameters<a name="user-defined-vars-parameters"></a>

Parameters are mutable variables, with settings that the calling application can provide at runtime\. You can define parameters in the `Parameters` section of the YAML document\.

**Rules for parameter names**
+ The name must be between 3 and 128 characters in length\.
+ The name can only contain alphanumeric characters \(a\-z, A\-Z, 0\-9\), dashes \(\-\), or underscores \(\_\)\.
+ The name must be unique within the document\.
+ The name must be specified as a YAML string\.

### Syntax<a name="w54aac14c35b7b7"></a>

```
parameters:
  - <name>:
      type: <parameter type>
      default: <parameter value>
      description: <parameter description>
```


| Key name | Required | Description | 
| --- | --- | --- | 
| `name` | Yes | The name of the parameter\. Must be unique for the document \(it must not be the same as any other parameter names or constants\)\. | 
| `type` | Yes | The data type of the parameter\. Supported types include: `string`\. | 
| `default` | No | The default value for the parameter\. | 
| `description` | No | Describes the parameter\. | 

### Reference parameter values in a document<a name="w54aac14c35b7b9"></a>

You can reference parameters in step or loop inputs inside of your YAML document, as follows:
+ Parameter references are case\-sensitive, and the name must match exactly\.
+ The name must be enclosed within double curly braces `{{` *MyParameter* `}}`\.
+ Spaces are allowed within the curly braces, and are automatically trimmed\. For example, all of the following references are valid:

  `{{ MyParameter }}`, `{{ MyParameter}}`, `{{MyParameter }}`, `{{MyParameter}}`
+ The reference in the YAML document must be specified as a string \(enclosed in single or double quotes\)\.

  For example: `- {{ MyParameter }}` is not valid, as it is not identified as a string\.

  However, the following references are both valid: `- '{{ MyParameter }}'` and `- "{{ MyParameter }}"`\.

**Examples**  
The following examples show how to use parameters in your YAML document:
+ Refer to a parameter in step inputs:

  ```
  name: Download AWS CLI version 2
  schemaVersion: 1.0
  parameters:
    - Source:
        type: string
        default: 'https://awscli.amazonaws.com/AWSCLIV2.msi'
        description: The AWS CLI installer source URL.
  phases:
    - name: build
      steps:
        - name: Download
          action: WebDownload
          inputs:
            - source: '{{ Source }}'
              destination: 'C:\Windows\Temp\AWSCLIV2.msi'
  ```
+ Refer to a parameter in loop inputs:

  ```
  name: PingHosts
  schemaVersion: 1.0
  parameters:
    - Hosts:
        type: string
        default: 127.0.0.1,amazon.com
        description: A comma separated list of hosts to ping.
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

### Override parameters at runtime<a name="w54aac14c35b7c11"></a>

You can use the `--parameters` option from the AWS CLI with a key/value pair to set a parameter value at runtime\.
+ Specify the parameter key/value pair as the name and value, separated by an equals sign \(<name>=<value>\)\.
+ Multiple parameters must be separated by a comma\.
+ Parameter names that are not found in the YAML component document are ignored\.
+ The parameter name and value are both required\.

#### Syntax<a name="w54aac14c35b7c11b7"></a>

```
--parameters name1=value1,name12=value2...
```


| CLI option | Required | Description | 
| --- | --- | --- | 
| \-\-parameters *name*=*value*,\.\.\. | No | This option takes list of key/value pairs, with the parameter name as the key\. | 

**Examples**  
The following examples show how to use parameters in your YAML document:
+ The parameter key/value pair specified in this `--parameter` option is not valid:

  ```
  --parameters ntp-server=
  ```
+ Set one parameter key/value pair with the `--parameter` option in the AWS CLI:

  ```
  --parameters ntp-server=ntp-server-windows-qe.us-east1.amazon.com
  ```
+ Set multiple parameter key/value pairs with the `--parameter` option in the AWS CLI:

  ```
  --parameters ntp-server=ntp-server.amazon.com,http-url=https://internal-us-east1.amazon.com
  ```

## Constants<a name="image-builder-component-manager-user-defined-variables-constants"></a>

Constants are immutable variables that cannot be modified or overridden once defined\. Constants can be defined using values in the `constants` section of an AWS TOE document\.

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