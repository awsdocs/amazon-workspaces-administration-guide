# Rebuild a WorkSpace<a name="rebuild-workspace"></a>

If needed, you can rebuild the operating system of a WorkSpace to its original state\. Rebuilding a WorkSpace causes the following to occur:
+ The system is restored to the most recent image of the bundle that the WorkSpace is created from\. Any applications that have been installed, or system settings that have been made after the WorkSpace was created are lost\.
+ The data drive \(for Microsoft Windows, the D: drive; for Linux, /home\) is recreated from the last automatic snapshot taken of the data drive\. The current contents of the data drive is overwritten\. Automatic snapshots of the data drive are taken every 12 hours, so the snapshot can be as much as 12 hours old\.
+ The primary elastic network interface \(ENI\) is recreated\. The WorkSpace receives a new private IP address\.

**To rebuild a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to be rebuilt and choose **Actions**, **Rebuild WorkSpace**\.

1. When prompted for confirmation, choose **Rebuild WorkSpace**\.