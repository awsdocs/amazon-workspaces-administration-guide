# Enable and Configure Amazon WorkSpaces Web Access<a name="web-access"></a>

Most WorkSpaces bundles support Amazon WorkSpaces Web Access through Chrome or Firefox browsers\. For a list of WorkSpaces that support web browser access, see "Which Amazon WorkSpaces bundles support Web Access?" in [ Client Access, Web Access, and User Experience](https://aws.amazon.com/workspaces/faqs/#Client_Access.2C_Web_Access.2C_and_User_Experience)\.

**Note**  
A web browser cannot be used to connect to Amazon Linux WorkSpaces\.

**Important**  
Beginning October 1, 2020, customers will no longer be able to use the Amazon WorkSpaces Web Access client to connect to Windows 7 custom WorkSpaces or to Windows 7 Bring Your Own License \(BYOL\) WorkSpaces\.

## Step 1: Enable Web Access to Your WorkSpaces<a name="enable-web-access"></a>

You control Web Access to your WorkSpaces at the directory level\. For each directory containing WorkSpaces that you want to allow users to access through the Web Access client, do the following steps\.

**To enable Web Access to your WorkSpaces**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Choose the appropriate directory, and then choose **Actions**, **Update Details**\.

1. Expand **Access Control Options** and find the **Other Platforms** section\.

1. Choose **Web Access**\.

1. Choose **Update and Exit**\.

## Step 2: Configure Inbound and Outbound Access to Ports for Web Access<a name="configure_inbound_outbound"></a>

Amazon WorkSpaces Web Access requires inbound and outbound access for certain ports\. For more information, see [Ports for Web Access](workspaces-port-requirements.md#web-access-ports)\.

## Step 3: Configure Group Policy and Security Policy Settings to Enable Users to Log On<a name="configure_group_policy"></a>

Amazon WorkSpaces relies on a specific logon screen configuration to enable users to successfully log on from their Web Access client\.

To enable Web Access users to log on to their WorkSpaces, you must configure a Group Policy setting and three Security Policy settings\. If these settings are not correctly configured, users might experience long logon times or black screens when they try to log on to their WorkSpaces\. To configure these settings, use the following procedures\. 

You can use Group Policy Objects \(GPOs\) to apply settings to manage Windows WorkSpaces or users that are part of your Windows WorkSpaces directory\. We recommend that you create an organizational unit for your WorkSpaces Computer Objects and an organizational unit for your WorkSpaces User Objects\.

For information about using the Active Directory administration tools to work with GPOs, see [ Installing the Active Directory Administration Tools](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_install_ad_tools.html) in the *AWS Directory Service Administration Guide*\.

**To enable the WorkSpaces logon agent to switch users**

In most cases, when a user attempts to log on to a WorkSpace, the user name field is prepopulated with the name of that user\. However, if an administrator has established an RDP connection to the WorkSpace to perform maintenance tasks, the user name field is populated with the name of the administrator instead\.

To avoid this issue, disable the **Hide entry points for Fast User Switching** Group Policy setting\. When you disable this setting, the WorkSpaces logon agent can use the **Switch User** button to populate the user name field with the correct name\.

1. Open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select a GPO at the domain or domain controller level of the directory that you use for your WorkSpaces\. \(If you have the [ Amazon WorkSpaces Group Policy administrative template](group_policy.md#gp_install_template) installed in your domain, you can use the WorkSpaces GPO for your WorkSpaces machine accounts\.\)

1. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **System**, and **Logon**\. 

1. Open the **Hide entry points for Fast User Switching** setting\.

1. In the **Hide entry points for Fast User Switching** dialog box, choose **Disabled**, and then choose **OK**\.

**To hide the last logged on user name**

By default, the list of last logged on users is displayed instead of the **Switch User** button\. Depending on the configuration of the WorkSpace, the list might not display the **Other User** tile\. When this situation occurs, if the prepopulated user name isn't correct, the WorkSpaces logon agent can't populate the field with the correct name\.

To avoid this issue, enable the Security Policy setting **Interactive logon: Don't display last signed\-in** or **Interactive logon: Do not display last user name** \(depending on which version of Windows you're using\)\.

1. Open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select a GPO at the domain or domain controller level of the directory that you use for your WorkSpaces\. \(If you have the [ Amazon WorkSpaces Group Policy administrative template](group_policy.md#gp_install_template) installed in your domain, you can use the WorkSpaces GPO for your WorkSpaces machine accounts\.\)

1. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Windows Settings**, **Security Settings**, **Local Policies**, and **Security Options**\. 

1. Open one of the following settings:
   + For Windows 7 — **Interactive logon: Don't display last signed\-in**
   + For Windows 10 — **Interactive logon: Do not display last user name**

1. In the **Properties** dialog box for the setting, choose **Enabled**, and then choose **OK**\.

**To require pressing CTRL\+ALT\+DEL before users can log on**

For WorkSpaces Web Access, you need to require that users press CTRL\+ALT\+DEL before they can log on\. Requiring users to press CTRL\+ALT\+DEL before they log on ensures that users are using a trusted path when they're entering their passwords\.

1. Open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select a GPO at the domain or domain controller level of the directory that you use for your WorkSpaces\. \(If you have the [ Amazon WorkSpaces Group Policy administrative template](group_policy.md#gp_install_template) installed in your domain, you can use the WorkSpaces GPO for your WorkSpaces machine accounts\.\)

1. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Windows Settings**, **Security Settings**, **Local Policies**, and **Security Options**\. 

1. Open the **Interactive logon: Do not require CTRL\+ALT\+DEL** setting\.

1. On the **Local Security Setting** tab, choose **Disabled**, and then choose **OK**\.

**To display the domain and user information when the session is locked**

The WorkSpaces logon agent looks for the user's name and domain\. After this setting is configured, the lock screen will display the user's full name \(if it is specified in Active Directory\), their domain name, and their user name\.

1. Open the Group Policy Management tool \(gpmc\.msc\) and navigate to and select a GPO at the domain or domain controller level of the directory that you use for your WorkSpaces\. \(If you have the [ Amazon WorkSpaces Group Policy administrative template](group_policy.md#gp_install_template) installed in your domain, you can use the WorkSpaces GPO for your WorkSpaces machine accounts\.\)

1. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Windows Settings**, **Security Settings**, **Local Policies**, and **Security Options**\. 

1. Open the **Interactive logon: Display user information when the session is locked** setting\.

1. On the **Local Security Setting** tab, choose **User display name, domain and user names**, and then choose **OK**\.

**To apply the Group Policy and Security Policy settings changes**  
Group Policy and Security Policy settings changes take effect after the next Group Policy update for the WorkSpace and after the WorkSpace session is restarted\. To apply the Group Policy and Security Policy changes in the prior procedures, do one of the following:
+ Reboot the WorkSpace \(in the Amazon WorkSpaces console, select the WorkSpace, then choose **Actions**, **Reboot WorkSpaces**\)\.
+ From an administrative command prompt, enter gpupdate /force\.