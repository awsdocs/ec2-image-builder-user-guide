# Use looping constructs in the awstoe application<a name="image-builder-looping-constructs"></a>

This section provides information to create looping constructs from the awstoe application\. Looping constructs define a repeated sequence of instructions\. You can use the following types of looping constructs in the awstoe application:
+ `for` constructs — Iterate over a bounded sequence of integers\.
+ `forEach` constructs
  + `forEach` loop with input list — Iterates over a finite collection of strings\. 
  + `forEach` loop with delimited list — Iterates over a finite collection of strings joined by a delimiter\.

**Note**  
Looping constructs support only string data types\.

**Topics**
+ [Reference iteration variables](#image-builder-looping-constructs-iteration-variables)
+ [Types of looping constructs](#image-builder-looping-constructs-types)
+ [Step fields](#image-builder-looping-constructs-step-fields)
+ [Step and iteration outputs](#image-builder-looping-constructs-step-output)

## Reference iteration variables<a name="image-builder-looping-constructs-iteration-variables"></a>

To refer to the index and value of the current iteration variable, the reference expression `{{ loop.* }}` must be used within the input body of a step that contains a looping construct\. This expression cannot be used to refer to the iteration variables of the looping construct of another step\.

The reference expression consists of the following members:
+ `{{ loop.index }}` — The ordinal position of the current iteration, which is indexed at `0`\. 
+ `{{ loop.value }}` — The value associated with the current iteration variable\. 

### Loop names<a name="image-builder-looping-constructs-iteration-variables-names"></a>

 All looping constructs have an optional name field for identification\. If a loop name is provided, it can be used to refer to iteration variables in the input body of the step\. To refer to the iteration indices and values of a named loop, use `{{ <loop_name>.* }}` with `{{ loop.* }}` in the input body of the step\. This expression cannot be used to refer to the named looping construct of another step\. 

The reference expression consists of the following members:
+ `{{ <loop_name>.index }}` — The ordinal position of the current iteration of the named loop, which is indexed at `0`\.
+ `{{ <loop_name>.value }}` — The value associated with the current iteration variable of the named loop\.

### Resolve reference expressions<a name="image-builder-looping-constructs-iteration-variables-expressions"></a>

The awstoe application resolves reference expressions as follows: 
+ `{{ <loop_name>.* }}` — awstoe resolves this expression using the following logic:
  + If the loop of the currently executing step matches the `<loop_name>` value, then the reference expression resolves to the looping construct of the currently executing step\.
  + `<loop_name>` resolves to the named looping construct if it appears within the currently executing step\.
+ `{{ loop.* }}` — awstoe resolves the expression using the looping construct defined in the currently executing step\.

If reference expressions are used within a step that does not contain a loop, then awstoe does not resolve the expressions and they appear in the step with no replacement\. 

**Note**  
Reference expressions must be enclosed in double quotes to be correctly interpreted by the YAML compiler\.

## Types of looping constructs<a name="image-builder-looping-constructs-types"></a>

This section provides information and examples about looping construct types that can be used in the awstoe application\.

**Topics**
+ [`for` loop](#image-builder-looping-constructs-types-for)
+ [`forEach` loop with input list](#image-builder-looping-constructs-types-foreach)
+ [`forEach` loop with delimited list](#image-builder-looping-constructs-types-foreach-delimited)

### `for` loop<a name="image-builder-looping-constructs-types-for"></a>

The `for` loop iterates on a range of integers specified within a boundary outlined by the start and end of the variables\. The iterating values are in the set `[start, end]` and includes boundary values\.

awstoe verifies the `start`, `end`, and `updateBy` values to ensure that the combination does not result in an infinite loop\.

`for` loop schema

```
name: "StepName"
action: "ActionModule"
loop:
  name: "string"
  for:
    start: int
    end: int
    updateBy: int
  inputs:
    ...
```


**`for` input**  

| Field | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
|  `name`  | Unique name of the loop\. It must be unique compared to other loop names in the same phase\. |  String  |  No  |  ""  | 
|  `start`  | Starting value of iteration\. Does not accept chaining expressions\.  |  Integer  |  Yes  |  n/a  | 
| end | Ending value of iteration\. Does not accept chaining expressions\.  | Integer | Yes | n/a | 
| updateBy | Difference by which an iterating value is updated through addition\. It must be a negative or positive non\-zero value\. Does not accept chaining expressions\.  | Integer | Yes | n/a | 

`for` loop input example

```
name: "CalculateFileUploadLatencies"
action: "ExecutePowerShell"
loop:
  for:
    start: 100000
    end: 1000000
    updateBy: 100000
inputs:
  commands:
    - |
      $f = new-object System.IO.FileStream c:\temp\test{{ loop.index }}.txt, Create, ReadWrite
      $f.SetLength({{ loop.value }}MB)
      $f.Close()
    - c:\users\administrator\downloads\latencyTest.exe --file c:\temp\test{{ loop.index }}.txt
    - aws s3 cp c:\users\administrator\downloads\latencyMetrics.json s3://bucket/latencyMetrics.json
    - |
      Remove-Item -Path c:\temp\test{{ loop.index }}.txt
      Remove-Item -Path c:\users\administrator\downloads\latencyMetrics.json
```

### `forEach` loop with input list<a name="image-builder-looping-constructs-types-foreach"></a>

The `forEach` loop iterates on an explicit list of values, which can be strings and chained expressions\. 

`forEach` schema

```
name: "StepName"
action: "ActionModule"
loop:
  name: "string"
  forEach:
    - "string"
  inputs:
    ...
```


**`forEach` input**  

| Field | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
|  `name`  | Unique name of the loop\. It must be unique compared to other loop names in the same phase\. |  String  |  No  |  ""  | 
|  List of strings of `forEach` loop  |  List of strings for iteration\. Accepts chained expressions as strings in the list\. Chained expressions must be enclosed by double quotes for the YAML compiler to correctly interpret them\.  |  List of strings  |  Yes  |  n/a  | 

`forEach` input example 1

```
name: "ExecuteCustomScripts"
action: "ExecuteBash"
loop:
  name: BatchExecLoop
  forEach:
    - /tmp/script1.sh
    - /tmp/script2.sh
    - /tmp/script3.sh
inputs:
  commands:
    - echo "Count {{ BatchExecLoop.index }}"
    - sh "{{ loop.value }}"
    - |
      retVal=$?
      if [ $retVal -ne 0 ]; then
        echo "Failed"
      else
        echo "Passed"
      fi
```

`forEach` input example 2

```
name: "RunMSIWithDifferentArgs"
action: "ExecuteBinary"
loop:
  name: MultiArgLoop
  forEach:
    - "ARG1=C:\Users ARG2=1"
    - "ARG1=C:\Users"
    - "ARG1=C:\Users ARG3=C:\Users\Administrator\Documents\f1.txt"
inputs:
  commands:
    path: "c:\users\administrator\downloads\runner.exe"
    args:
      - "{{ MultiArgLoop.value }}"
```

`forEach` input example 3

```
name: "DownloadAllBinaries"
action: "S3Download"
loop:
  name: MultiArgLoop
  forEach:
    - "bin1.exe"
    - "bin10.exe"
    - "bin5.exe"
inputs:
  -
    source: "s3://bucket/{{ loop.value }}"
    destination: "c:\temp\{{ loop.value }}"
```

### `forEach` loop with delimited list<a name="image-builder-looping-constructs-types-foreach-delimited"></a>

The loop iterates over a string containing values separated by a delimiter\. In order to iterate over the string’s constituents, TOE uses the delimiter to split the string into an array suitable for iteration\. 

`forEach` loop with delimited list schema

```
name: "StepName"
action: "ActionModule"
loop:
  name: "string"
  forEach:
    list: "string"
    delimiter: ".,;:\n\t -_"
  inputs:
    ...
```


**`forEach` loop with delimited list input**  

| Field | Description | Type | Required | Default | 
| --- | --- | --- | --- | --- | 
|  `name`  | Unique name given to the loop\. It should be unique when compared to other loop names in the same phase |  String  |  No  |  ""  | 
|  `list`  | A string that is composed of constituent strings joined by a common delimeter character\. Also accepts chained expressions\. In case of chained expressions, ensure that those are enclosed by double quotes for correct interpretation by the YAML compiler | String |  Yes  |  n/a  | 
| delimiter | Character used to separate out strings within a block\. Default is the comma character\. Only one delimeter character is allowed from the given list: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/imagebuilder/latest/userguide/image-builder-looping-constructs.html) Chaining expressions cannot be used\. | String | No | Comma: "," | 

**Note**  
The value of `list` is treated as an immutable string\. If the source of `list` is changed during runtime, it will not be reflected during execution\.

`forEach` loop with delimited list example 1

```
// Uses chaning expression ({{ <phase_name>.<step_name>.inputs/outputs.<var_name> }})
// to refer to another step's input/output variables for code re-use.
name: "RunMSIs"
action: "ExecuteBinary"
loop:
  forEach:
    list: "{{ build.GetAllMSIPathsForInstallation.outputs.stdout }}"
    delimiter: "\n"
inputs:
  commands:
    path: "{{ loop.value }}"
```

`forEach` loop with delimited list example 2

```
name: "UploadMetricFiles"
action: "S3Upload"
loop:
  forEach:
    list: "/tmp/m1.txt,/tmp/m2.txt,/tmp/m3.txt,..."
inputs:
  commands:
    -
      source: "{{ loop.value }}"
      destination: "s3://bucket/key/{{ loop.value }}"
```

## Step fields<a name="image-builder-looping-constructs-step-fields"></a>

Loops are part of a step\. Any field related to the execution of a step is not applied to individual iterations\. Step fields apply only at the step level as follows:
+ *timeoutSeconds* — All iterations of the loop must be executed within the time period specified by this field\. If loop execution times out, then awstoe runs the retry policy of the step and resets the timeout parameter for each new attempt\. If loop execution exceeds the timeout value after reaching the maximum number of retries, the failure message of the step states that loop execution had timed out\. 
+ *onFailure* — Failure handling is applied to the step:
  + If *onFailure* is set to `Abort`, then a failed iteration causes awstoe to exit the loop to run the retry policy\. If the step fails after reaching the maximum number of attempts, awstoe aborts execution of its process after the current step is marked for failure\.
  + If *onFailure* is set to `Continue`, then all iterations run regardless of failures and awstoe runs the retry policy of the step\. If the step fails after reaching the maximum number of attempts, awstoe continues to execute the step after the current step, and the global execution status is marked for failure\.
+ *maxAttempts * — For every retry, the entire step and all iterations are executed from scratch\.
+ *status* — The overall status of the execution of a step\.`status` does not represent the status of individual iterations\. The status of a step with loops is determined as follows:
  + If a single iteration fails to execute, the status of a step points to a failure\.
  + If all iterations succeed, the status of a step points to a success\.
+ *startTime * — The overall start time of the execution of a step\. Does not represent the start time of individual iterations\.
+ *endTime * — The overall end time of the execution of a step\. Does not represent the end time of individual iterations\.
+ *failureMessage * — Includes the iteration indices that failed in case of non\-timeout errors\. In case of timeout errors, the message states that the loop execution has failed\. Individual error messages for each iteration are not provided in order to minimize the size of failure messages\.

## Step and iteration outputs<a name="image-builder-looping-constructs-step-output"></a>

Every iteration contains an output\. At the end of loop execution, awstoe consolidates all successful iteration outputs in `detailedOutput.json`\. The consolidated outputs are a collation of values that belong to the corresponding output keys as defined in the output schema of the action module\. The following example shows how the outputs are consolidated:

**Output of `ExecuteBash` for Iteration 1**

```
[{"stdout":"Hello"}]
```

**Output of `ExecuteBash` for Iteration 2**

```
[{"stdout":"World"}]
```

**Output of `ExecuteBash` for Step**

```
[{"stdout":"Hello\nWorld"}]
```

For example, `ExecuteBash`, `ExecutePowerShell`, and `ExecuteBinary` are action modules which return `STDOUT` as the action module output\. `STDOUT` messages are joined with the new line character to produce the overall output of the step in `detailedOutput.json`\.

awstoe will not consolidate the outputs of unsuccessful iterations\.
