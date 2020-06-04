# Enable Self\-Service WorkSpace Management Capabilities for Your Users<a name="enable-user-self-service-workspace-management"></a>

In Amazon WorkSpaces, you can enable self\-service WorkSpace management capabilities for your users to provide them with more control over their experience\. It can also reduce your IT support staff workload for Amazon WorkSpaces\. When you enable self\-service capabilities, you can allow users to perform one or more of the following tasks directly from their Windows, macOS, or Linux client for Amazon WorkSpaces:
+ Cache their credentials on their client\. This lets them reconnect to their WorkSpace without re\-entering their credentials\.
+ Restart their WorkSpace\.
+ Increase the size of the root and user volumes on their WorkSpace\. 
+ Change the compute type \(bundle\) for their WorkSpace\.
+ Switch the running mode of their WorkSpace\.
+ Rebuild their WorkSpace\.

To enable one or more of these capabilities for your users, perform the following steps\.

**To enable self\-service management capabilities for your users**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory, and choose **Actions**, **Update Details**\.

1. Expand **User Self\-Service Permissions**\. Enable or disable the following options as required to determine the WorkSpace management tasks that users can perform from their client:
   + **Remember me** — Users can choose whether to cache their credentials on their client by selecting the **Remember Me** or **Keep me logged in** check box on the login screen\. The credentials are cached in RAM only\. When users choose to cache their credentials, they can reconnect to their WorkSpaces without re\-entering their credentials\. To control how long users can cache their credentials, see [Set the Maximum Lifetime for a Kerberos Ticket](group_policy.md#gp_kerberos_ticket)\.
   + **Restart WorkSpace from client**— Users can restart their WorkSpace\. Restarting disconnects the user from their WorkSpace, shuts it down, and restarts it\. The user data, operating system, and system settings are not affected\.
   + **Increase volume size** — Users can expand the root and user volumes on their WorkSpace to a specified size without contacting IT support\. Users can increase the size of the root volume \(for Windows, the C: drive; for Linux, /\) up to 175 GB, and the size of the user volume \(for Windows, the D: drive; for Linux, /home\) up to 100 GB\. WorkSpace root and user volumes come in set groups that can't be changed\. The available groups are \[Root\(GB\), User\(GB\)\]: \[80, 10\], \[80, 50\], \[80, 100\], \[175 to 2000, 100 to 2000\]\. For more information, see [Modify a WorkSpace](modify-workspaces.md)\.

     For a newly created WorkSpace, users must wait 6 hours before they can increase the size of these drives\. After that, they can do so only once in a 6\-hour period\. While a volume size increase is in progress, users can perform most tasks on their WorkSpace\. The tasks that they can't perform are: changing their WorkSpace compute type, switching their WorkSpace running mode, restarting their WorkSpace, or rebuilding their WorkSpace\. When the process is finished, the WorkSpace must be rebooted for the changes to take effect\. This process might take up to an hour\.
**Note**  
If users increase the volume size on their WorkSpace, this will increase the billing rate for their WorkSpace\.
   + **Change compute type** — Users can switch their WorkSpace between compute types \(bundles\)\. For a newly created WorkSpace, users must wait 6 hours before they can switch to a different bundle\. After that, they can switch to a larger bundle only once in a 6\-hour period, or to a smaller bundle once in a 30\-day period\. When a WorkSpace compute type change is in progress, users are disconnected from their WorkSpace, and they can't use or change the WorkSpace\. The WorkSpace is automatically rebooted during the compute type change process\. This process might take up to an hour\.
**Note**  
If users change their WorkSpace compute type, this will change the billing rate for their WorkSpace\.
   + **Switch running mode** — Users can switch their WorkSpace between the **AlwaysOn** and **AutoStop** running modes\. For more information, see [Manage the WorkSpace Running Mode](running-mode.md)\.
**Note**  
If users switch the running mode of their WorkSpace, this will change the billing rate for their WorkSpace\.
   + **Rebuild WorkSpace from client** — Users can rebuild the operating system of a WorkSpace to its original state\. When a WorkSpace is rebuilt, the user volume \(D: drive\) is recreated from the latest backup\. Because backups are completed every 12 hours, users' data might be up to 12 hours old\. For a newly created WorkSpace, users must wait 12 hours before they can rebuild their WorkSpace\. When a WorkSpace rebuild is in progress, users are disconnected from their WorkSpace, and they can't use or make changes to their WorkSpace\. This process might take up to an hour\. 

1. Choose **Update** or **Update and Exit**\.