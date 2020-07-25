# Update DNS Servers for WorkSpaces<a name="update-dns-server"></a>

If you need to update the on\-premises DNS server IP addresses for your Active Directory after launching your WorkSpaces, you must also update your WorkSpaces with the new DNS server settings\.

You can update your WorkSpaces with the new DNS settings in one of the following ways:
+ Update a registry setting on the WorkSpaces **before** you update the DNS settings for Active Directory\.
+ Rebuild the WorkSpaces **after** you update the DNS settings for Active Directory\.

We recommend updating the registry setting \(as explained in the following procedure\)\.

If you want to rebuild the WorkSpaces instead, update your DNS server in your Active Directory, and then follow the procedure in [Rebuild a WorkSpace](rebuild-workspace.md)\. For more information about updating your DNS server, see [Step 2: Update the DNS Server Settings for Active Directory](#update-dns-active-directory)\.

## Step 1: Update the DNS Server Settings in the Registry on Your WorkSpaces<a name="update-registry-dns"></a>

If you have multiple WorkSpaces, you can deploy the following registry update to the WorkSpaces by applying a Group Policy Object \(GPO\) on the Active Directory OU for your WorkSpaces\. For more information about working with GPOs, see [Manage Your Windows WorkSpaces Using Group Policy](group_policy.md)\.

**To update the DNS registry settings on a WorkSpace**

You can either follow these steps to update the DNS registry settings on your WorkSpace, or you can use the PowerShell script included after these steps\.

1. On your WorkSpace, open the Windows search box, and enter **registry editor** to open the Registry Editor \(regedit\.exe\)\. 

1. In the Registry Editor, navigate to the following registry entry:

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\SkyLight**

1. Open the **DomainJoinDns** registry key\. Update the IP addresses for your DNS servers, and then choose **OK**\.

1. Close the Registry Editor\.

1. Reboot the WorkSpace, or restart the service SkyLightWorkspaceConfigService\.

To use the following PowerShell script to update your registry and restart the service SkyLightWorkspaceConfigService, replace the `DomainJoinDNS DomainJoinDns` IP address values `12.3.4.56,12.3.4.57` with the IP addresses for your on\-premises DNS servers: 

```
PS C:\Windows\system32> Get-ItemProperty -Path HKLM:\SOFTWARE\Amazon\SkyLight -Name 
DomainJoinDNS DomainJoinDns : 12.3.4.56,12.3.4.57 
PSPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Amazon\SkyLight 
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Amazon 
PSChildName : SkyLight PSDrive : HKLM PSProvider : Microsoft.PowerShell.Core\Registry
restart-service -Name SkyLightWorkspaceConfigService
```

## Step 2: Update the DNS Server Settings for Active Directory<a name="update-dns-active-directory"></a>

For information about updating your DNS server settings for Active Directory, see the following documentation in the *AWS Directory Service Administration Guide*:
+ AD Connector: [ Update the DNS Address for Your AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_update_dns.html)
+ AWS Managed Microsoft AD: [ Configure DNS Conditional Forwarders for Your On\-premises Domain](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_tutorial_setup_trust_prepare_onprem.html#tutorial_setup_trust_onprem_forwarder)
+ Simple AD: [ Configure DNS](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/simple_ad_dns.html)