# Manage Your WorkSpaces Using Group Policy<a name="group_policy"></a>

You can apply Group Policy settings to the WorkSpaces or users that are part of your WorkSpaces directory\.

We recommend that you create an organizational unit for your WorkSpaces machine accounts and an organizational unit for your WorkSpaces user accounts\.

Group Policy settings can affect a WorkSpace user's experience as follows:
+ Depending on the number of custom Group Policy settings applied to a WorkSpace, a user's first login to their WorkSpace after it is launched or rebooted can take several minutes\.
+ Changes to Group Policy settings can cause an active session to be closed when a user is not connected to the WorkSpace\.
+ Some Group Policy settings force a user to log off when they are disconnected from a session\. Any applications that a user has open on the WorkSpace are closed\.
+ Implementing an interactive logon message to display a logon banner prevents users from being able to access their WorkSpace\. The interactive logon message Group Policy setting is not currently supported by Amazon WorkSpaces\.

**Topics**
+ [Install the Group Policy Administrative Template](#gp_install_template)
+ [Local Printer Support](#gp_local_printers)
+ [Clipboard Redirection](#gp_clipboard)
+ [Setting the Session Resume Timeout](#gp_auto_resume)

## Install the Group Policy Administrative Template<a name="gp_install_template"></a>

To use the Group Policy settings that are specific to Amazon WorkSpaces, you need to install the Group Policy administrative template\. Perform the following procedure on a directory administration WorkSpace or Amazon EC2 instance that is joined to your directory\.

**To install the Group Policy administrative template**

1. From a running WorkSpace, make a copy of the `pcoip.adm` file in the `C:\Program Files (x86)\Teradici\PCoIP Agent\configuration` directory\.

1. Open the Group Policy Management tool and navigate to the organizational unit in your domain that contains your WorkSpaces machine accounts\.

1. Open the context \(right\-click\) menu for the machine account organizational unit and choose **Create a GPO in this domain, and link it here**\.

1. In the **New GPO** dialog box, enter a descriptive name for the Group Policy object, such as **WorkSpaces Machine Policies**, and leave **Source Starter GPO** set to **\(none\)**\. Choose **OK**\.

1. Open the context \(right\-click\) menu for the new Group Policy object and choose **Edit**\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, and **Administrative Templates**\. Choose **Action**, **Add/Remove Templates** from the main menu\. 

1. In the **Add/Remove Templates** dialog box, choose **Add**, select the `pcoip.adm` file copied previously, and then choose **Open**, **Close**\.

1. Close the Group Policy Management Editor\. You can now use this Group Policy object to modify the Group Policy settings that are specific to Amazon WorkSpaces\.

## Local Printer Support<a name="gp_local_printers"></a>

By default, Amazon WorkSpaces disables local printer redirection\. You can use Group Policy settings to enable this feature if needed\.

The Group Policy setting change takes effect after the WorkSpace's next Group Policy settings update and the session is restarted\.

**To enable or disable local printer support**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administration Defaults**\.

1. Open the **Configure remote printing** setting\.

1. In the **Configure remote printing** dialog box, choose **Enabled** or **Disabled**, and then choose **OK**\.

By default, local printer auto\-redirection is disabled\. You can use Group Policy settings to enable this feature so that your local printer is set as the default printer every time you connect to your WorkSpace\.

**To enable or disable local printer auto\-redirection**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administration Defaults**\.

1. Open the **Configure remote printing** setting\.

1. In the **Configure remote printing** dialog box, choose **Enabled**, set or clear **Automatically set default printer**, and then choose **OK**\.

## Clipboard Redirection<a name="gp_clipboard"></a>

By default, Amazon WorkSpaces supports clipboard redirection\. You can use Group Policy settings to disable this feature if needed\. 

The Group Policy setting change takes effect after the WorkSpace's next Group Policy settings update and the session is restarted\.

**To enable or disable clipboard redirection**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**,**Classic Administrative Templates**, **PCoIP Session Variables**, and **Overridable Administration Defaults**\.

1. Open the **Configure clipboard redirection** setting\.

1. In the **Configure clipboard redirection** dialog box, choose **Enabled** and set the **Configure clipboard redirection** option to the desired setting, enabled or disabled, and choose **OK**\. 

**Known Limitation**  
With clipboard redirection enabled on the WorkSpace, if you copy content that is larger than 890KB from a Microsoft Office application, the application might become slow or unresponsive for up to 5 seconds\.

## Setting the Session Resume Timeout<a name="gp_auto_resume"></a>

When using the Amazon WorkSpaces client applications, an interruption of network connectivity causes an active session to be disconnected\. This can be caused by events such as closing the laptop lid, or the loss of your wireless network connection\. The Amazon WorkSpaces client applications for Windows and OS X attempt to reconnect the session automatically if network connectivity is regained within a certain amount of time\. The default session resume timeout is 20 minutes, but you can modify that value for WorkSpaces that are controlled by your domain's Group Policy settings\.

The Group Policy setting change takes effect after the WorkSpace's next Group Policy settings update and the session is restarted\.

**To set the automatic session resume timeout value**

1. Make sure that the most recent [Amazon WorkSpaces Group Policy administrative template](#gp_install_template) is installed in your domain\.

1. Open the Group Policy Management tool and navigate to and select the WorkSpaces Group Policy object for your WorkSpaces machine accounts\. Choose **Action**, **Edit** in the main menu\.

1. In the Group Policy Management Editor, choose **Computer Configuration**, **Policies**, **Administrative Templates**, **Classic Administrative Templates**, and **PCoIP Session Variables**\.

   To allow the user to override your setting, choose **Overridable Administration Defaults**; otherwise, choose **Not Overridable Administration Defaults**\.

1. Open the **Configure Session Automatic Reconnection Policy** setting\.

1. In the **Configure Session Automatic Reconnection Policy** dialog box, choose **Enabled**, set the **Configure Session Automatic Reconnection Policy** option to the desired timeout, in minutes, and choose **OK**\. 