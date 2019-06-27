# Manage Your Windows WorkSpaces Using Group Policy<a name="group_policy"></a>

You can use Group Policy objects to apply settings to manage Windows WorkSpaces or users that are part of your Windows WorkSpaces directory\.

**Note**  
Linux instances do not adhere to Group Policy\. For information about managing Amazon Linux WorkSpaces, see [Manage Your Amazon Linux WorkSpaces](manage_linux_workspace.md)\. 

We recommend that you create an organizational unit for your WorkSpaces Computer Objects and an organizational unit for your WorkSpaces User Objects\.

Group Policy settings can affect a WorkSpace user's experience as follows:
+ Some Group Policy settings force a user to log off when they are disconnected from a session\. Any applications that a user has open on the WorkSpace are closed\.
+ Implementing an interactive logon message to display a logon banner prevents users from being able to access their WorkSpace\. The interactive logon message Group Policy setting is not currently supported by Amazon WorkSpaces\.
+ Group Policy can be used to restrict drive access\. If you configure Group Policy to restrict access to Drive C or to Drive D, users can't access their WorkSpace\. To prevent this issue from occurring, make sure that your users can access Drive C and Drive D\. 

**Topics**
+ [Install the Group Policy Administrative Template](#gp_install_template)
+ [Enable or Disable Local Printer Support for Windows WorkSpaces](#gp_local_printers)
+ [Enable or Disable Clipboard Redirection for Windows WorkSpaces](#gp_clipboard)
+ [Set the Session Resume Timeout for Windows WorkSpaces](#gp_auto_resume)

## Install the Group Policy Administrative Template<a name="gp_install_template"></a>

To use the Group Policy settings that are specific to Amazon WorkSpaces, you need to install the Group Policy administrative template\. Perform the following procedure on a directory administration WorkSpace or Amazon EC2 instance that is joined to your directory\.

**To install the Group Policy administrative template**

1. From a running Windows WorkSpace, make a copy of the `pcoip.adm` file in the `C:\Program Files (x86)\Teradici\PCoIP Agent\configuration` directory\.

1. Open the Group Policy Management tool and navigate to the organizational unit in your domain that contains your WorkSpaces machine accounts\.

1. Open the context \(right\-click\) menu for the machine account organizational unit and choose **Create a GPO in this domain, and link it here**\.

1. In the **New GPO** dialog box, enter a descriptive name for the Group Policy object, such as **WorkSpaces Machine Policies**, and leave **Source Starter GPO** set to **\(none\)**\. Choose **OK**\.

1. Open the context \(right\-click\) menu for the new Group Policy object and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, and **Administrative Templates**\. Choose **Action**, **Add/Remove Templates** from the main menu\. 

1. In the **Add/Remove Templates** dialog box, choose **Add**, select the `pcoip.adm` file copied previously, and then choose **Open**, **Close**\.

1. Close the Group Policy Management Editor\. You can now use this Group Policy object to modify the Group Policy settings that are specific to Amazon WorkSpaces\.

## Enable or Disable Local Printer Support for Windows WorkSpaces<a name="gp_local_printers"></a>

By default, Amazon WorkSpaces disables local printer redirection\. For Windows WorkSpaces, you can use Group Policy settings to enable this feature if needed\.

**Note**  
Local printer support is not available for Amazon Linux Workspaces\. 

The Group Policy setting change takes effect after the WorkSpace's next Group Policy settings update and the session is restarted\.

**To enable or disable local printer support**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administration Defaults**\.

1. Open the **Configure remote printing** setting\.

1. In the **Configure remote printing** dialog box, choose **Enabled** or **Disabled**, and then choose one of the following settings:

By default, local printer auto\-redirection is disabled\. You can use Group Policy settings to enable this feature so that your local printer is set as the default printer every time you connect to your WorkSpace\.

**To enable local printer auto\-redirection**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administration Defaults**\.

1. Open the **Configure remote printing** setting\.

1. In the **Configure remote printing** dialog box, choose **Enabled**, set or clear **Automatically set default printer**, and then choose **OK**\.

## Enable or Disable Clipboard Redirection for Windows WorkSpaces<a name="gp_clipboard"></a>

By default, Amazon WorkSpaces supports clipboard redirection\. You can use Group Policy settings to disable this feature if needed for Windows WorkSpaces\. 

The Group Policy setting change takes effect after the WorkSpace's next Group Policy settings update and the session is restarted\.

**To enable or disable clipboard redirection for Windows WorkSpaces**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**,**Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administration Defaults**\.

1. Open the **Configure clipboard redirection** setting\.

1. In the **Configure clipboard redirection** dialog box, choose **Enabled** and then choose one of the following settings to determine the direction in which clipboard redirection is allowed\. When you're done, choose **OK**\.
   + Disabled in both directions
   + Enabled agent to client only \(WorkSpace to local computer\)
   + Enabled client to agent only \(local computer to WorkSpace\)
   + Enabled in both directions 

**Known Limitation**  
With clipboard redirection enabled on the WorkSpace, if you copy content that is larger than 890KB from a Microsoft Office application, the application might become slow or unresponsive for up to 5 seconds\.

## Set the Session Resume Timeout for Windows WorkSpaces<a name="gp_auto_resume"></a>

When using the Amazon WorkSpaces client applications, an interruption of network connectivity causes an active session to be disconnected\. This can be caused by events such as closing the laptop lid, or the loss of your wireless network connection\. The Amazon WorkSpaces client applications for Windows and MacOS X attempt to reconnect the session automatically if network connectivity is regained within a certain amount of time\. The default session resume timeout is 20 minutes, but you can modify that value for WorkSpaces that are controlled by your domain's Group Policy settings\.

The Group Policy setting change takes effect after the WorkSpace's next Group Policy settings update and the session is restarted\.

**To set the automatic session resume timeout value**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, and **PCoIP Session Variables**\.

   To allow the user to override your setting, choose **Overridable Administration Defaults**; otherwise, choose **Not Overridable Administration Defaults**\.

1. Open the **Configure Session Automatic Reconnection Policy** setting\.

1. In the **Configure Session Automatic Reconnection Policy** dialog box, choose **Enabled**, set the **Configure Session Automatic Reconnection Policy** option to the desired timeout, in minutes, and choose **OK**\. 