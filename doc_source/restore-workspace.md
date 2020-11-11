# Restore a WorkSpace<a name="restore-workspace"></a>

If needed, you can restore a WorkSpace to its last known healthy state\. This recreates both the root volume and user volume, based on the most recent snapshots of these volumes that were created when the WorkSpace was healthy\.

Restoring a WorkSpace causes the following to occur:
+ The root volume \(for Microsoft Windows, drive C; for Linux, /\) is restored to the most recent snapshot\. Any applications that were installed, or system settings that were changed after the most recent snapshot was created, are lost\.
+ The user volume \(for Microsoft Windows, the D drive; for Linux, /home\) is recreated from the most recent snapshot\. The current contents of the user volume are overwritten\.

**When Snapshots Are Taken**  
Snapshots of the root and user volume are taken on the following basis\. When you choose **Actions**, **Rebuild / Restore WorkSpace**, the date and time of the most recent snapshots are shown\. 
+ **After a WorkSpace is first created** — Typically, the initial snapshots of the root and user volumes are taken soon after a WorkSpace is created \(often within 30 minutes\)\. In some AWS Regions, it might take several hours for the initial snapshots to be taken after a WorkSpace is created\.

  If a WorkSpace becomes unhealthy before the initial snapshots are taken, the WorkSpace can't be restored\. In that case, you can try [ rebuilding the WorkSpace](rebuild-workspace.md) or contact AWS Support for assistance\.
+ **During regular use** — Automatic snapshots for use when restoring a WorkSpace are scheduled every 12 hours\. If the WorkSpace is healthy, snapshots of both the root volume and user volume are created around the same time\. If the WorkSpace is unhealthy, these snapshots are not created\.
+ **After a WorkSpace has been restored** — When you restore a WorkSpace, new snapshots are taken soon after the restore is finished \(often within 30 minutes\)\. In some AWS Regions, it might take several hours for these snapshots to be taken after a WorkSpace is restored\.

  After a WorkSpace has been restored, if the WorkSpace becomes unhealthy before new snapshots can be taken, the WorkSpace can't be restored again\. In that case, you can try [rebuilding the WorkSpace](rebuild-workspace.md) or contact AWS Support for assistance\.

You can restore a WorkSpace only if the following conditions are met:
+ The WorkSpace must have a state of `AVAILABLE`, `ERROR`, `UNHEALTHY`, or `STOPPED`\.
+ Snapshots of the root and user volumes must exist\.

**To restore a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to be restored and choose **Actions**, **Rebuild / Restore WorkSpace**\.

1. Select the **Restore WorkSpace** option\.

1. Choose **Rebuild / Restore WorkSpace**\.