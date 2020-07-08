# Set Up Active Directory Administration Tools for Amazon WorkSpaces<a name="directory_administration"></a>

You'll perform most administrative tasks for your WorkSpaces directory using directory management tools, such as the Active Directory Administration Tools\. However, you'll use Amazon WorkSpaces console to perform some directory\-related tasks\. For more information, see [Manage Directories for Amazon WorkSpaces](manage-workspaces-directory.md)\.

If you create a directory with Microsoft AD or Simple AD that includes five or more WorkSpaces, we recommend that you centralize administration on an Amazon EC2 instance\. Although you can install the directory management tools on a WorkSpace, using an Amazon EC2 instance is a more robust solution\.

**To set up the Active Directory Administration Tools**

1. Launch a Windows instance and join it to your WorkSpaces directory\.

   You can join an Amazon EC2 Windows instance to your directory domain when you launch the instance\. For more information, see [Joining a Windows Instance to an AWS Directory Service Domain](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-join-aws-domain.html) in the *Amazon EC2 User Guide for Windows Instances*\.

   Alternatively, you can join the instance to your directory manually\. For more information, see [Manually Add a Windows Instance \(Simple AD and Microsoft AD\)](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/join_windows_instance.html) in the *AWS Directory Service Administration Guide*\.

1. Install the Active Directory Administration Tools on the instance\. For more information, see [Installing the Active Directory Administration Tools](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/install_ad_tools.html) in the *AWS Directory Service Administration Guide*\.

1. Run the tools as a directory administrator as follows:

   1. Open the **Administrative Tools**\.

   1. Hold down the Shift key, right\-click the tool shortcut, and choose **Run as different user**\.

   1. Type the username and password for the administrator\. With Simple AD, the username is **Administrator** and with Microsoft AD, the administrator is **Admin**\.

You can now perform directory administration tasks using the Active Directory tools that you are familiar with\. For example, you can use the Active Directory Users and Computers Tool to add users, remove users, promote a user to directory administrator, or reset a user password\. Note that you must be logged into your Windows instance as a user that has permissions to manage users in the directory\.

**To promote a user to a directory administrator**
**Note**  
This procedure applies only to directories created with Simple AD, not AWS Managed AD\. For directories created with AWS Managed AD, see [ Manage Users and Groups in AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_manage_users_groups.html) in the *AWS Directory Service Administration Guide*\.

1. Open the Active Directory Users and Computers tool\.

1. Navigate to the **Users** folder under your domain and select the user to promote\.

1. Choose **Action**, **Properties**\.

1. In the user properties dialog box, choose **Member of**\.

1. Add the user to the following groups and choose **OK**\.
   + Administrators
   + Domain Admins
   + Enterprise Admins
   + Group Policy Creator Owners
   + Schema Admins

**To add or remove users**  
You can create new users from the Amazon WorkSpaces console only during the process of launching a WorkSpace, and you cannot delete users through the Amazon WorkSpaces console\. Most user management tasks, including managing user groups, must be performed through your directory\. 

**Important**  
Before you can remove a user, you must delete the WorkSpace assigned to that user\. For more information, see [Delete a WorkSpace](delete-workspaces.md)\.

The process you use for managing users and groups depends on which type of directory you're using\.
+ If you're using AWS Managed Microsoft AD, see [ Manage Users and Groups in AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_manage_users_groups.html) in the *AWS Directory Service Administration Guide*\.
+ If you're using Simple AD, see [ Manage Users and Groups in Simple AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/simple_ad_manage_users_groups.html) in the *AWS Directory Service Administration Guide*\. 
+ If you use Microsoft Active Directory through AD Connector or a trust relationship, you can manage users and groups by using [ Active Directory](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps)\. 

**To reset a user password**  
When you reset the password for an existing user, do not set **User must change password at next logon**\. Otherwise, the users cannot connect to their WorkSpaces\. Instead, assign a secure temporary password to each user and then ask the users to manually change their passwords from within the WorkSpace the next time they log on\.

**Note**  
If you're using AD Connector, your users won't be able to reset their own passwords\. \(The **Forgot password?** option on the WorkSpaces client application login screen won't be available\.\)