# Amazon CloudWatch Logs integration in Image Builder<a name="integ-cwlogs"></a>

CloudWatch Logs support is turned on by default\. Logs are retained on the instance during the build process, and streamed to CloudWatch Logs\. The instance logs are removed from the instance before image creation\.

Build logs are streamed to following the Image Builder CloudWatch Logs group and stream:
+ LogGroup: `"/aws/imagebuilder/<ImageName>`
+ LogStream: `<ImageVersion>/<ImageBuildVersion>["x.x.x/x"]`

You can opt out of CloudWatch Logs streaming by removing the following permissions associated with the instance profile\.

```
"Statement": [
    {
        "Effect": "Allow",
        "Action": [
            "logs:CreateLogStream",
            "logs:CreateLogGroup",
            "logs:PutLogEvents"
        ],
        "Resource": "arn:aws:logs:*:*:log-group:/aws/imagebuilder/*"
    }
]
```

For advanced troubleshooting, you can run predefined commands and scripts using [AWS Systems Manager Run Command](https://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html)\. For more information, see [Troubleshoot EC2 Image Builder](troubleshooting.md)\.