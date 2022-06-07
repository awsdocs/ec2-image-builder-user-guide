# Create a YAML component document<a name="create-component-yaml"></a>

To build a component, you must provide a YAML application component document, which represents the phases and steps to create the component\.

The examples in this section create a build component that calls the `UpdateOS` action module in the AWSTOE component management application\. The module updates the operating system\. For more information about the `UpdateOS` action module, see [UpdateOS](toe-action-modules.md#action-modules-updateos)\. For more information about the phases, steps, and syntax for AWSTOE YAML application component documents, see [Use documents in AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-use-documents.html)\.

**Note**  
Image Builder determines the component types in the pipeline workflow\. This workflow corresponds to the *Build stage* and the *Test stage* in the build process\. Image Builder determines the component type as follows:  
**Build** – This is the default component type\. Anything that is not classified as a test component, is a build component\. This type of component runs during the *Build stage*\. If this component has a `test` phase defined, that phase runs during the *Test stage*\.
**Test** – To qualify as a test component, the component document must include only one phase, named `test`\. For tests that are related to build component configurations, we recommend that you don't use a standalone test component\. Rather, use the `test` phase in the associated build component\.
For more information about how Image Builder uses stages and phases to manage component workflow in its build process, see [Manage components with AWSTOE](manage-components.md)\.

To create a YAML application component document for a sample application, follow the steps on the tab that matches your image operating system\. 

------
#### [ Linux ]

**Create a YAML component file**  
Use a file editing tool to create a file named `update-linux-os.yaml`\. Include the following content:

```
# Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of this
# software and associated documentation files (the "Software"), to deal in the Software
# without restriction, including without limitation the rights to use, copy, modify,
# merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
# INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
# PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
name: update-linux-os
description: Updates Linux with the latest security updates.
schemaVersion: 1
phases:
  - name: build
    steps:
    - name: UpdateOS
      action: UpdateOS
# Document End
```

**Tip**  
Use a tool like this online [YAML Validator](https://jsonformatter.org/yaml-validator), or a YAML lint extension in your code environment to verify that your YAML is well\-formed\.

------
#### [ Windows ]

**Create a YAML component file**  
Use a file editing tool to create a file named `update-windows-os.yaml`\. Include the following content:

```
# Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
# SPDX-License-Identifier: MIT-0
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of this
# software and associated documentation files (the "Software"), to deal in the Software
# without restriction, including without limitation the rights to use, copy, modify,
# merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
# INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
# PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
# HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
name: update-windows-os
description: Updates Windows with the latest security updates.
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: UpdateOS
        action: UpdateOS
# Document End
```

**Tip**  
Use a tool like this online [YAML Validator](https://jsonformatter.org/yaml-validator), or a YAML lint extension in your code environment to verify that your YAML is well\-formed\.

------