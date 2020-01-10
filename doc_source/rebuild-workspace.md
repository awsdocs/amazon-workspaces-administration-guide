# Rebuild a WorkSpace<a name="rebuild-workspace"></a>

Automatic snapshots for use when rebuilding a WorkSpace are scheduled every 12 hours\. If the WorkSpace is healthy, a snapshot of the user volume is created\. If the WorkSpace is unhealthy, the snapshot is not created\.

If needed, you can rebuild a WorkSpace\. This recreates both the root volume and the user volume\.

**Note**  
After January 14, 2020, WorkSpaces created from a public Windows 7 bundle can no longer be rebuilt\. You might want to consider migrating your Windows 7 WorkSpaces to Windows 10\. For more information, see [Migrate a WorkSpace](migrate-workspaces.md)\.

Rebuilding a WorkSpace causes the following to occur:
+ The system is refreshed with the most recent image of the bundle that the WorkSpace was created from\. Any applications that were installed, or system settings that were changed after the WorkSpace was created are lost\.
+ The user volume \(for Microsoft Windows, the D: drive; for Linux, /home\) is recreated from the most recent snapshot\. The current contents of the user volume are overwritten\.
+ The primary elastic network interface \(ENI\) is recreated\. The WorkSpace receives a new private IP address\.

**To rebuild a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to be rebuilt and choose **Actions**, **Rebuild / Restore WorkSpace**\.

1. When prompted for confirmation, choose **Rebuild WorkSpace**\.