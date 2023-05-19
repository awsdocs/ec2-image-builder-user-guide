# Patch Management inEC2 Image Builder<a name="security-patch-management"></a>

EC2 Image Builder provides the latest Amazon Linux 2, Red Hat Enterprise Linux \(RHEL\), CentOS, Ubuntu, SUSE Linux Enterprise Server, and Windows 2012 R2 and later AMIs as managed image sources\. You maintain the Amazon EC2 system patching responsibility, per the [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/)\. If the EC2 instances in your application workload can be easily replaced, then it might be more efficient to update the base AMI and redeploy all compute nodes based on this image\. 

The following are two ways you can keep your Image Builder AMIs up to date\.
+ **AWS\-provided patching components** – EC2 Image Builder provides two build components, `update-linux` and `update-windows`,which install all pending operating system updates\. These components use the `UpdateOS` action module\. For more information, see [UpdateOS](toe-action-modules.md#action-modules-updateos)\. The components can be added to your image build pipelines by selecting them from the list of AWS\-provided components\.
+ **Custom build components with patching operations** – To selectively install or update patches on operating systems of supported AMIs, you can author an Image Builder component to install the required patches\. A custom component can install patches using shell scripts \(Bash or PowerShell\), or it can use the `UpdateOS` action module to specify patches for installation or exclusion\. For more information, see [Action modules supported by AWSTOE component manager](toe-action-modules.md)\.

  Component that uses the `UpdateOS` action module \(Linux and Windows\)

  ```
  schemaVersion: 1.0
  phases:
    - name: build
      steps:
        - name: UpdateOS
          action: UpdateOS
  ```

  Component that uses Bash to install yum updates

  ```
  schemaVersion: 1.0
  phases:
    - name: build
      steps:
        - name: InstallYumUpdates
          action: ExecuteBash
          inputs:
            commands:
              - sudo yum update -y
  ```