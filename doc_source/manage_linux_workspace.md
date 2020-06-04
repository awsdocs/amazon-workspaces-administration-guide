# Manage Your Amazon Linux WorkSpaces<a name="manage_linux_workspace"></a>

As with Windows WorkSpaces, Amazon Linux WorkSpaces are domain joined, so you can use Active Directory Users and Groups to:
+ Administer your Amazon Linux WorkSpaces
+ Provide access to those WorkSpaces for users

Because Linux instances do not adhere to Group Policy, we recommend that you use a configuration management solution to distribute and enforce policy\. For example, you can use [AWS Opsworks for Chef Automate](https://aws.amazon.com/opsworks/chefautomate/), [AWS OpsWorks for Puppet Enterprise](https://aws.amazon.com/opsworks/puppetenterprise/), or [Ansible](https://www.ansible.com/)\.

## Control PCoIP Agent Behavior on Amazon Linux WorkSpaces<a name="pcoip_agent_linux"></a>

The behavior of the PCoIP Agent is controlled by configuration settings in the `pcoip-agent.conf` file, which is located in the `/etc/pcoip-agent/` directory\. To deploy and enforce changes to the policy, use a configuration management solution that supports Amazon Linux\. Any changes take effect when the agent starts up\. Restarting the agent ends any open connections and restarts the window manager\. For a full listing of the available settings, run `man pcoip-agent.conf` from the terminal on any Amazon Linux WorkSpace\.

## Enable or Disable Clipboard Redirection for Amazon Linux WorkSpaces<a name="linux_clipboard"></a>

By default, Amazon WorkSpaces supports clipboard redirection\. Use the PCoIP Agent conf to disable this feature, if needed\. 

**To enable or disable clipboard redirection for Amazon Linux WorkSpaces**

1. Open the `pcoip-agent.conf` file in an editor with elevated rights by using the following command\.

   ```
   [domain\username@workspace-id ~]$ sudo vi /etc/pcoip-agent/pcoip-agent.conf
   ```

1. Add the following line to the end of the file\.

   ```
   pcoip.server_clipboard_state = X
   ```

   Where the possible values for *X* are:

   0 — Disabled in both directions

   1 — Enabled in both directions

   2 — Enabled client to agent only

   3 — Enabled agent to client only

## Grant SSH Access to Amazon Linux WorkSpaces Administrators<a name="linux_ssh"></a>

By default, only assigned users and accounts in the Domain Admins group can connect to Amazon Linux WorkSpaces by using SSH\. 

We recommend that you create a dedicated administrators group for your Amazon Linux WorkSpaces administrators in Active Directory\.

**To enable sudo access for members of the Linux\_Workspaces\_Admins Active Directory group**

1. Edit the `sudoers` file by using `visudo`, as shown in the following example\.

   ```
   [example\username@workspace-id ~]$ sudo visudo
   ```

1. Add the following line\.

   ```
   %example.com\\Linux_WorkSpaces_Admins ALL=(ALL) ALL 
   ```

After you create the dedicated administrators group, follow these steps to enable login for members of the group\.

**To enable login for members of the Linux\_WorkSpaces\_Admins Active Directory group**

1. Edit `/etc/security/access.conf` with elevated rights\.

   ```
   [example\username@workspace-id ~]$ sudo vi /etc/security/access.conf
   ```

1. Add the following line\.

   ```
   +:(example\Linux_WorkSpaces_Admins):ALL 
   ```

For more information about enabling SSH connections, see [Enable SSH Connections for Your Linux WorkSpaces](connect-to-linux-workspaces-with-ssh.md)\.

## Override the Default Shell for Amazon Linux WorkSpaces<a name="linux_shell"></a>

To override the default shell for Linux WorkSpaces, we recommend that you edit the user's `~/.bashrc` file\. For example, to use `Z shell` instead of `Bash` shell, add the following lines to `/home/username/.bashrc`\.

```
export SHELL=$(which zsh)
[ -n "$SSH_TTY" ] && exec $SHELL
```

## Protect Custom Repositories from Unauthorized Access<a name="password_protect_repos"></a>

To control access to your custom repositories, we recommend using the security features built into Amazon Virtual Private Cloud \(Amazon VPC\) rather than using passwords\. For example, use network access control lists \(ACLs\) and security groups\. For more information about these features, see [Security](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html) in the *Amazon VPC User Guide*\.

If you must use passwords to protect your repositories, be sure to create your `yum` repository definition files as shown in [Repository Definition Files](https://docs.fedoraproject.org/en-US/Fedora_Core/3/html/Software_Management_Guide/sn-writing-repodefs.html) in the Fedora documentation\.

## Use the Amazon Linux Extras Library Repository<a name="linux_extras"></a>

With Amazon Linux, you can use the Extras Library to install application and software updates on your instances\. For information about using the Extras Library, see [Extras Library \(Amazon Linux\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-basics.html#extras-library) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
If you are using the Amazon Linux repository, your Amazon Linux WorkSpaces must have internet access, or you must configure virtual private cloud \(VPC\) endpoints to this repository and to the main Amazon Linux repository\. For more information, see [Provide Internet Access from Your WorkSpace](amazon-workspaces-internet-access.md)\.