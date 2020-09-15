# Security Best Practices for EC2 Image Builder<a name="security-best-practices"></a>

EC2 Image Builder provides a number of security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\.
+ Do not use overly\-permissive security groups in Image Builder recipes\.
+ Do not share images with accounts that you do not trust\.
+ Do not make images public that have private or sensitive data\.
+ Apply all available Windows or Linux security patches during image builds\.

**Script Execution**

When building Linux images using EC2 Image Builder, AWS will enforce the execution of a script that will run at the end of the image building process\. Similarly, EC2 Image Builder will run Microsoft’s [Sysprep](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) utility after customizing Windows images\. These actions follow [AWS best practices for hardening and cleaning the image](https://aws.amazon.com/articles/public-ami-publishing-hardening-and-clean-up-requirements/)\. However, because additional customizations can be made during image customization, AWS does not guarantee the images produced to be compliant with any specific regulatory criteria\.

AWS recommends that you test your images to validate the security posture and applicable security compliance levels\. 

The following script is run as a mandatory step when Linux images are customized with EC2 Image Builder\.

```
#!/bin/bash

FILES=(
        # Secure removal of list of sudo users
        "/etc/sudoers.d/90-cloud-init-users"
        
        # Secure removal of RSA encrypted SSH host keys.        
        "/etc/ssh/ssh_host_rsa_key"
        "/etc/ssh/ssh_host_rsa_key.pub"

        # Secure removal of ECDSA encrypted SSH host keys.
        "/etc/ssh/ssh_host_ecdsa_key"
        "/etc/ssh/ssh_host_ecdsa_key.pub"

        # Secure removal of ED25519 encrypted SSH host keys.
        "/etc/ssh/ssh_host_ed25519_key"
        "/etc/ssh/ssh_host_ed25519_key.pub"

        # Secure removal of "root" user approved SSH keys list.
        "/root/.ssh/authorized_keys"

        # Secure removal of "ec2-user" user approved SSH keys list.
        "/home/ec2-user/.ssh/authorized_keys"

        # Secure removal of file which tracks system updates
        "/etc/.updated"
        "/var/.updated"

        # Secure removal of file with aliases for mailing lists
        "/etc/aliases.db"

        # Secure removal of file which contains the hostname of the system
        "/etc/hostname"

        # Secure removal of files with system-wide locale settings
        "/etc/locale.conf"

        # Secure removal of cached GPG signatures of yum repositories
        "/var/cache/yum/x86_64/2/.gpgkeyschecked.yum"

        # Secure removal of audit framework logs
        "/var/log/audit/audit.log"

        # Secure removal of boot logs
        "/var/log/boot.log"

        # Secure removal of kernel message logs
        "/var/log/dmesg"

        # Secure removal of cloud-init logs
        "/var/log/cloud-init.log"

        # Secure removal of cloud-init's output logs
        "/var/log/cloud-init-output.log"

        # Secure removal of cron logs
        "/var/log/cron"

        # Secure removal of aliases file for the Postfix mail transfer agent
        "/var/lib/misc/postfix.aliasesdb-stamp"

        # Secure removal of master lock for the Postfix mail transfer agent
        "/var/lib/postfix/master.lock"

        # Secure removal of spool data for the Postfix mail transfer agent
        "/var/spool/postfix/pid/master.pid"

        # Secure removal of history of Bash commands
        "/home/ec2-user/.bash_history"

        # Secure removal of file which relabels all files in the next boot
        "/.autorelabel"
)

for FILE in "${FILES[@]}"; do
        echo "Deleting $FILE"
        sudo shred -zuf $FILE
        if [[ -f $FILE ]]; then
                echo "Failed to delete '$FILE'. Failing."
                exit 1
        fi
done

# Secure removal of TOE's log directories
echo "Deleting {{workingDirectory}}/TOE_*"
sudo find {{workingDirectory}}/TOE_* -type f -exec shred -zuf {} \;
if [[ $( sudo find {{workingDirectory}}/TOE_* -type f | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete {{workingDirectory}}/TOE_*"
        exit 1
fi
sudo rm -rf {{workingDirectory}}/TOE_*
if [[ $( sudo find {{workingDirectory}}/TOE_* -type d | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete {{workingDirectory}}/TOE_*"
        exit 1
fi

# Secure removal of system activity reports/logs
echo "Deleting /var/log/sa/sa*"
sudo shred -zuf /var/log/sa/sa*
if [[ $( sudo find /var/log/sa/sa* -type f | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/log/sa/sa*"
        exit 1
fi

# Secure removal of SSM logs
echo "Deleting /var/log/amazon/ssm/*"
sudo find /var/log/amazon/ssm -type f -exec shred -zuf {} \;
if [[ $( sudo find /var/log/amazon/ssm -type f | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete /var/log/amazon/ssm"
        exit 1
fi
sudo rm -rf /var/log/amazon/ssm
if [[ -d "/var/log/amazon/ssm" ]]; then
        echo "Failed to delete /var/log/amazon/ssm"
        exit 1
fi

# Secure removal of DHCP client leases that have been acquired
echo "Deleting /var/lib/dhclient/dhclient*.lease"
sudo shred -zuf /var/lib/dhclient/dhclient*.lease
if [[ $( sudo find /var/lib/dhclient/dhclient*.lease -type f | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/lib/dhclient/dhclient*.lease"
        exit 1
fi

# Secure removal of cloud-init files
echo "Deleting /var/lib/cloud/*"
sudo find /var/lib/cloud -type f -exec shred -zuf {} \;
if [[ $( sudo find /var/lib/cloud -type f | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/lib/cloud"
        exit 1
fi
sudo rm -rf /var/lib/cloud/*
if [[ $( sudo ls /var/lib/cloud | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/lib/cloud/*"
        exit 1
fi

# Secure removal of temporary files
echo "Deleting /var/tmp/*"
sudo find /var/tmp -type f -exec shred -zuf {} \;
if [[ $( sudo find /var/tmp -type f | sudo wc -l) -gt 0 ]]; then
        echo "Failed to delete /var/tmp"
        exit 1
fi
sudo rm -rf /var/tmp/*
if [[ $( sudo ls /var/tmp | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/tmp/*"
        exit 1
fi

# Shredding is not guaranteed to work well on rolling logs

# Removal of system logs
echo "Deleting /var/lib/rsyslog/imjournal.state"
sudo shred -zuf /var/lib/rsyslog/imjournal.state
sudo rm -f /var/lib/rsyslog/imjournal.state
if [[ -f "/var/lib/rsyslog/imjournal.state" ]]; then
        echo "Failed to delete /var/lib/rsyslog/imjournal.state"
        exit 1
fi

# Removal of journal logs
echo "Deleting /var/log/journal/*"
sudo find /var/log/journal/ -type f -exec shred -zuf {} \;
sudo rm -rf /var/log/journal/*
if [[ $( sudo ls /var/log/journal/ | sudo wc -l ) -gt 0 ]]; then
        echo "Failed to delete /var/log/journal/*"
        exit 1
fi
```
