# Manage Your Amazon Linux WorkSpaces<a name="manage_linux_workspace"></a>


****  

|  | 
| --- |
| Amazon WorkSpaces Streaming Protocol \(WSP\) WorkSpaces are available as a beta service and are subject to change\. WSP beta WorkSpaces should not be used for production workloads\. For more information about the WSP beta, see [Amazon WorkSpaces Streaming Protocol \(beta\)](http://aws.amazon.com/workspaces/wsp/)\. | 

As with Windows WorkSpaces, Amazon Linux WorkSpaces are domain joined, so you can use Active Directory Users and Groups to:
+ Administer your Amazon Linux WorkSpaces
+ Provide access to those WorkSpaces for users

Because Linux instances do not adhere to Group Policy, we recommend that you use a configuration management solution to distribute and enforce policy\. For example, you can use [AWS Opsworks for Chef Automate](https://aws.amazon.com/opsworks/chefautomate/), [AWS OpsWorks for Puppet Enterprise](https://aws.amazon.com/opsworks/puppetenterprise/), or [Ansible](https://www.ansible.com/)\.

**Note**  
Linux WorkSpaces on WorkSpaces Streaming Protocol \(WSP\) beta bundles are available only in the AWS GovCloud \(US\-West\) Region at this time\.

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

## Use Smart Cards for Authentication<a name="linux_smart_cards"></a>

Linux WorkSpaces on WorkSpaces Streaming Protocol \(WSP\) beta bundles allow the use of [Common Access Card \(CAC\)](https://www.cac.mil/Common-Access-Card) and [Personal Identity Verification \(PIV\)](https://piv.idmanagement.gov/) smart cards for authentication\.

By default, Linux WorkSpaces are configured to use smart cards for in\-session authentication, meaning that users can use smart cards only after logging in to their WorkSpaces\. Users can use smart cards for in\-session authentication for web browsers and applications\. They can also use smart cards for running `sudo` and `sudo -i` commands \(if the user has administrative privileges on the WorkSpace\)\.

**Note**  
Smart cards can be used only in the AWS GovCloud \(US\-West\) Region at this time\.
Using smart cards for pre\-session \(login\) authentication isn't available at this time\.

To use a smart card with a Linux WorkSpace, the user must use the Amazon WorkSpaces Windows client\. For more information about using smart cards with the Windows client, see [ WorkSpaces Client Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.

**To enable smart cards on Linux WorkSpaces**  
To enable the use of smart cards, you need a root CA certificate file in the PEM format included in the WorkSpace image\.

Your root CA certificate must meet the following requirements:
+ The certificate must be Base64\-encoded certificate files in PEM format\.
+ The certificate must include a Common Name\.
+ Use a strong encryption algorithm\. We recommend SHA256 with RSA, SHA256 with ECDSA, SHA384 with ECDSA, or SHA512 with ECDSA\.

Your smart card certificates must meet the following requirements:
+ The certificates must include a Common Name\.
+ The maximum length of certificate chain supported is 4\.
+ Amazon WorkSpaces does not currently support certificate revocation lists \(CRL\) for revocation of smart card certificates\. However, Online Certificate Status Protocol \(OCSP\) is supported\.
+ Use a strong encryption algorithm\. We recommend SHA256 with RSA, SHA256 with ECDSA, SHA384 with ECDSA, or SHA512 with ECDSA\.
+ Make sure the certificate is issued for Digital Signature key usage \(`<KU>digitalSignature`\) and MS Smart Card Login extended key usage \(`<EKU>msScLogin`\)\. 
+ We recommend issuing the smart card certificates for the user's default user principal name \(UPN\)\. If your smart card certificates have been issued using alternate UPN suffixes, see [To add your root CA certificate to your Linux WorkSpaces](#add-root-ca-linux) for information about how to use these suffixes\.

**To obtain your root CA certificate**  
You can obtain your root CA certificate in several ways:
+ You can use a root CA certificate issued by a third\-party certification authority\. 
+ You can export your root CA certificate by using the Web Enrollment site, which is either `http://ip_address/certsrv` or `http://fqdn/certsrv`, where `ip_address` and `fqdn` are the IP address and the fully qualified domain name \(FQDN\) of the root certification CA server\. For more information about using the Web Enrollment site, see [ How to export a Root Certification Authority Certificate](https://docs.microsoft.com/troubleshoot/windows-server/identity/export-root-certification-authority-certificate) in the Microsoft documentation\. 
+ You can use the following procedure to export the root CA certificate from the domain controller \(DC\) of your WorkSpaces directory with the Active Directory Certificate Services \(AD CS\) role installed\. For information about installing the AD CS role, see [ Install the Certification Authority](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) in the Microsoft documentation\. 

  1. Log into the root CA server using an administrator account\.

  1. From the Windows **Start** menu, open a command prompt window \(**Start** > **Windows System** > **Command Prompt**\)\. 

  1. Use the following command to export the root CA certificate to a new file, where `rootca.cer` is the name of the new file:

     ```
     certutil -ca.cert rootca.cer
     ```

     For more information about running certutil, see [ certutil](https://docs.microsoft.com/windows-server/administration/windows-commands/certutil) in the Microsoft documentation\.

  1. Use the following OpenSSL command to convert the exported root CA certificate from DER format to PEM format, where *rootca* is the name of the certificate\. For more information about OpenSSL, see [www\.openssl\.org](https://www.openssl.org/)\.

     ```
     openssl x509 -inform der -in rootca.cer -out /tmp/rootca.pem
     ```

**To add your root CA certificate to your Linux WorkSpaces**

To assist you with enabling smart cards, we've added the `enable_smartcard` script to our Amazon Linux WSP beta bundles\. This script performs the following actions:
+ Imports your root CA certificate into the [ Network Security Services \(NSS\)](https://developer.mozilla.org/docs/Mozilla/Projects/NSS) database\. 
+ Installs the `pam_pkcs11` module for Pluggable Authentication Module \(PAM\) authentication\.
+ Performs a default configuration, which includes enabling `pkinit` during WorkSpace provisioning\.

The following procedure explains how to use the `enable_smartcard` script to add your root CA certificate to your Linux WorkSpaces and enable smart cards for your Linux WorkSpaces\.

1. Create a new Linux WorkSpace with the WSP beta protocol enabled\. When launching the WorkSpace in the Amazon WorkSpaces console, on the **Select Bundles** page, be sure to select **WSP\-Beta** for the protocol, and then select one of the Amazon Linux 2 public bundles\.

1. On the new WorkSpace, run the following command as root, where `pem-path` is the path to the root CA certificate file in PEM format\.

   ```
   /usr/lib/skylight/enable_smartcard --ca-cert pem-path
   ```
**Note**  
Linux WorkSpaces assume that the certificates on the smart cards are issued for the user's default user principal name \(UPN\), such as `sAMAccountName@domain`, where `domain` is a fully qualified domain name \(FQDN\)\.   
To use alternate UPN suffixes, `run /usr/lib/skylight/enable_smartcard --help` for more information\.

1. By default, all services are enabled to use smart card authentication on Linux WorkSpaces\. To limit smart card authentication to only specific services, you must edit `/etc/pam.d/system-auth`\. Uncomment the `auth` line for `pam_succeed_if.so` and edit the list of services as needed\.

   After the `auth` line is uncommented, to allow a service use smart card authentication, you must add it to the list\. To make a service use only password authentication, you must remove it from the list\.

1. Perform any additional customizations to the WorkSpace\. For example, you might want to add a system\-wide policy to [enable users to use smart cards in Firefox](#smart-cards-firefox-linux)\. \(Chrome users must enable smart cards on their clients themselves\. For more information, see [ WorkSpaces Client Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.\) 

1. [Create a custom WorkSpace image and bundle](create-custom-bundle.md) from the WorkSpace\.

1. Use the new custom bundle to launch WorkSpaces for your users\.

**To enable users to use smart cards in Firefox**

You can enable your users to use smart cards in Firefox by adding a SecurityDevices policy to your Linux WorkSpace image\. For more information about adding system\-wide policies to Firefox, see the [Mozilla policy templates](https://github.com/mozilla/policy-templates/releases) on GitHub\.

1. On the WorkSpace that you're using to create your WorkSpace image, create a new file named `policies.json` in `/usr/lib64/firefox/distribution/`\.

1. In the JSON file, add the following SecurityDevices policy, where `NAME_OF_DEVICE` is whatever value you want to use to identify the `pkcs` module\. For example, you might want to use a value such as `"OpenSC"`:

   ```
   {
       "policies": {
           "SecurityDevices": {
               "NAME_OF_DEVICE": "/usr/lib64/opensc-pkcs11.so"
           }
       }
   }
   ```

**Troubleshooting**  
For troubleshooting, we recommend adding the `pkcs11-tools` utility\. This utility allows you to perform the following actions: 
+ List each smart card\.
+ List the slots on each smart card\.
+ List the certificates on each smart card\.

Some common issues that can cause problems:
+ Incorrect mapping of the slots to the certificates\.
+ Having multiple certificates on the smart card that can match the user\. Certificates are matched using the following criteria:
  + The root CA for the certificate\.
  + The `<KU>` and `<EKU>` fields of the certificate\.
  + The UPN in the certificate subject\.
+ Having multiple certificates that have `<EKU>msScLogin` in their key usage\.

In general, it's best to have only one certificate for smart card authentication that is mapped to the very first slot in the smart card\.

The tools for managing the certificates and keys on the smart card \(such as removing or remapping the certificates and keys\) might be manufacturer\-specific\. Additional tools that you can use to work with smart cards are:
+ `opensc-explorer`
+ `opensc-tool`
+ `pkcs11_inspect`
+ `pkcs11_listcerts`
+ `pkcs15-tool`

**To enable debug logging**

To troubleshoot your `pam_pkcs11` and `pam-krb5` configuration, you can enable debug logging\.

1. In the `/etc/pam.d/system-auth-ac` file, edit the `auth` action and change the `nodebug` parameter of `pam_pksc11.so` to `debug`\. 

1. In the `/etc/pam_pkcs11/pam_pkcs11.conf` file, change `debug = false;` to `debug = true;`\. The `debug` option applies separately to each mapper module, so you might need to change it both directly under the `pam_pkcs11` section and also under the appropriate mapper section \(by default, this is `mapper generic`\)\.

1. In the `/etc/pam.d/system-auth-ac` file, edit the `auth` action and add the `debug` or the `debug_sensitive` parameter to `pam_krb5.so`\. 

After you've enabled debug logging, the system prints out `pam_pkcs11` debug messages directly in the active terminal\. Messages from `pam_krb5` are logged in `/var/log/secure`\. 

To check which username a smart card certificate maps to, use the following `pklogin_finder` command:

```
sudo pklogin_finder debug config_file=/etc/pam_pkcs11/pam_pkcs11.conf
```

When prompted, enter the smart card PIN\. `pklogin_finder` outputs on `stdout` the username on the smart card certificate in the form `NETBIOS\username`\. This username should match the WorkSpace username\.

In Active Directory Domain Services \(AD DS\), the NetBIOS domain name is the pre\-Windows 2000 domain name\. Typically \(but not always\), the NetBIOS domain name is the subdomain of the Domain Name System \(DNS\) domain name\. For example, if the DNS domain name is `example.com`, the NetBIOS domain name is usually `EXAMPLE`\. If the DNS domain name is `corp.example.com`, the NetBIOS domain name is usually `CORP`\. 

For example, for the user `mmajor` in the domain `corp.example.com`, the output from `pklogin_finder` is `CORP\mmajor`\.

**Note**  
If you receive the message `"ERROR:pam_pkcs11.c:504: verify_certificate() failed"`, this message indicates that `pam_pkcs11` has found a certificate on the smart card that matches the username criteria but that doesn't chain up to a root CA certificate that is recognized by the machine\. When that happens, `pam_pkcs11` outputs the above message and then tries the next certificate\. It allows authentication only if it finds a certificate that both matches the username and that chains up to a recognized root CA certificate\.

To troubleshoot your `pam_krb5` configuration, you can manually invoke `kinit` with the following command:

```
kinit -X X509_user_identity=PKCS11:/usr/lib64/pkcs11/opensc-pkcs11.so -V
```

This command should successfully obtain a Kerberos Ticket Granting Ticket \(TGT\)\. If it fails, try adding the correct Kerberos principal name explicitly to the command\. For example, for the user `mmajor` in the domain `corp.example.com`, use this command: 

```
kinit -X X509_user_identity=PKCS11:/usr/lib64/pkcs11/opensc-pkcs11.so -V mmajor@CORP.EXAMPLE.COM
```

If this command succeeds, the issue is most likely in the mapping from the WorkSpace username to the Kerberos principal name\. Check the `[appdefaults]/pam/mappings` section in the `/etc/krb5.conf` file\. 

If this command doesn't succeed, but a password\-based `kinit` command does succeed, check the `pkinit_`\-related configurations in the `/etc/krb5.conf` file\. For example, if the smart card contains more than one certificate, you might need to make changes to `pkinit_cert_match`\.