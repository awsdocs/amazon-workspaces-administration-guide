# Update DNS Servers for Amazon WorkSpaces<a name="update-dns-server"></a>

If you need to update the DNS server IP addresses for your Active Directory after launching your WorkSpaces, you must also update your WorkSpaces with the new DNS server settings\.

You can update your WorkSpaces with the new DNS settings in one of the following ways:
+ Update the DNS settings on the WorkSpaces **before** you update the DNS settings for Active Directory\.
+ Rebuild the WorkSpaces **after** you update the DNS settings for Active Directory\.

We recommend updating the DNS settings on the WorkSpaces before updating the DNS settings in Active Directory \(as explained in [Step 1](#update-registry-dns) of the following procedure\)\.

If you want to rebuild the WorkSpaces instead, update one of the DNS server IP addresses in your Active Directory \([Step 2](#update-dns-active-directory)\), and then follow the procedure in [Rebuild a WorkSpace](rebuild-workspace.md) to rebuild your WorkSpaces\. After you've rebuilt your WorkSpaces, follow the procedure in [Step 3](#test-updated-dns-settings) to test your DNS server updates\. After completing that step, update the IP address of your second DNS server in Active Directory, and then rebuild your WorkSpaces again\. Be sure to follow the procedure in [Step 3](#test-updated-dns-settings) to test your second DNS server update\. As noted in the [ Best Practices](#update-dns-best-practices) section, we recommend updating your DNS server IP addresses one at a time\. 

## Best Practices<a name="update-dns-best-practices"></a>

When you're updating your DNS server settings, we recommend the following best practices:
+ To avoid disconnections and inaccessibility of domain resources, we strongly recommend performing DNS server updates during off\-peak hours or during a planned maintenance period\.
+ Don't launch any new WorkSpaces during the 15 minutes before and the 15 minutes after changing your DNS server settings\.
+ When updating your DNS server settings, change one DNS server IP address at a time\. Verify that the first update is correct before updating the second IP address\. We recommend performing the following procedure \([Step 1](#update-registry-dns), [Step 2](#update-dns-active-directory), and [Step 3](#test-updated-dns-settings)\) twice to update the IP addresses one at a time\.

## Step 1: Update the DNS Server Settings on Your WorkSpaces<a name="update-registry-dns"></a>

In the following procedure, the current and new DNS server IP address values are referred to as follows:
+ Current DNS IP addresses: `OldIP1`, `OldIP2`
+ New DNS IP addresses: `NewIP1`, `NewIP2`

**Note**  
 If this is the second time you're performing this procedure, replace `OldIP1` with `OldIP2` and `NewIP1` with `NewIP2`\.

### Update the DNS Server Settings for Windows WorkSpaces<a name="update-registry-dns-windows"></a>

If you have multiple WorkSpaces, you can deploy the following registry update to the WorkSpaces by applying a Group Policy Object \(GPO\) on the Active Directory OU for your WorkSpaces\. For more information about working with GPOs, see [Manage Your Windows WorkSpaces Using Group Policy](group_policy.md)\.

You can make these updates either by using the Registry Editor or by using Windows PowerShell\. Both procedures are described in this section\.

**To update the DNS registry settings using the Registry Editor**

1. On your Windows WorkSpace, open the Windows search box, and enter **registry editor** to open the Registry Editor \(regedit\.exe\)\. 

1. When asked "Do you want to allow this app to make changes to your device?", choose **Yes**\.

1. In the Registry Editor, navigate to the following registry entry:

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\SkyLight**

1. Open the **DomainJoinDns** registry key\. Update `OldIP1` with `NewIP1`, and then choose **OK**\.

1. Close the Registry Editor\.

1. Reboot the WorkSpace, or restart the service SkyLightWorkspaceConfigService\.

1. Proceed to [Step 2](#update-dns-active-directory), and update your DNS server settings in Active Directory to replace `OldIP1` with `NewIP1`\.

**To update the DNS registry settings using PowerShell**

The following procedure uses PowerShell commands to update your registry and restart the service SkyLightWorkspaceConfigService\.

1. On your Windows WorkSpace, open the Windows search box, and enter **powershell**\. Choose **Run as Administrator**\.

1. When asked "Do you want to allow this app to make changes to your device?", choose **Yes**\.

1. In the PowerShell window, run the following command to retrieve the current DNS server IP addresses\.

   ```
   Get-ItemProperty -Path HKLM:\SOFTWARE\Amazon\SkyLight -Name DomainJoinDNS
   ```

   You should receive the following output\.

   ```
   DomainJoinDns : OldIP1,OldIP2
   PSPath        : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Amazon\SkyLight
   PSParentPath  : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Amazon
   PSChildName   : SkyLight
   PSDrive       : HKLM
   PSProvider    : Microsoft.PowerShell.Core\Registry
   ```

1. In the PowerShell window, run the following command to change `OldIP1` to `NewIP1`\. Be sure to leave `OldIP2` as is for now\.

   ```
   Set-ItemProperty -Path HKLM:\SOFTWARE\Amazon\SkyLight -Name DomainJoinDNS -Value "NewIP1, OldIP2"
   ```

1. Run the following command to restart the service SkyLightWorkspaceConfigService\.

   ```
   restart-service -Name SkyLightWorkspaceConfigService
   ```

1. Proceed to [Step 2](#update-dns-active-directory), and update your DNS server settings in Active Directory to replace `OldIP1` with `NewIP1`\.

### Update the DNS Server Settings for Linux WorkSpaces<a name="update-registry-dns-linux"></a>

If you have more than one Linux WorkSpace, we recommend that you use a configuration management solution to distribute and enforce policy\. For example, you can use [AWS Opsworks for Chef Automate](https://aws.amazon.com/opsworks/chefautomate/), [AWS OpsWorks for Puppet Enterprise](https://aws.amazon.com/opsworks/puppetenterprise/), or [Ansible](https://www.ansible.com/)\.

**To update the DNS server settings on a Linux WorkSpace**

1. On your Linux WorkSpace, open a Terminal window \(**Applications** > **System Tools** > **MATE Terminal**\)\.

1. Use the following Linux command to edit the `/etc/dhcp/dhclient.conf` file\. You must have root user privileges to edit this file\. Either become root by using the `sudo -i` command, or execute all commands with `sudo` as shown\.

   ```
   sudo vi /etc/dhcp/dhclient.conf
   ```

   In the `/etc/dhcp/dhclient.conf` file, you will see the following `prepend` command, where `OldIP1` and `OldIP2` are the IP addresses of your DNS servers\.

   ```
   prepend domain-name-servers OldIP1, OldIP2; # skylight
   ```

1. Replace `OldIP1` with `NewIP1`, and leave `OldIP2` as is for now\.

1. Save your changes to `/etc/dhcp/dhclient.conf`\.

1. Reboot the WorkSpace\.

1. Proceed to [Step 2](#update-dns-active-directory), and update your DNS server settings in Active Directory to replace `OldIP1` with `NewIP1`\.

## Step 2: Update the DNS Server Settings for Active Directory<a name="update-dns-active-directory"></a>

In this step, you update your DNS server settings for Active Directory\. As noted in the [Best Practices](#update-dns-best-practices) section, we recommend updating your DNS server IP addresses one at a time\.

To update your DNS server settings for Active Directory, see the following documentation in the *AWS Directory Service Administration Guide*:
+ **AD Connector**: [ Update the DNS Address for Your AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_update_dns.html)
+ **AWS Managed Microsoft AD**: [ Configure DNS Conditional Forwarders for Your On\-premises Domain](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_tutorial_setup_trust_prepare_onprem.html#tutorial_setup_trust_onprem_forwarder)
+ **Simple AD**: [ Configure DNS](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/simple_ad_dns.html)

After updating your DNS server settings, proceed to [Step 3](#test-updated-dns-settings)\.

## Step 3: Test the Updated DNS Server Settings<a name="test-updated-dns-settings"></a>

After completing [Step 1](#update-registry-dns) and [Step 2](#update-dns-active-directory), use the following procedure to verify that your updated DNS server settings are working as expected\.

In the following procedure, the current and new DNS server IP address values are referred to as follows:
+ Current DNS IP addresses: `OldIP1`, `OldIP2`
+ New DNS IP addresses: `NewIP1`, `NewIP2`

**Note**  
If this is the second time you're performing this procedure, replace `OldIP1` with `OldIP2` and `NewIP1` with `NewIP2`\.

### Test the Updated DNS Server Settings for Windows WorkSpaces<a name="test-updated-dns-settings-windows"></a>

1. Shut down the `OldIP1` DNS server\.

1. Log in to a Windows WorkSpace\.

1. On the Windows **Start** menu, choose **Windows System**, then choose **Command Prompt**\.

1. Run the following command, where `AD_Name` is the name of your Active Directory \(for example, `corp.example.com`\)\.

   ```
   nslookup AD_Name
   ```

   The `nslookup` command should return the following output\. \(If this is the second time you're performing this procedure, you should see `NewIP2` in place of `OldIP2`\.\)

   ```
   Server:  Full_AD_Name
   Address:  NewIP1
   
   Name:    AD_Name
   Addresses:  OldIP2
             NewIP1
   ```

1. If the output is not what you were expecting or if you receive any errors, repeat [Step 1](#update-registry-dns)\.

1. Wait for an hour and confirm that no user issues have been reported\. Verify that `NewIP1` is getting DNS queries and responding with answers\.

1. After you've verified that the first DNS server is working properly, repeat [Step 1](#update-registry-dns) to update the second DNS server, this time replacing `OldIP2` with `NewIP2`\. Then repeat Step 2 and Step 3\. 

### Test the Updated DNS Server Settings for Linux WorkSpaces<a name="test-updated-dns-settings-linux"></a>

1. Shut down the `OldIP1` DNS server\.

1. Log in to a Linux WorkSpace\.

1. On your Linux WorkSpace, open a Terminal window \(**Applications** > **System Tools** > **MATE Terminal**\)\.

1. The DNS server IP addresses returned in the DHCP response are written to the local `/etc/resolv.conf` file on the WorkSpace\. Run the following command to view the contents of the `/etc/resolv.conf `file\.

   ```
   cat /etc/resolv.conf
   ```

   You should see the following output\. \(If this is the second time you're performing this procedure, you should see `NewIP2` in place of `OldIP2`\.\)

   ```
   ; This file is generated by Amazon WorkSpaces
   ; Modifying it can make your WorkSpace inaccessible until reboot
   options timeout:2 attempts:5
   ; generated by /usr/sbin/dhclient-script
   search region.compute.internal
   nameserver NewIP1
   nameserver OldIP2
   nameserver WorkSpaceIP
   ```
**Note**  
If you make manual modifications to the `/etc/resolv.conf` file, those changes are lost when the WorkSpace is restarted\.

1. If the output is not what you were expecting or if you receive any errors, repeat [Step 1](#update-registry-dns)\.

1. The actual DNS server IP addresses are stored in the `/etc/dhcp/dhclient.conf` file\. To see the contents of this file, run the following command\.

   ```
   sudo cat /etc/dhcp/dhclient.conf
   ```

   You should see the following output\. \(If this is the second time you're performing this procedure, you should see `NewIP2` in place of `OldIP2`\.\)

   ```
   # This file is generated by Amazon WorkSpaces
   # Modifying it can make your WorkSpace inaccessible until rebuild
   prepend domain-name-servers NewIP1, OldIP2; # skylight
   ```

1. Wait for an hour and confirm that no user issues have been reported\. Verify that `NewIP1` is getting DNS queries and responding with answers\.

1. After you've verified that the first DNS server is working properly, repeat [Step 1](#update-registry-dns) to update the second DNS server, this time replacing `OldIP2` with `NewIP2`\. Then repeat Step 2 and Step 3\. 