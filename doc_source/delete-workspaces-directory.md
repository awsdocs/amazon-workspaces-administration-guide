# Delete the directory for your WorkSpaces<a name="delete-workspaces-directory"></a>

You can delete the directory for your WorkSpaces if it is no longer in use by other WorkSpaces or other applications, such as Amazon WorkDocs, Amazon WorkMail, or Amazon Chime\. Note that you must deregister a directory before you can delete it\.

**Note**  
Simple AD and AD Connector are made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD or AD Connector directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon WorkSpaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
If you delete your Simple AD or AD Connector directory, you can always create a new one when you want to start using WorkSpaces again\.

**What happens when you delete a directory**  
When a Simple AD or AWS Directory Service for Microsoft Active Directory directory is deleted, all of the directory data and snapshots are deleted and cannot be recovered\. After the directory is deleted, any Amazon EC2 instances that are joined to the directory remain intact\. You cannot, however, use your directory credentials to log in to these instances\. You need to log in to these instances with a user account that is local to the instance\.

When an AD Connector directory is deleted, your on\-premises directory remains intact\. Any Amazon EC2 instances that are joined to the directory also remain intact and remain joined to your on\-premises directory\. You can still use your directory credentials to log in to these instances\.

**To delete a directory**

1. Delete all WorkSpaces in the directory\. For more information, see [Delete a WorkSpace](delete-workspaces.md)\.

1. Find and remove all of the applications and services that are registered to the directory\. For more information, see [Delete Your Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_delete.html) in the *AWS Directory Service Administration Guide*\.

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Deregister**\.

1. When prompted for confirmation, choose **Deregister**\.

1. Select the directory again and choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Delete**\.
**Note**  
Removing application assignments can sometimes take more time than expected\. If you receive the following error message, verify that you've removed all application assignments, and then wait 30 to 60 minutes before trying again to delete the directory:  

   ```
   An Error Has Occurred
   Cannot delete the directory because it still has authorized applications. 
   Additional directory details can be viewed at the Directory Service console.
   ```

1. \(Optional\) After you delete all resources in the virtual private cloud \(VPC\) for your directory, you can delete the VPC and release the Elastic IP address used for the NAT gateway\. For more information, see [ Deleting your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#VPC_Deleting) and [ Working with Elastic IP addresses](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html#WorkWithEIPs) in the *Amazon VPC User Guide*\.

1. \(Optional\) To delete any custom bundles and images that you are finished with, see [Delete a custom WorkSpaces bundle or image](delete_bundle.md)\.