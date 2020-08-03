# Verify the signature of the awstoe installation download<a name="awstoe-verify-sig"></a>

This section describes the recommended process for verifying the validity of the installation download for the awstoe application on Linux\- and Windows\-based operating systems\.

**Topics**
+ [Verify the signature of the awstoe installation download on Linux](#awstoe-verify-sig-linux)
+ [Verify the signature of the awstoe application installation download on Windows](#awstoe-verify-sig-win)

## Verify the signature of the awstoe installation download on Linux<a name="awstoe-verify-sig-linux"></a>

This topic describes the recommended process for verifying the validity of the installation download for the awstoe application on Linux\-based operating systems\. 

Whenever you download an application from the internet, we recommend that you authenticate the identity of the software publisher and check that the application is not altered or corrupted since it was published\. This protects you from installing a version of the application that contains a virus or other malicious code\.

If after running the steps in this topic, you determine that the software for the awstoe application is altered or corrupted, do NOT run the installation file\. Instead, contact AWS Support\.

awstoe application files for Linux\-based operating systems are signed using `GnuPG`, an open source implementation of the Pretty Good Privacy \(OpenPGP\) standard for secure digital signatures\. `GnuPG` \(also known as `GPG`\) provides authentication and integrity checking through a digital signature\. Amazon EC2 publishes a public key and signatures that you can use to verify the downloaded Amazon EC2 CLI tools\. For more information about `PGP` and `GnuPG` \(`GPG`\), see [http://www\.gnupg\.org](http://www.gnupg.org)\.

The first step is to establish trust with the software publisher\. Download the public key of the software publisher, check that the owner of the public key is who they claim to be, and then add the public key to your *keyring*\. Your keyring is a collection of known public keys\. After you establish the authenticity of the public key, you can use it to verify the signature of the application\.

**Topics**
+ [Installing the GPG tools](#awstoe-verify-signature-of-binary-download-install-tools)
+ [Authenticating and importing the public key](#awstoe-verify-signature-of-binary-download-authenticate-import-public-key)
+ [Verify the signature of the package](#awstoe-verify-signature-of-binary-package)

### Installing the GPG tools<a name="awstoe-verify-signature-of-binary-download-install-tools"></a>

If your operating system is Linux or Unix, the GPG tools are likely already installed\. To test whether the tools are installed on your system, type gpg at a command prompt\. If the GPG tools are installed, you see a GPG command prompt\. If the GPG tools are not installed, you see an error stating that the command cannot be found\. You can install the GnuPG package from a repository\. 

**To install GPG tools on Debian\-based Linux**
+ From a terminal, run the following command: apt\-get install gnupg\.

**To install GPG tools on Red Hat–based Linux**
+ From a terminal, run the following command: yum install gnupg\.

### Authenticating and importing the public key<a name="awstoe-verify-signature-of-binary-download-authenticate-import-public-key"></a>

The next step in the process is to authenticate the awstoe public key and add it as a trusted key in your `GPG` keyring\.

**To authenticate and import the awstoe public key**

1. Obtain a copy of our public `GPG` build key by doing one of the following:
   + Download the key from https://awstoe\-**<region>**\.s3\.**<region>**\.amazonaws\.com/assets/awstoe\.gpg\. For example, [https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/assets/awstoe.gpg](https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/assets/awstoe.gpg)\.
   + Copy the key from the following text and paste it into a file called `awstoe.gpg`\. Make sure to include everything that follows:

     ```
     -----BEGIN PGP PUBLIC KEY BLOCK-----
     Version: GnuPG v2
     
     mQENBF8UqwsBCACdiRF2bkZYaFSDPFC+LIkWLwFvtUCRwAHtD8KIwTJ6LVn3fHAU
     GhuK0ZH9mRrqRT2bq/xJjGsnF9VqTj2AJqndGJdDjz75YCZYM+ocZ+r5HSJaeW9i
     S5dykHj7Txti2zHe0G5+W0v7v5bPi2sPHsN7XWQ7+G2AMEPTz8PjxY//I0DvMQns
     Sle3l9hz6wCClz1l9LbBzTyHfSm5ucTXvNe88XX5Gmt37OCDM7vfli0Ctv8WFoLN
     6jbxuA/sV71yIkPm9IYp3+GvaKeT870+sn8/JOOKE/U4sJV1ppbqmuUzDfhrZUaw
     8eW8IN9A1FTIuWiZED/5L83UZuQs1S7s2PjlABEBAAG0GkFXU1RPRSA8YXdzdG9l
     QGFtYXpvbi5jb20+iQE5BBMBCAAjBQJfFKsLAhsDBwsJCAcDAgEGFQgCCQoLBBYC
     AwECHgECF4AACgkQ3r3BVvWuvFJGiwf9EVmrBR77+Qe/DUeXZJYoaFr7If/fVDZl
     6V3TC6p0J0Veme7uXleRUTFOjzbh+7e5sDX19HrnPquzCnzfMiqbp4lSoeUuNdOf
     FcpuTCQH+M+sIEIgPno4PLl0Uj2uE1o++mxmonBl/Krk+hly8hB2L/9n/vW3L7BN
     OMb1Ll9PmgGPbWipcT8KRdz4SUex9TXGYzjlWb3jU3uXetdaQY1M3kVKE1siRsRN
     YYDtpcjmwbhjpu4xm19aFqNoAHCDctEsXJA/mkU3erwIRocPyjAZE2dnlkL9ZkFZ
     z9DQkcIarbCnybDM5lemBbdhXJ6hezJE/b17VA0t1fY04MoEkn6oJg==
     =oyze
     -----END PGP PUBLIC KEY BLOCK-----
     ```

1. At a command prompt in the directory where you saved **awstoe\.gpg**, use the following command to import the awstoe public key into your keyring:

   ```
   gpg --import awstoe.gpg
   ```

   The command returns results that are similar to the following:

   ```
   gpg: key F5AEBC52: public key "AWSTOE <awstoe@amazon.com>" imported
   gpg: Total number processed: 1
   gpg:               imported: 1  (RSA: 1)
   ```

   Make a note of the key value; you need it in the next step\. In the preceding example, the key value is `F5AEBC52`\.

1. Verify the fingerprint by running the following command, replacing *key\-value* with the value from the preceding step:

   ```
   gpg --fingerprint key-value
   ```

   This command returns results similar to the following:

   ```
   pub   2048R/F5AEBC52 2020-07-19
         Key fingerprint = F6DD E01C 869F D639 15E5  5742 DEBD C156 F5AE BC52
   uid       [ unknown] AWSTOE <awstoe@amazon.com>
   ```

   Additionally, the fingerprint string should be identical to `F6DD E01C 869F D639 15E5 5742 DEBD C156 F5AE BC52`, as shown in the preceding example\. Compare the key fingerprint that is returned to the one published on this page\. They should match\. If they don't match, don't install the awstoe application installation script, and contact AWS Support\. 

### Verify the signature of the package<a name="awstoe-verify-signature-of-binary-package"></a>

After you install the `GPG` tools, authenticate and import the awstoe public key, and verify that the public key is trusted, you are ready to verify the signature of the installation script\. 

**To verify the installation script signature**

1. At a command prompt, run the following command to download the application binary:

   ```
   curl -O https://awstoe-<region>.s3.<region>.amazonaws.com/latest/linux/<architecture>/awstoe
   ```

   For example:

   ```
   curl -O https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/amd64/awstoe
   ```

   Supported values for **architecture** can be `amd64`, `386`, and `arm64`\.

1. At a command prompt, run the following command to download the signature file for the corresponding application binary from the same S3 key prefix path:

   ```
   curl -O https://awstoe-<region>.s3.<region>.amazonaws.com/latest/linux/<architecture>/awstoe.sig
   ```

   For example:

   ```
   curl -O https://awstoe-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/amd64/awstoe.sig
   ```

   Supported values for **architecture** can be `amd64`, `386`, and `arm64`\.

1. Verify the signature by running the following command at a command prompt in the directory where you saved `awstoe.sig` and the awstoe installation file\. Both files must be present\.

   ```
   gpg --verify ./awstoe.sig ~/awstoe
   ```

   The output should look something like the following:

   ```
   gpg: Signature made Mon 20 Jul 2020 08:54:55 AM IST using RSA key ID F5AEBC52
   gpg: Good signature from "AWSTOE awstoe@amazon.com" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: F6DD E01C 869F D639 15E5 5742 DEBD C156 F5AE BC52
   ```

   If the output contains the phrase `Good signature from "AWSTOE <awstoe@amazon.com>"`, it means that the signature has successfully been verified, and you can proceed to run the awstoe installation script\.

   If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, don't run the installation file that you downloaded previously, and contact AWS Support\.

The following are details about the warnings you might see: 
+ **WARNING: This key is not certified with a trusted signature\! There is no indication that the signature belongs to the owner\.** This refers to your personal level of trust in your belief that you possess an authentic public key for awstoe\. In an ideal world, you would visit an AWS office and receive the key in person\. However, more often you download it from a website\. In this case, the website is an AWS website\. 
+ **gpg: no ultimately trusted keys found\.** This means that the specific key is not "ultimately trusted" by you \(or by other people whom you trust\)\.

For more information, see [http://www\.gnupg\.org](http://www.gnupg.org)\.

## Verify the signature of the awstoe application installation download on Windows<a name="awstoe-verify-sig-win"></a>

This topic describes the recommended process for verifying the validity of the installation file for the awstoe application on Windows\-based operating systems\. 

Whenever you download an application from the internet, we recommend that you authenticate the identity of the software publisher and check that the application is not altered or corrupted since it was published\. This protects you from installing a version of the application that contains a virus or other malicious code\.

If, after running the steps in this topic, you determine that the software for the awstoe application is altered or corrupted, do NOT run the installation file\. Instead, contact AWS Support\.

To verify the validity of the downloaded awstoe binary on Windows\-based operating systems, make sure that the thumbprint of its Amazon Services LLC signer certificate is equal to this value:

**4B AD 22 73 29 AD EF 18 F2 15 B6 47 5F B7 94 8E 16 29 B5 05**

To verify this value, perform the following procedure: 

1. Right\-click the downloaded `awstoe.exe`, and open the **Properties** window\.

1. Choose the **Digital Signatures** tab\.

1. From the **Signature List**, choose **Amazon Services LLC**, and then choose **Details**\.

1. Choose the **General** tab, if not already selected, and then choose **View Certificate**\.

1. Choose the **Details** tab, and then choose **All** in the **Show** dropdown list, if not already selected\.

1. Scroll down until you see the **Thumbprint** field and then choose **Thumbprint**\. This displays the entire thumbprint value in the lower window\.
   + If the thumbprint value in the lower window is identical to the following value:

     **4B AD 22 73 29 AD EF 18 F2 15 B6 47 5F B7 94 8E 16 29 B5 05**

     then your downloaded awstoe binary is authentic and can be safely installed\.
   + If the thumbprint value in the lower details window is not identical to the value above, do not run `awstoe.exe`\. 
