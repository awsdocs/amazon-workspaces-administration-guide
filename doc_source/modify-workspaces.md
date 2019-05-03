# Modify a WorkSpace<a name="modify-workspaces"></a>

You can increase the size of the root and user volumes for a WorkSpace, up to 2000 GB each\. You can expand these volumes whether they are encrypted or unencrypted\. You can request a volume expansion once in a 6\-hour period\. To ensure that your data is preserved, you cannot decrease the size of the root or user volumes after you launch a WorkSpace\.

**Note**  
When you expand a volume for a WorkSpace, Amazon WorkSpaces automatically extends the volume's partition within Windows\. However, for this change to take effect, you must restart the WorkSpace\.

You can switch a WorkSpace between the Value, Standard, Performance, Power, and PowerPro bundles\. When you request a bundle change, Amazon WorkSpaces restarts the WorkSpace using the new bundle\. Amazon WorkSpaces preserves the operating system, applications, data, and storage settings for the WorkSpace\. You can request a larger bundle once in a 6\-hour period or a smaller bundle once in 30 days\. For a newly launched WorkSpace, you must wait 6 hours before requesting a larger bundle\.

**To modify the configuration of a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Modify WorkSpace**\.

1. To increase the size of the root volume or user volume, choose **Modify Volume Sizes** and type the new values\.
**Note**  
You can only resize SSD volumes\.

1. To change the bundle, choose **Change Compute Type** and select the new bundle type\.

1. Choose **Modify**\.