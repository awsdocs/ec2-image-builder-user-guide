# awstoe validate command<a name="cmd-validate"></a>

When you run this command, it validates the YAML document syntax for each of the component documents specified by the `--documents` parameter\.

## Syntax<a name="validate-syntax"></a>

```
awstoe validate [--document-s3-bucket-owner <owner>] 
      --documents <file path,file path,...> [--help] [--trace]
```

## Parameters and options<a name="validate-parameters"></a>Parameters

**\-\-document\-s3\-bucket\-owner**  
Short form: N/A  
Source account ID of S3 URI\-based documents provided\.

**\-\-documents *`./doc-1.yaml`,`./doc-n.yaml`***  
Short form: \-d *`./doc-1.yaml`*,*`./doc-n`*  
The component documents *\(required\)*\. This parameter contains a comma\-separated list of file locations for the YAML component documents to run\. Valid locations include:  
+ local file paths \(*\./component\-doc\-example\.yaml*\)
+ S3 URIs \(`s3://bucket/key`\)
+ Image Builder component build version ARNs \(arn:aws:imagebuilder:us\-west\-*2:123456789012*:component/*my\-example\-component*/2021\.12\.02/1\)
There are no spaces between items in the list, only commas\.Options

**\-\-help**  
Short form: \-h  
Displays a help manual for using the component management application options\.

**\-\-trace**  
Short form: \-t  
Enables verbose logging to the console\.