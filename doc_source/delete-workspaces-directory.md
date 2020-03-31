# Delete the Directory for Your WorkSpaces<a name="delete-workspaces-directory"></a>

You can delete the directory for your WorkSpaces if it is no longer in use by other WorkSpaces or other applications, such as Amazon WorkDocs, Amazon WorkMail, or Amazon Chime\. Note that you must deregister a directory before you can delete it\.

**What Happens When a Directory Is Deleted**  
When a Simple AD or AWS Directory Service for Microsoft Active Directory directory is deleted, all of the directory data and snapshots are deleted and cannot be recovered\. After the directory is deleted, all instances that are joined to the directory remain intact\. You cannot, however, use your directory credentials to log in to these instances\. You need to log in to these instances with a user account that is local to the instance\.

When an AD Connector directory is deleted, your on\-premises directory remains intact\. All instances that are joined to the directory also remain intact and remain joined to your on\-premises directory\. You can still use your directory credentials to log in to these instances\.

**To delete a directory**

1. Find and remove all of the applications and services that are registered to the directory\. For more information, see [Delete Your Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_delete.html) in the *AWS Directory Service Administration Guide*\.

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Deregister**\.

1. When prompted for confirmation, choose **Deregister**\.

1. Select the directory again and choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Delete**\.