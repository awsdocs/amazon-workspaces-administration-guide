# Restart a WorkSpace<a name="reboot-workspaces"></a>

Occasionally, you might need to restart a WorkSpace manually\. Restarting a WorkSpace performs a shutdown and reboot of the WorkSpace\. To avoid data loss, make sure users save any open documents and other application files before you restart the WorkSpace\. The user data, operating system, and system settings are not affected\.

**Warning**  
To reboot an encrypted WorkSpace, first make sure that the AWS KMS CMK is enabled; otherwise, the WorkSpace becomes unusable\. To determine whether a CMK is enabled, see [ Displaying CMK Details](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys-console.html#viewing-console-details) in the *AWS Key Management Service Developer Guide*\.

**To restart a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpaces to be restarted and choose **Actions**, **Reboot WorkSpaces**\.

1. When prompted for confirmation, choose **Reboot WorkSpaces**\.