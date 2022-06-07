# Run your AMI image pipeline<a name="pipelines-run"></a>

If you chose the manual schedule option for your pipeline, it will only run when you manually kick off the build\. If you chose one of the automatic scheduling options, you can also run it manually, in between regularly scheduled runs\. For example, if you have a pipeline that normally runs once a month, but you need to incorporate an update to one of your components two weeks after the prior run, you can choose to run your pipeline manually\.

------
#### [ Console ]

To run your pipeline from the pipeline details page in the Image Builder console, choose **Run pipeline** from the **Actions** menu at the top of the page\. A status message appears at the top of the page to notify you that your pipeline has started, or if there is an error\.

1. In the upper left corner of the pipeline details page, choose **Run pipeline**, from the **Actions** menu\.

1. You can see the current status of your pipeline on the **Output images** tab, in the **Status** column\.

------
#### [ AWS CLI ]

The following example shows how to use the start\-image\-pipeline\-execution command in the AWS CLI to start an image pipeline manually\. When you run this command in the pipeline, you create a new image on demand\.

```
aws imagebuilder start-image-pipeline-execution --image-pipeline-arn arn:aws:imagebuilder:us-west-2:123456789012:image-pipeline/my-example-pipeline
```

To see what resources are created when the build pipeline runs, see [Resources created](how-image-builder-works.md#image-builder-resources)\.

------