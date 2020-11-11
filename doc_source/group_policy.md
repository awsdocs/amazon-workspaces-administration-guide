# Manage Your Windows WorkSpaces<a name="group_policy"></a>


****  

|  | 
| --- |
| Amazon WorkSpaces Streaming Protocol \(WSP\) WorkSpaces are available as a beta service and are subject to change\. WSP beta WorkSpaces should not be used for production workloads\. For more information about the WSP beta, see [Amazon WorkSpaces Streaming Protocol \(beta\)](http://aws.amazon.com/workspaces/wsp/)\. | 

You can use Group Policy Objects \(GPOs\) to apply settings to manage Windows WorkSpaces or users that are part of your Windows WorkSpaces directory\.

**Note**  
Linux instances do not adhere to Group Policy\. For information about managing Amazon Linux WorkSpaces, see [Manage Your Amazon Linux WorkSpaces](manage_linux_workspace.md)\. 

We recommend that you create an organizational unit for your WorkSpaces Computer Objects and an organizational unit for your WorkSpaces User Objects\.

To use the Group Policy settings that are specific to Amazon WorkSpaces, you must install the Group Policy administrative template for the protocol or protocols that you are using, either PCoIP or WorkSpaces Streaming Protocol \(WSP\) beta\.

**Warning**  
Group Policy settings can affect the experience of your WorkSpace users as follows:  
**Implementing an interactive logon message to display a logon banner prevents users from being able to access their WorkSpaces\.** The interactive logon message Group Policy setting is not currently supported by Amazon WorkSpaces\.
**Disabling removable storage through Group Policy settings causes a login failure** that results in users being logged in to temporary user profiles with no access to drive D\.
**Removing users from the Remote Desktop Users local group through Group Policy settings prevents those users from being able to authenticate** through the WorkSpaces client applications\.
Group Policy settings can be used to restrict drive access\. **If you configure Group Policy settings to restrict access to drive C or to drive D, users can't access their WorkSpaces\.** To prevent this issue from occurring, make sure that your users can access drive C and drive D\. 
Some Group Policy settings force users to log off when they are disconnected from a session\. Any applications that users have open on their WorkSpaces are closed\.

For information about using the Active Directory administration tools to work with GPOs, see [Set Up Active Directory Administration Tools for Amazon WorkSpaces](directory_administration.md)\.

**Contents**
+ [Install the Group Policy Administrative Template for PCoIP](#gp_install_template)
  + [Configure Printer Support for PCoIP](#gp_local_printers)
  + [Enable or Disable Clipboard Redirection for PCoIP](#gp_clipboard)
  + [Set the Session Resume Timeout for PCoIP](#gp_auto_resume)
  + [Disable Time Zone Redirection for PCoIP](#gp_time_zone)
+ [Install the Group Policy Administrative Template Files for the WorkSpaces Streaming Protocol \(WSP\) beta](#gp_install_template_wsp)
  + [Configure Printer Support for WSP beta](#gp_local_printers_wsp)
  + [Enable or Disable Clipboard Redirection for WSP beta](#gp_clipboard_wsp)
  + [Enable or Disable Video\-In Redirection for WSP beta](#gp_video_in_wsp)
  + [Enable or Disable Audio\-In Redirection for WSP beta](#gp_audio_in_wsp)
  + [Disable Time Zone Redirection for WSP beta](#gp_time_zone_wsp)
+ [Set the Maximum Lifetime for a Kerberos Ticket](#gp_kerberos_ticket)
+ [Configure Device Proxy Server Settings for Internet Access](#gp_device_proxy)

## Install the Group Policy Administrative Template for PCoIP<a name="gp_install_template"></a>

To use the Group Policy settings that are specific to Amazon WorkSpaces when using the PCoIP protocol, you must install the Group Policy administrative template for PCoIP\. Perform the following procedure on a directory administration WorkSpace or Amazon EC2 instance that is joined to your directory\.

**To install the Group Policy administrative template for PCoIP**

1. From a running Windows WorkSpace, make a copy of the `pcoip.adm` file in the `C:\Program Files (x86)\Teradici\PCoIP Agent\configuration` directory\.

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\) and navigate to the organizational unit in your domain that contains your WorkSpaces machine accounts\.

1. Open the context \(right\-click\) menu for the machine account organizational unit and choose **Create a GPO in this domain, and link it here**\.

1. In the **New GPO** dialog box, enter a descriptive name for the GPO, such as **WorkSpaces Machine Policies**, and leave **Source Starter GPO** set to **\(none\)**\. Choose **OK**\.

1. Open the context \(right\-click\) menu for the new GPO and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, and **Administrative Templates**\. Choose **Action**, **Add/Remove Templates** from the main menu\. 

1. In the **Add/Remove Templates** dialog box, choose **Add**, select the `pcoip.adm` file copied previously, and then choose **Open**, **Close**\.

1. Close the Group Policy Management Editor\. You can now use this GPO to modify the Group Policy settings that are specific to Amazon WorkSpaces\.

### Configure Printer Support for PCoIP<a name="gp_local_printers"></a>

By default, Amazon WorkSpaces enables Basic remote printing, which offers limited printing capabilities because it uses a generic printer driver on the host side to ensure compatible printing\.

Advanced remote printing for Windows clients lets you use specific features of your printer, such as double\-sided printing, but it requires installation of the matching printer driver on the host side\.

Remote printing is implemented as a virtual channel\. If virtual channels are disabled, remote printing does not function\.

For Windows WorkSpaces, you can use Group Policy settings to configure printer support as needed\.

**To configure printer support**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for PCoIP](#gp_install_template) is installed in your domain\.

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select the WorkSpaces GPO for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administrator Defaults**\.

1. Open the **Configure remote printing** setting\.

1. In the **Configure remote printing** dialog box, do one of the following:
   + To enable Advanced remote printing, choose **Enabled**, and then under **Options,** **Configure remote printing**, choose **Basic and Advanced printing for Windows clients**\. To automatically use the client computer's current default printer, select **Automatically set default printer**\.
   + To disable printing, choose **Enabled**, and then under **Options,** **Configure remote printing**, choose **Printing disabled**\.

1. Choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

By default, local printer auto\-redirection is disabled\. You can use Group Policy settings to enable this feature so that your local printer is set as the default printer every time you connect to your WorkSpace\.

**Note**  
Local printer redirection is not available for Amazon Linux WorkSpaces\. 

**To enable local printer auto\-redirection**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for PCoIP](#gp_install_template) is installed in your domain\.

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select the WorkSpaces GPO for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administrator Defaults**\.

1. Open the **Configure remote printing** setting\.

1. Choose **Enabled**, and then under **Options**, **Configure remote printing**, choose either **Basic and Advanced printing for Windows clients** or choose **Basic printing**, select **Automatically set default printer**, and then choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

### Enable or Disable Clipboard Redirection for PCoIP<a name="gp_clipboard"></a>

By default, Amazon WorkSpaces supports clipboard redirection\. If needed for Windows WorkSpaces, you can use Group Policy settings to disable this feature\. 

**To enable or disable clipboard redirection**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for PCoIP](#gp_install_template) is installed in your domain\.

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select the WorkSpaces GPO for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**,**Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administrator Defaults**\.

1. Open the **Configure clipboard redirection** setting\.

1. In the **Configure clipboard redirection** dialog box, choose **Enabled** and then choose one of the following settings to determine the direction in which clipboard redirection is allowed\. When you're done, choose **OK**\.
   + Disabled in both directions
   + Enabled agent to client only \(WorkSpace to local computer\)
   + Enabled client to agent only \(local computer to WorkSpace\)
   + Enabled in both directions 

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

**Known Limitation**  
With clipboard redirection enabled on the WorkSpace, if you copy content that is larger than 890 KB from a Microsoft Office application, the application might become slow or unresponsive for up to 5 seconds\.

### Set the Session Resume Timeout for PCoIP<a name="gp_auto_resume"></a>

When using the Amazon WorkSpaces client applications, an interruption of network connectivity causes an active session to be disconnected\. This can be caused by events such as closing the laptop lid, or the loss of your wireless network connection\. The Amazon WorkSpaces client applications for Windows and macOS attempt to reconnect the session automatically if network connectivity is regained within a certain amount of time\. The default session resume timeout is 20 minutes, but you can modify that value for WorkSpaces that are controlled by your domain's Group Policy settings\.

**To set the automatic session resume timeout value**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for PCoIP](#gp_install_template) is installed in your domain\.

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select the WorkSpaces GPO for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, and **PCoIP Session Variables**\.

   To allow the user to override your setting, choose **Overridable Administrator Defaults**; otherwise, choose **Not Overridable Administrator Defaults**\.

1. Open the **Configure Session Automatic Reconnection Policy** setting\.

1. In the **Configure Session Automatic Reconnection Policy** dialog box, choose **Enabled**, set the **Configure Session Automatic Reconnection Policy** option to the desired timeout, in minutes, and choose **OK**\. 

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

### Disable Time Zone Redirection for PCoIP<a name="gp_time_zone"></a>

By default, the time within a Workspace is set to mirror the time zone of the client that is being used to connect to the WorkSpace\. This behavior is controlled through time zone redirection\. You might want to turn off time zone direction for various reasons: 
+ Your company wants all employees to work in a certain time zone \(even if some employees are in other time zones\)\.
+ You have scheduled tasks in a WorkSpace that are meant to run at a certain time in a specific time zone\.
+ Your users who travel a lot want to keep their WorkSpaces in one time zone for consistency and personal preference\.

If needed for Windows WorkSpaces, you can use Group Policy settings to disable this feature\.

**To disable time zone redirection**

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select a GPO at the domain or domain controller level of the directory you use for your WorkSpaces\. \(If you have the [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) installed in your domain, you can use the WorkSpaces GPO for your WorkSpaces machine accounts\.\)

1. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Windows Components**, **Remote Desktop Services**, **Remote Desktop Session Host**, and **Device and Resource Redirection**\. 

1. Open the **Allow time zone redirection** setting\.

1. In the **Allow time zone redirection** dialog box, choose **Disabled**, and choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

1. Set the time zone for the WorkSpaces to the desired time zone\.

The time zone of the WorkSpaces is now static and no longer mirrors the time zone of the client machines\. 

## Install the Group Policy Administrative Template Files for the WorkSpaces Streaming Protocol \(WSP\) beta<a name="gp_install_template_wsp"></a>

To use the Group Policy settings that are specific to Amazon WorkSpaces when using the WorkSpaces Streaming Protocol \(WSP\) beta, you must add the Group Policy administrative template `wsp.admx` and `wsp.adml` files for WSP beta to the Central Store of the domain controller for your WorkSpaces directory\. For more information about `.admx` and `.adml` files, see [ How to create and manage the Central Store for Group Policy Administrative Templates in Windows](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra)\.

The following procedure describes how to create the Central Store and add the administrative template files to it\. Perform the following procedure on a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory\.

**To install the Group Policy administrative template files for WSP beta**

1. From a running Windows WorkSpace, make a copy of the `wsp.adm` and `wsp.adml` files in the `C:\Program Files\Amazon\WSP` directory\.

1. On a directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, navigate to the domain's shared network folder\. This folder will have your organization's fully qualified domain name \(FQDN\), such as `\\example.com`\. In the Windows File Explorer, go to **Network** > ***FQDN***\.

1. Open the `SYSVOL` folder\.

1. Open the folder with the `FQDN` name\.

1. Open the `Policies` folder\. You should now be in `\\FQDN\SYSVOL\FQDN\Policies`\.

1. If it doesn't already exist, create a folder named `PolicyDefinitions`\.

1. Open the `PolicyDefinitions` folder\.

1. Copy the `wsp.admx` file into the `\\FQDN\SYSVOL\FQDN\Policies\PolicyDefinitions` folder\.

1. Create a folder named `en-US` in the `PolicyDefinitions` folder\.

1. Open the `en-US` folder\.

1. Copy the `wsp.adml` file into the `\\FQDN\SYSVOL\FQDN\Policies\PolicyDefinitions\en-US` folder\.

**To verify that the administrative template files are correctly installed**

1. On your directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\)\.

1. Expand the forest \(**Forest:*FQDN***\)\.

1. Expand **Domains**\. 

1. Expand your FQDN \(for example, `example.com`\)\.

1. Expand **Group Policy Objects**\.

1. Select **Default Domain Policy**, open the context \(right\-click\) menu, and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Amazon**, and **WSP**\.

1. You can now use this WSP Group Policy object to modify the Group Policy settings that are specific to Amazon WorkSpaces when using WSP beta\.

### Configure Printer Support for WSP beta<a name="gp_local_printers_wsp"></a>

By default, Amazon WorkSpaces enables Basic remote printing, which offers limited printing capabilities because it uses a generic printer driver on the host side to ensure compatible printing\.

Advanced remote printing \(not available for the WSP beta\) for Windows clients lets you use specific features of your printer, such as double\-sided printing, but it requires installation of the matching printer driver on the host side\.

Remote printing is implemented as a virtual channel\. If virtual channels are disabled, remote printing does not function\.

For Windows WorkSpaces, you can use Group Policy settings to configure printer support as needed\.

**To configure printer support**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for WSP beta](#gp_install_template_wsp) is installed in the Central Store of the domain controller for your WorkSpaces directory\.

1. On your directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\)\.

1. Expand the forest \(**Forest:*FQDN***\)\.

1. Expand **Domains**\. 

1. Expand your FQDN \(for example, `example.com`\)\.

1. Expand **Group Policy Objects**\.

1. Select **Default Domain Policy**, open the context \(right\-click\) menu, and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Amazon**, and **WSP**\.

1. Open the **Configure remote printing** setting\.

1. In the **Configure remote printing** dialog box, do one of the following:
   + To enable local printer redirection, choose **Enabled**, and then for **Printing options**, choose **Basic**\. To automatically use the client computer's current default printer, select **Map local default printer to the remote host**\.
   + To disable printing, choose **Disabled**\.

1. Choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

### Enable or Disable Clipboard Redirection for WSP beta<a name="gp_clipboard_wsp"></a>

By default, Amazon WorkSpaces supports two\-way \(copy/paste\) clipboard redirection\. If needed for Windows WorkSpaces, you can use Group Policy settings to disable this feature\. 

**To enable or disable clipboard redirection for Windows WorkSpaces**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for WSP beta](#gp_install_template_wsp) is installed in the Central Store of the domain controller for your WorkSpaces directory\.

1. On your directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\)\.

1. Expand the forest \(**Forest:*FQDN***\)\.

1. Expand **Domains**\. 

1. Expand your FQDN \(for example, `example.com`\)\.

1. Expand **Group Policy Objects**\.

1. Select **Default Domain Policy**, open the context \(right\-click\) menu, and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Amazon**, and **WSP**\.

1. Open the **Enable/disable clipboard redirection** setting\.

1. In the **Enable/disable clipboard redirection** dialog box, choose **Enabled** or **Disabled**\.

1. Choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

**Known Limitation**  
With clipboard redirection enabled on the WorkSpace, if you copy content that is larger than 890 KB from a Microsoft Office application, the application might become slow or unresponsive for up to 5 seconds\.

### Enable or Disable Video\-In Redirection for WSP beta<a name="gp_video_in_wsp"></a>

By default, Amazon WorkSpaces supports redirecting data from a local camera\. If needed for Windows WorkSpaces, you can use Group Policy settings to disable this feature\. 

**To enable or disable video\-in redirection for Windows WorkSpaces**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for WSP beta](#gp_install_template_wsp) is installed in the Central Store of the domain controller for your WorkSpaces directory\.

1. On your directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\)\.

1. Expand the forest \(**Forest:*FQDN***\)\.

1. Expand **Domains**\. 

1. Expand your FQDN \(for example, `example.com`\)\.

1. Expand **Group Policy Objects**\.

1. Select **Default Domain Policy**, open the context \(right\-click\) menu, and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Amazon**, and **WSP**\.

1. Open the **Enable/disable video\-in redirection** setting\.

1. In the **Enable/disable video\-in redirection** dialog box, choose **Enabled** or **Disabled**\.

1. Choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

### Enable or Disable Audio\-In Redirection for WSP beta<a name="gp_audio_in_wsp"></a>

By default, Amazon WorkSpaces supports redirecting data from a local microphone\. If needed for Windows WorkSpaces, you can use Group Policy settings to disable this feature\. 

**To enable or disable audio\-in redirection for Windows WorkSpaces**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for WSP beta](#gp_install_template_wsp) is installed in the Central Store of the domain controller for your WorkSpaces directory\.

1. On your directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\)\.

1. Expand the forest \(**Forest:*FQDN***\)\.

1. Expand **Domains**\. 

1. Expand your FQDN \(for example, `example.com`\)\.

1. Expand **Group Policy Objects**\.

1. Select **Default Domain Policy**, open the context \(right\-click\) menu, and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Amazon**, and **WSP**\.

1. Open the **Enable/disable audio\-in redirection** setting\.

1. In the **Enable/disable audio\-in redirection** dialog box, choose **Enabled** or **Disabled**\.

1. Choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

### Disable Time Zone Redirection for WSP beta<a name="gp_time_zone_wsp"></a>

By default, the time within a Workspace is set to mirror the time zone of the client that is being used to connect to the WorkSpace\. This behavior is controlled through time zone redirection\. You might want to turn off time zone direction for various reasons: 
+ Your company wants all employees to work in a certain time zone \(even if some employees are in other time zones\)\.
+ You have scheduled tasks in a WorkSpace that are meant to run at a certain time in a specific time zone\.
+ Your users who travel a lot want to keep their WorkSpaces in one time zone for consistency and personal preference\.

If needed for Windows WorkSpaces, you can use Group Policy settings to disable this feature\.

**To disable time zone redirection for Windows WorkSpaces**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template for WSP beta](#gp_install_template_wsp) is installed in the Central Store of the domain controller for your WorkSpaces directory\.

1. On your directory administration WorkSpace or Amazon EC2 instance that is joined to your WorkSpaces directory, open the Group Policy Management tool \(gpmc\.msc\)\.

1. Expand the forest \(**Forest:*FQDN***\)\.

1. Expand **Domains**\. 

1. Expand your FQDN \(for example, `example.com`\)\.

1. Expand **Group Policy Objects**\.

1. Select **Default Domain Policy**, open the context \(right\-click\) menu, and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Amazon**, and **WSP**\.

1. Open the **Enable/disable time zone redirection** setting\.

1. In the **Enable/disable time zone redirection** dialog box, choose **Disabled**\.

1. Choose **OK**\.

1. The Group Policy setting change takes effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy changes, do one of the following:
   + Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
   + From an administrative command prompt, type gpupdate /force\.

1. Set the time zone for the WorkSpaces to the desired time zone\.

The time zone of the WorkSpaces is now static and no longer mirrors the time zone of the client machines\. 

## Set the Maximum Lifetime for a Kerberos Ticket<a name="gp_kerberos_ticket"></a>

If you have not disabled the **Remember Me** feature of your Windows WorkSpaces, your WorkSpace users can use the **Remember Me** check box in their WorkSpaces client application to save their credentials\. This feature allows users to easily connect to their WorkSpaces while the client application remains running\. Their credentials are securely cached up to the maximum lifetime of their Kerberos tickets\.

If your WorkSpace uses an AD Connector directory, you can modify the maximum lifetime of the Kerberos tickets for your WorkSpaces users through Group Policy by following the steps in [ Maximum Lifetime for a User Ticket](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/maximum-lifetime-for-user-ticket) in the Microsoft Windows documentation\.

To enable or disable the **Remember Me** feature, see [Enable Self\-Service WorkSpace Management Capabilities for Your Users](enable-user-self-service-workspace-management.md)\.

## Configure Device Proxy Server Settings for Internet Access<a name="gp_device_proxy"></a>

By default, the WorkSpaces Windows client application uses the proxy server specified in the device operating system settings for HTTPS \(port 443\) traffic\. The Amazon WorkSpaces client applications use the HTTPS port for updates, registration, and authentication\. 

**Note**  
The desktop streaming connections to the WorkSpace require ports 4172 and 4195 to be enabled, and do not go through the proxy server\.
Proxy servers that require authentication with a username and password are not supported\.

You can configure the device proxy server settings for your Windows WorkSpaces through Group Policy by following the steps in [ Configure device proxy and internet connectivity settings](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet) in the Microsoft documentation\.

For more information about configuring the proxy settings in the WorkSpaces Windows client application, see [ Proxy Server](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-windows-client.html#windows_proxy_server) in the *Amazon WorkSpaces User Guide*\.