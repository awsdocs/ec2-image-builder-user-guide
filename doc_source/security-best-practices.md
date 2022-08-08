# Security best practices for EC2 Image Builder<a name="security-best-practices"></a>

EC2 Image Builder provides a number of security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\.
+ Do not use overly\-permissive security groups in Image Builder recipes\.
+ Do not share images with accounts that you do not trust\.
+ Do not make images public that have private or sensitive data\.
+ Apply all available Windows or Linux security patches during image builds\.

We strongly recommend that you test your images to validate the security posture and applicable security compliance levels\. Solutions such as [Amazon Inspector](https://aws.amazon.com/inspector) can help validate the security and compliance posture of images\.

**IMDSv2 for Image Builder pipelines**  
When your Image Builder pipeline runs, it sends HTTP requests to launch EC2 instances that Image Builder uses to build and test your image\. To configure the version of IMDS that your pipeline uses for the launch requests, set the `httpTokens` parameter in your Image Builder infrastructure configuration instance metadata settings\.

**Note**  
We recommend that you configure all EC2 instances that Image Builder launches from a pipeline build to use IMDSv2 so that instance metadata retrieval requests require a signed token header\.

For more information about Image Builder infrastructure configuration, see [Manage EC2 Image Builder infrastructure configuration](manage-infra-config.md)\. For more information about EC2 instance metadata options for Linux images, see [Configure the instance metadata options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-options.html) in the Amazon EC2 User Guide for Linux Instances\. For Windows images, see [Configure the instance metadata options](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/configuring-instance-metadata-options.html) in the Amazon EC2 User Guide for Windows Instances\.

## Required post\-build clean up<a name="post-build-cleanup"></a>

After Image Builder completes all of the build steps for your custom image, Image Builder prepares the build instance for testing and image creation\. Before shutting down the build instance to create the snapshot, Image Builder performs the following clean up to ensure the security of your image:

------
#### [ Linux ]

The Image Builder pipeline runs a clean up script to help ensure that the final image follows security best practices, and to remove any build artifacts or settings that should not carry over to your snapshot\. However, you can skip sections of the script, or override the user data entirely\. Therefore, the images produced by Image Builder pipelines are not necessarily compliant with any specific regulatory criteria\.

When the pipeline completes its build and test stages, Image Builder automatically runs the following clean\-up script just before it creates the output image\.

**Important**  
If you override **User data** in your recipe, the script doesn't run\. In that case, make sure that you include a command in your user data that creates an empty file named `perform_cleanup`\. Image Builder detects this file and runs the clean\-up script prior to creating the new image\.

```
#!/bin/bash
if [[ ! -f {{workingDirectory}}/perform_cleanup ]]; then
    echo "Skipping cleanup"
    exit 0
else
    sudo rm -f {{workingDirectory}}/perform_cleanup
fi

function cleanup() {
    FILES=("$@")
    for FILE in "${FILES[@]}"; do
        if [[ -f "$FILE" ]]; then
            echo "Deleting $FILE";
            sudo shred -zuf $FILE;
        fi;
        if [[ -f $FILE ]]; then
            echo "Failed to delete '$FILE'. Failing."
            exit 1
        fi;
    done
};


# Clean up for cloud-init files
CLOUD_INIT_FILES=(
    "/etc/sudoers.d/90-cloud-init-users"
    "/etc/locale.conf"
    "/var/log/cloud-init.log"
    "/var/log/cloud-init-output.log"
)
if [[ -f {{workingDirectory}}/skip_cleanup_cloudinit_files ]]; then
    echo "Skipping cleanup of cloud init files"
else
    echo "Cleaning up cloud init files"
    cleanup "${CLOUD_INIT_FILES[@]}"
    if [[ $( sudo find /var/lib/cloud -type f | sudo wc -l ) -gt 0 ]]; then
        echo "Deleting files within /var/lib/cloud/*"
        sudo find /var/lib/cloud -type f -exec shred -zuf {} \;
    fi;

    if [[ $( sudo ls /var/lib/cloud | sudo wc -l ) -gt 0 ]]; then
        echo "Deleting /var/lib/cloud/*"
        sudo rm -rf /var/lib/cloud/* || true
    fi;
fi;


# Clean up for temporary instance files
INSTANCE_FILES=(
    "/etc/.updated"
    "/etc/aliases.db"
    "/etc/hostname"
    "/var/lib/misc/postfix.aliasesdb-stamp"
    "/var/lib/postfix/master.lock"
    "/var/spool/postfix/pid/master.pid"
    "/var/.updated"
    "/var/cache/yum/x86_64/2/.gpgkeyschecked.yum"
)
if [[ -f {{workingDirectory}}/skip_cleanup_instance_files ]]; then
    echo "Skipping cleanup of instance files"
else
    echo "Cleaning up instance files"
    cleanup "${INSTANCE_FILES[@]}"
fi;


# Clean up for ssh files
SSH_FILES=(
    "/etc/ssh/ssh_host_rsa_key"
    "/etc/ssh/ssh_host_rsa_key.pub"
    "/etc/ssh/ssh_host_ecdsa_key"
    "/etc/ssh/ssh_host_ecdsa_key.pub"
    "/etc/ssh/ssh_host_ed25519_key"
    "/etc/ssh/ssh_host_ed25519_key.pub"
    "/root/.ssh/authorized_keys"
)
if [[ -f {{workingDirectory}}/skip_cleanup_ssh_files ]]; then
    echo "Skipping cleanup of ssh files"
else
    echo "Cleaning up ssh files"
    cleanup "${SSH_FILES[@]}"
    USERS=$(ls /home/)
    for user in $USERS; do
        echo Deleting /home/"$user"/.ssh/authorized_keys;
        sudo find /home/"$user"/.ssh/authorized_keys -type f -exec shred -zuf {} \;
    done
    for user in $USERS; do
        if [[ -f /home/"$user"/.ssh/authorized_keys ]]; then
            echo Failed to delete /home/"$user"/.ssh/authorized_keys;
            exit 1
        fi;
    done;
fi;


# Clean up for instance log files
INSTANCE_LOG_FILES=(
    "/var/log/audit/audit.log"
    "/var/log/boot.log"
    "/var/log/dmesg"
    "/var/log/cron"
)
if [[ -f {{workingDirectory}}/skip_cleanup_instance_log_files ]]; then
    echo "Skipping cleanup of instance log files"
else
    echo "Cleaning up instance log files"
    cleanup "${INSTANCE_LOG_FILES[@]}"
fi;

# Clean up for TOE files
if [[ -f {{workingDirectory}}/skip_cleanup_toe_files ]]; then
    echo "Skipping cleanup of TOE files"
else
    echo "Cleaning TOE files"
    if [[ $( sudo find {{workingDirectory}}/TOE_* -type f | sudo wc -l) -gt 0 ]]; then
        echo "Deleting files within {{workingDirectory}}/TOE_*"
        sudo find {{workingDirectory}}/TOE_* -type f -exec shred -zuf {} \;
    fi
    if [[ $( sudo find {{workingDirectory}}/TOE_* -type f | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete {{workingDirectory}}/TOE_*"
        exit 1
    fi
    if [[ $( sudo find {{workingDirectory}}/TOE_* -type d | sudo wc -l) -gt 0 ]]; then
        echo "Deleting {{workingDirectory}}/TOE_*"
        sudo rm -rf {{workingDirectory}}/TOE_*
    fi
    if [[ $( sudo find {{workingDirectory}}/TOE_* -type d | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete {{workingDirectory}}/TOE_*"
        exit 1
    fi
fi

# Clean up for ssm log files
if [[ -f {{workingDirectory}}/skip_cleanup_ssm_log_files ]]; then
    echo "Skipping cleanup of ssm log files"
else
    echo "Cleaning up ssm log files"
    if [[ $( sudo find /var/log/amazon/ssm -type f | sudo wc -l) -gt 0 ]]; then
        echo "Deleting files within /var/log/amazon/ssm/*"
        sudo find /var/log/amazon/ssm -type f -exec shred -zuf {} \;
    fi
    if [[ $( sudo find /var/log/amazon/ssm -type f | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete /var/log/amazon/ssm"
        exit 1
    fi
    if [[ -d "/var/log/amazon/ssm" ]]; then
        echo "Deleting /var/log/amazon/ssm/*"
        sudo rm -rf /var/log/amazon/ssm
    fi
    if [[ -d "/var/log/amazon/ssm" ]]; then
        echo "Failed to delete /var/log/amazon/ssm"
        exit 1
    fi
fi


if [[ $( sudo find /var/log/sa/sa* -type f | sudo wc -l ) -gt 0 ]]; then
    echo "Deleting /var/log/sa/sa*"
    sudo shred -zuf /var/log/sa/sa*
fi
if [[ $( sudo find /var/log/sa/sa* -type f | sudo wc -l ) -gt 0 ]]; then
    echo "Failed to delete /var/log/sa/sa*"
    exit 1
fi

if [[ $( sudo find /var/lib/dhclient/dhclient*.lease -type f | sudo wc -l ) -gt 0 ]]; then
        echo "Deleting /var/lib/dhclient/dhclient*.lease"
        sudo shred -zuf /var/lib/dhclient/dhclient*.lease
fi
if [[ $( sudo find /var/lib/dhclient/dhclient*.lease -type f | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/lib/dhclient/dhclient*.lease"
        exit 1
fi

if [[ $( sudo find /var/tmp -type f | sudo wc -l) -gt 0 ]]; then
        echo "Deleting files within /var/tmp/*"
        sudo find /var/tmp -type f -exec shred -zuf {} \;
fi
if [[ $( sudo find /var/tmp -type f | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete /var/tmp"
        exit 1
fi
if [[ $( sudo ls /var/tmp | sudo wc -l ) -gt 0 ]]; then
        echo "Deleting /var/tmp/*"
        sudo rm -rf /var/tmp/*
fi

# Shredding is not guaranteed to work well on rolling logs

if [[ -f "/var/lib/rsyslog/imjournal.state" ]]; then
        echo "Deleting /var/lib/rsyslog/imjournal.state"
        sudo shred -zuf /var/lib/rsyslog/imjournal.state
        sudo rm -f /var/lib/rsyslog/imjournal.state
fi

if [[ $( sudo ls /var/log/journal/ | sudo wc -l ) -gt 0 ]]; then
        echo "Deleting /var/log/journal/*"
        sudo find /var/log/journal/ -type f -exec shred -zuf {} \;
        sudo rm -rf /var/log/journal/*
fi

sudo touch /etc/machine-id
```

------
#### [ Windows ]

After the Image Builder pipeline customizes Windows images, it runs the Microsoft [Sysprep](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) utility \. These actions follow [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements/)\.

------

## Override the Linux clean up script<a name="override-linux-cleanup-script"></a>

Image Builder creates images that are secure by default and follow our security best practices\. However, some more advanced use\-cases might require you to skip one or more sections of the built\-in clean up script\. If you do need to skip some of the clean up, we strongly recommend that you test your output AMI to ensure the security of your image\.

**Important**  
Skipping sections in the clean up script can result in sensitive information, such as owner account details or SSH keys being included in the final image, and in any instance launched from that image\. You might also experience problems with launching in different Availability Zones, Regions, or accounts\.

The following table outlines the sections of the clean up script, the files that are deleted in that section, and the file names that you can use to flag a section that Image Builder should skip\. To skip a specific section of the clean up script, you can use the [CreateFile](toe-action-modules.md#action-modules-createfile) component action module or a command in your user data \(if overriding\) to create an empty file with the name specified in the **Skip section file name** column\.

**Note**  
The files that you create to skip a section of the clean up script should not include a file extension\. For example, if you want to skip the `CLOUD_INIT_FILES` section of the script, but you create a file named `skip_cleanup_cloudinit_files.txt`, Image Builder will not recognize the skip file\.


**Input**  

| Clean up section | Files removed | Skip section file name | 
| --- | --- | --- | 
| `CLOUD_INIT_FILES` | `/etc/sudoers.d/90-cloud-init-users` `/etc/locale.conf` `/var/log/cloud-init.log` `/var/log/cloud-init-output.log`  | `skip_cleanup_cloudinit_files` | 
| `INSTANCE_FILES` | `/etc/.updated` `/etc/aliases.db` `/etc/hostname` `/var/lib/misc/postfix.aliasesdb-stamp` `/var/lib/postfix/master.lock` `/var/spool/postfix/pid/master.pid` `/var/.updated` `/var/cache/yum/x86_64/2/.gpgkeyschecked.yum`  | `skip_cleanup_instance_files` | 
| `SSH_FILES` | `/etc/ssh/ssh_host_rsa_key` `/etc/ssh/ssh_host_rsa_key.pub` `/etc/ssh/ssh_host_ecdsa_key` `/etc/ssh/ssh_host_ecdsa_key.pub` `/etc/ssh/ssh_host_ed25519_key` `/etc/ssh/ssh_host_ed25519_key.pub` `/root/.ssh/authorized_keys` `/home/<all users>/.ssh/authorized_keys;`  | `skip_cleanup_ssh_files` | 
| `INSTANCE_LOG_FILES` | `/var/log/audit/audit.log` `/var/log/boot.log` `/var/log/dmesg` `/var/log/cron`  | `skip_cleanup_instance_log_files` | 
| `TOE_FILES` | `{{workingDirectory}}/TOE_*` | `skip_cleanup_toe_files` | 
| `SSM_LOG_FILES` | `/var/log/amazon/ssm/*` | `skip_cleanup_ssm_log_files` | 