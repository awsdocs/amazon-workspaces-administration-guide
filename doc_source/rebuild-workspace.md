# Rebuild a WorkSpace<a name="rebuild-workspace"></a>

If needed, you can rebuild a WorkSpace\. This recreates the root volume, the user volume, and the primary elastic network interface\.

Rebuilding a WorkSpace causes the following to occur:
+ The root volume \(for Microsoft Windows, drive C; for Linux, /\) is refreshed with the most recent image of the bundle that the WorkSpace was created from\. Any applications that were installed, or system settings that were changed after the WorkSpace was created, are lost\.
+ The user volume \(for Microsoft Windows, the D drive; for Linux, /home\) is recreated from the most recent snapshot\. The current contents of the user volume are overwritten\.

  Automatic snapshots for use when rebuilding a WorkSpace are scheduled every 12 hours\. These snapshots of the user volume are taken regardless of the health of the WorkSpace\. When you choose **Actions**, **Rebuild / Restore WorkSpace**, the date and time of the most recent snapshot is shown\.
+ The primary elastic network interface is recreated\. The WorkSpace receives a new private IP address\.

**Important**  
After January 14, 2020, WorkSpaces created from a public Windows 7 bundle can no longer be rebuilt\. You might want to consider migrating your Windows 7 WorkSpaces to Windows 10\. For more information, see [Migrate a WorkSpace](migrate-workspaces.md)\.

You can rebuild a WorkSpace only if the following conditions are met:
+ The WorkSpace must have a state of `AVAILABLE`, `ERROR`, `UNHEALTHY`, `STOPPED`, or `REBOOTING`\. To rebuild a WorkSpace in the `REBOOTING` state, you must use the [ RebuildWorkspaces](https://docs.aws.amazon.com/workspaces/latest/api/API_RebuildWorkspaces.html) API operation or the [ rebuild\-workspaces](https://docs.aws.amazon.com/cli/latest/reference/workspaces/rebuild-workspaces.html) AWS Command Line Interface \(CLI\) command\.
+ A snapshot of the user volume must exist\.

**To rebuild a WorkSpace**
**Warning**  
To rebuild an encrypted WorkSpace, first make sure that the AWS KMS CMK is enabled; otherwise, the WorkSpace becomes unusable\. To determine whether a CMK is enabled, see [ Displaying CMK Details](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys-console.html#viewing-console-details) in the *AWS Key Management Service Developer Guide*\.

1. Open the Workspaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to be rebuilt and choose **Actions**, **Rebuild / Restore WorkSpace**\.

1. Select the **Rebuild WorkSpace** option\.

1. Choose **Rebuild / Restore WorkSpace**\.

**Note**  
If you rebuild a WorkSpace after changing the user's **sAMAccountName** user naming attribute in Active Directory, you might receive the following error message:  

```
"ErrorCode": "InvalidUserConfiguration.Workspace"
"ErrorMessage": "The user was either not found or is misconfigured."
```
To work around this issue, either revert to the original user naming attribute and then reinitiate the rebuild, or create a new WorkSpace for that user\.