# Use Smart Cards for Authentication<a name="smart-cards"></a>

Windows and Linux WorkSpaces on WorkSpaces Streaming Protocol \(WSP\) bundles allow the use of [Common Access Card \(CAC\)](https://www.cac.mil/Common-Access-Card) and [Personal Identity Verification \(PIV\)](https://piv.idmanagement.gov/) smart cards for authentication\.

By default, Amazon WorkSpaces is configured to support the use of smart cards for both *pre\-session authentication* and *in\-session authentication*\. Pre\-session authentication refers to smart card authentication that's performed while users are logging in to their WorkSpaces\. In\-session authentication refers to authentication that's performed after logging in\.

For example, users can use smart cards for in\-session authentication while working with web browsers and applications\. They can also use smart cards for actions that require administrative permissions\. For example, if the user has administrative permissions on their Linux WorkSpace, they can use smart cards to authenticate themselves when running `sudo` and `sudo -i` commands\. 

**Topics**
+ [Requirements](#smart-cards-requirements)
+ [Limitations](#smart-cards-limitations)
+ [Directory Configuration](#smart-cards-directory-config)
+ [Enabling Smart Cards for Windows WorkSpaces](#smart-cards-windows-workspaces)
+ [Enabling Smart Cards for Linux WorkSpaces](#smart-cards-linux-workspaces)

## Requirements<a name="smart-cards-requirements"></a>
+ An Active Directory Connector \(AD Connector\) directory is required\. For more information about how to configure your AD Connector and your on\-premises directory, see [Directory Configuration](#smart-cards-directory-config)\.
+ To use a smart card with a Windows or Linux WorkSpace, the user must use the Amazon WorkSpaces Windows client version 3\.1\.1 or later\. For more information about using smart cards with the Windows client, see [ Smart Card Support](https://docs.aws.amazon.com/workspaces/latest/userguide/smart_card_support.html) in the *Amazon WorkSpaces User Guide*\. 
+ The root CA and smart card certificates must meet certain requirements\. For more information, see [ Enabling Smart Card Authentication for Amazon WorkSpaces](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_clientauth.html) in the *AWS Directory Service Administration Guide* and [ Certificate Requirements](https://docs.microsoft.com/en-us/windows/security/identity-protection/smart-cards/smart-card-certificate-requirements-and-enumeration#certificate-requirements) in the Microsoft documentation\. 

  In addition to those requirements, user certificates employed for smart card authentication to Amazon WorkSpaces must include the following attributes:
  + The AD user's userPrincipalName \(UPN\) in the subjectAltName \(SAN\) field of the certificate\. We recommend issuing smart card certificates for the user's default UPN\.
  + The Client Authentication \(1\.3\.6\.1\.5\.5\.7\.3\.2\) Extended Key Usage \(EKU\) attribute\.
  + The Smart Card Logon \(1\.3\.6\.1\.4\.1\.311\.20\.2\.2\) EKU attribute\.
+ For pre\-session authentication, Online Certificate Status Protocol \(OCSP\) is required for certificate revocation checking\. For in\-session authentication, OCSP is recommended, but not required\.

## Limitations<a name="smart-cards-limitations"></a>
+ Only the WorkSpaces Windows client application version 3\.1\.1 or later is currently supported for smart card authentication\.
+ The WorkSpaces Windows client application 3\.1\.1 or later supports smart cards only when the client is running on a 64\-bit version of Windows\.
+ Only AD Connector directories are currently supported for smart card authentication\.
+ Pre\-session authentication is available only in the AWS GovCloud \(US\-West\) Region at this time\. In\-session authentication is available in all Regions where WSP is supported\.
+ For in\-session authentication and pre\-session authentication on Linux or Windows WorkSpaces, only one smart card is currently allowed at a time\.
+ For pre\-session authentication, enabling both smart card authentication and username and password authentication on the same directory is not currently supported\.
+ Only CAC and PIV cards are supported at this time\. Other types of smart cards might also work, but they haven't been fully tested for use with WSP\.
+ Using a smart card to unlock the screen during a Windows or Linux WorkSpace session currently isn't supported\. To work around this issue for Windows WorkSpaces, see [To detect the Windows lock screen and disconnect the session](#lock-screen-windows)\. To work around this issue for Linux WorkSpaces, see [To disable the lock screen on Linux WorkSpaces](#lock-screen-linux)\.

## Directory Configuration<a name="smart-cards-directory-config"></a>

To enable smart card authentication, you must configure your AD Connector directory and your on\-premises directory in the following manner\.

**AD Connector Directory Configuration**  
Before you begin, make sure your AD Connector directory has been set up as described in [ AD Connector Prerequisites](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/prereq_connector.html) in the *AWS Directory Service Administration Guide*\. In particular, make sure that you have opened up the necessary ports in your firewall\. 

To finish configuring your AD Connector directory, follow the instructions in [ Enabling Smart Card Authentication for Amazon WorkSpaces](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_clientauth.html) in the *AWS Directory Service Administration Guide*\.

**Note**  
The AWS Directory Service API actions and Directory Service AWS command line interface \(AWS CLI\) commands used to configure pre\-session smart card authentication are currently available only in the AWS GovCloud \(US\-West\) Region\. 

**On\-Premises Directory Configuration**  
In addition to configuring your AD Connector directory, you must also make sure that the certificates that are issued to the domain controllers for your on\-premises directory have the "KDC Authentication" extended key usage \(EKU\) set\. To do this, use the Active Directory Domain Services \(AD DS\) default Kerberos Authentication certificate template\. Do not use a Domain Controller certificate template or a Domain Controller Authentication certificate template because those templates don't contain the necessary settings for smart card authentication\.

## Enabling Smart Cards for Windows WorkSpaces<a name="smart-cards-windows-workspaces"></a>

For general guidance on how to enable smart card authentication on Windows, see [ Guidelines for enabling smart card logon with third\-party certification authorities](https://docs.microsoft.com/troubleshoot/windows-server/windows-security/enabling-smart-card-logon-third-party-certification-authorities) in the Microsoft documentation\.

**To detect the Windows lock screen and disconnect the session**  
To allow users to unlock Windows WorkSpaces that are enabled for smart card pre\-session authentication when the screen is locked, you can enable Windows lock screen detection in users' sessions\. When the Windows lock screen is detected, the WorkSpace session is disconnected, and the user can reconnect from the WorkSpaces client by using their smart card\.

 You can enable disconnecting the session when the Windows lock screen is detected by using Group Policy settings\. For more information, see [Enable or Disable Disconnect Session on Screen Lock for WSP](group_policy.md#gp_lock_screen_in_wsp)\.

**To disable in\-session or pre\-session authentication**  
By default, Windows WorkSpaces are configured to support the use of smart cards for pre\-session and in\-session authentication\. If needed, you can disable in\-session authentication for Windows WorkSpaces by using Group Policy settings\. For more information, see [Enable or Disable Smart Card Redirection for WSP](group_policy.md#gp_smart_cards_in_wsp)\.

You can't disable pre\-session authentication through Group Policy, but you can do so through your AD Connector directory settings by using the DisableClientAuthentication API action or the disable\-client\-authentication CLI command\. For more information, see [ Enabling Smart Card Authentication for Amazon WorkSpaces](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_clientauth.html) in the *AWS Directory Service Administration Guide*\.

**To enable users to use smart cards in a browser**  
If your users are using Chrome as their browser, no special configuration is required to use smart cards\.

If your users are using Firefox as their browser, you can enable your users to use smart cards in Firefox through Group Policy\. You can use these [ Firefox Group Policy templates](https://github.com/mozilla/policy-templates/tree/master/windows) in GitHub\.

You must install the appropriate version of [OpenSC](https://github.com/OpenSC/OpenSC/wiki) for Windows \(64\-bit or 32\-bit\) to support PKCS \#11, and then use the following Group Policy setting, where `NAME_OF_DEVICE` is whatever value you want to use to identify PKCS \#11, such as `OpenSC`, and where `PATH_TO_LIBRARY_FOR_DEVICE` is the path to PKCS \#11\. This path should point to a library with a \.DLL extension, such as `C:\Windows\System32\pkcs11.dll`\.

```
Software\Policies\Mozilla\Firefox\SecurityDevices\NAME_OF_DEVICE = PATH_TO_LIBRARY_FOR_DEVICE
```

**Troubleshooting**  
For information about troubleshooting smart cards, see [ Certificate and configuration problems](https://docs.microsoft.com/troubleshoot/windows-server/windows-security/enabling-smart-card-logon-third-party-certification-authorities#certificate-and-configuration-problems) in the Microsoft documentation\. 

Some common issues that can cause problems:
+ Incorrect mapping of the slots to the certificates\.
+ Having multiple certificates on the smart card that can match the user\. Certificates are matched using the following criteria:
  + The root CA for the certificate\.
  + The `<KU>` and `<EKU>` fields of the certificate\.
  + The UPN in the certificate subject\.
+ Having multiple certificates that have `<EKU>msScLogin` in their key usage\.

In general, it's best to have only one certificate for smart card authentication that is mapped to the very first slot in the smart card\.

The tools for managing the certificates and keys on the smart card \(such as removing or remapping the certificates and keys\) might be manufacturer\-specific\. For more information, see the documentation provided by the manufacturer of your smart cards\.

## Enabling Smart Cards for Linux WorkSpaces<a name="smart-cards-linux-workspaces"></a>

To enable the use of smart cards on Linux WorkSpaces, you need to include a root CA certificate file in the PEM format in the WorkSpace image\.

**To obtain your root CA certificate**  
You can obtain your root CA certificate in several ways:
+ You can use a root CA certificate operated by a third\-party certification authority\. 
+ You can export your own root CA certificate by using the Web Enrollment site, which is either `http://ip_address/certsrv` or `http://fqdn/certsrv`, where `ip_address` and `fqdn` are the IP address and the fully qualified domain name \(FQDN\) of the root certification CA server\. For more information about using the Web Enrollment site, see [ How to export a Root Certification Authority Certificate](https://docs.microsoft.com/troubleshoot/windows-server/identity/export-root-certification-authority-certificate) in the Microsoft documentation\. 
+ You can use the following procedure to export the root CA certificate from a root CA certification server that is running Active Directory Certificate Services \(AD CS\)\. For information about installing AD CS, see [ Install the Certification Authority](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) in the Microsoft documentation\. 

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

To assist you with enabling smart cards, we've added the `enable_smartcard` script to our Amazon Linux WSP bundles\. This script performs the following actions:
+ Imports your root CA certificate into the [ Network Security Services \(NSS\)](https://developer.mozilla.org/docs/Mozilla/Projects/NSS) database\. 
+ Installs the `pam_pkcs11` module for Pluggable Authentication Module \(PAM\) authentication\.
+ Performs a default configuration, which includes enabling `pkinit` during WorkSpace provisioning\.

The following procedure explains how to use the `enable_smartcard` script to add your root CA certificate to your Linux WorkSpaces and to enable smart cards for your Linux WorkSpaces\.

1. Create a new Linux WorkSpace with the WSP protocol enabled\. When launching the WorkSpace in the Amazon WorkSpaces console, on the **Select Bundles** page, be sure to select **WSP** for the protocol, and then select one of the Amazon Linux 2 public bundles\.

1. On the new WorkSpace, run the following command as root, where `pem-path` is the path to the root CA certificate file in PEM format\.

   ```
   /usr/lib/skylight/enable_smartcard --ca-cert pem-path
   ```
**Note**  
Linux WorkSpaces assume that the certificates on the smart cards are issued for the user's default user principal name \(UPN\), such as `sAMAccountName@domain`, where `domain` is a fully qualified domain name \(FQDN\)\.   
To use alternate UPN suffixes, `run /usr/lib/skylight/enable_smartcard --help` for more information\. The mapping for alternate UPN suffixes is unique to each user\. Therefore, that mapping must be performed individually on each user's WorkSpace\.

1. \(Optional\) By default, all services are enabled to use smart card authentication on Linux WorkSpaces\. To limit smart card authentication to only specific services, you must edit `/etc/pam.d/system-auth`\. Uncomment the `auth` line for `pam_succeed_if.so` and edit the list of services as needed\.

   After the `auth` line is uncommented, to allow a service to use smart card authentication, you must add it to the list\. To make a service use only password authentication, you must remove it from the list\.

1. \(Optional\) Using a smart card to unlock the screen isn't currently supported\. To disable the lock screen on Linux WorkSpaces, create a file named `/usr/share/glib-2.0/schemas/10_screensaver.gschema.override` with the following contents:

   ```
   [org.mate.screensaver]
   lock-enabled=false
   ```

   After creating this file, run this command: 

   ```
   sudo glib-compile-schemas /usr/share/glib-2.0/schemas/
   ```

1. Perform any additional customizations to the WorkSpace\. For example, you might want to add a system\-wide policy to [enable users to use smart cards in Firefox](#smart-cards-firefox-linux)\. \(Chrome users must enable smart cards on their clients themselves\. For more information, see [ Smart Card Support](https://docs.aws.amazon.com/workspaces/latest/userguide/smart_card_support.html) in the *Amazon WorkSpaces User Guide*\.\) 

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
If you receive the message `"ERROR:pam_pkcs11.c:504: verify_certificate() failed"`, this message indicates that `pam_pkcs11` has found a certificate on the smart card that matches the username criteria but that doesn't chain up to a root CA certificate that is recognized by the machine\. When that happens, `pam_pkcs11` outputs the above message and then tries the next certificate\. It allows authentication only if it finds a certificate that both matches the username and chains up to a recognized root CA certificate\.

To troubleshoot your `pam_krb5` configuration, you can manually invoke `kinit` in debug mode with the following command:

```
KRB5_TRACE=/dev/stdout kinit -V
```

This command should successfully obtain a Kerberos Ticket Granting Ticket \(TGT\)\. If it fails, try adding the correct Kerberos principal name explicitly to the command\. For example, for the user `mmajor` in the domain `corp.example.com`, use this command: 

```
KRB5_TRACE=/dev/stdout kinit -V mmajor
```

If this command succeeds, the issue is most likely in the mapping from the WorkSpace username to the Kerberos principal name\. Check the `[appdefaults]/pam/mappings` section in the `/etc/krb5.conf` file\. 

If this command doesn't succeed, but a password\-based `kinit` command does succeed, check the `pkinit_`\-related configurations in the `/etc/krb5.conf` file\. For example, if the smart card contains more than one certificate, you might need to make changes to `pkinit_cert_match`\.