# Restore a WorkSpace<a name="restore-workspace"></a>

Automatic snapshots for use when restoring a WorkSpace are scheduled every 12 hours\. If the WorkSpace is healthy, snapshots of both the root volume and user volume are created around the same time\. If the WorkSpace is unhealthy, these snapshots are not created\.

If needed, you can restore a WorkSpace to its last known healthy state\. This recreates both the root volume and user volume, based on the most recent snapshots of these volumes that were created when the WorkSpace was healthy\.

You cannot restore a WorkSpace unless its state is `AVAILABLE`, `ERROR`, `UNHEALTHY`, or `STOPPED`\.

Restoring a WorkSpace causes the following to occur:
+ The system is restored to the most recent snapshot of the root volume\. Any applications that were installed, or system settings that were changed after the most recent snapshot was created, are lost\.
+ The user volume \(for Microsoft Windows, the D drive; for Linux, /home\) is recreated from the most recent snapshot\. The current contents of the user volume are overwritten\.

**To restore a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to be restored and choose **Actions**, **Rebuild / Restore WorkSpace**\.

1. Choose **Restore WorkSpace**\.