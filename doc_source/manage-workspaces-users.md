# Manage WorkSpaces Users<a name="manage-workspaces-users"></a>

As an administrator for Amazon WorkSpaces, you can perform the following tasks to manage WorkSpaces users\.

## Edit User Information<a name="edit-user"></a>

You can use the Amazon WorkSpaces console to edit the user information for a WorkSpace\.

**Note**  
This feature is available only if you use AWS Managed Microsoft AD or Simple AD\. If you use Microsoft Active Directory through AD Connector or a trust relationship, you can manage users and groups by using [ Active Directory](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps)\.

**To edit user information**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select a user and choose **Actions**, **Edit User**\.

1. Update **First Name**, **Last Name**, and **Email** as needed\.

1. Choose **Update**\.

## Add or Delete Users<a name="add-delete-user"></a>

You can create new users from the Amazon WorkSpaces console only during the process of launching a WorkSpace, and you cannot delete users through the Amazon WorkSpaces console\. Most user management tasks, including managing user groups, must be performed through your directory\.

**To add or delete users and groups**  
To add, delete, or otherwise manage users and groups, you must do this through your directory\. You'll perform most administrative tasks for your WorkSpaces directory using directory management tools, such as the Active Directory Administration Tools\. For more information, see [Set Up Active Directory Administration Tools for Amazon WorkSpaces](directory_administration.md)\.

**Important**  
Before you can remove a user, you must delete the WorkSpace assigned to that user\. For more information, see [Delete a WorkSpace](delete-workspaces.md)\.

The process you use for managing users and groups depends on which type of directory you're using\.
+ If you're using AWS Managed Microsoft AD, see [ Manage Users and Groups in AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_manage_users_groups.html) in the *AWS Directory Service Administration Guide*\.
+ If you're using Simple AD, see [ Manage Users and Groups in Simple AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/simple_ad_manage_users_groups.html) in the *AWS Directory Service Administration Guide*\. 
+ If you use Microsoft Active Directory through AD Connector or a trust relationship, you can manage users and groups by using [ Active Directory](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps)\. 

## Send an Invitation Email<a name="send-invitation"></a>

You can send an invitation email to a user manually if needed\.

**Note**  
If you're using AD Connector, welcome emails aren't automatically sent to your users, so you must send them manually\.

**To resend an invitation email**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. On the **WorkSpaces** page, use the search box to search for the user you want to send an invitation to, and then select the corresponding WorkSpace from the search results\. You can select only one WorkSpace at a time\.

1. Choose **Actions**, **Invite User**\.

1. Copy the email body text and paste it into an email to the user using your own email application\. You can modify the body text if desired\. When the invitation email is ready, send it to the user\.