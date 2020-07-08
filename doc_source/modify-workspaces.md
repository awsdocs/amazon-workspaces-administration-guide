# Modify a WorkSpace<a name="modify-workspaces"></a>

After you launch a WorkSpace, you can modify its configuration in two ways: 
+ You can change the size of its root volume \(for Windows, drive C; for Linux, /\) and its user volume \(for Windows, drive D; for Linux /home\)\.
+ You can change its compute type to select a new bundle\.

The current modification state of a WorkSpace is displayed in the **State** setting in the Amazon WorkSpaces console\. The possible values for **State** are **Modifying Compute**, **Modifying Storage,** and **None**\.

If you want to modify a WorkSpace, it must have a status of `AVAILABLE` or `STOPPED`\. When you are modifying the volume size, you can't change the compute type at the same time, and vice versa\.

Changing the volume size or compute type of a WorkSpace will change the billing rate for the WorkSpace\.

To allow your users to modify their volumes and compute types themselves, see [Enable Self\-Service WorkSpace Management Capabilities for Your Users](enable-user-self-service-workspace-management.md)\.

## Changing Volume Sizes<a name="change_volume_sizes"></a>

You can increase the size of the root and user volumes for a WorkSpace, up to 2000 GB each\. WorkSpace root and user volumes come in set groups that can't be changed\. The available groups are:


| \[Root \(GB\), User \(GB\)\] | 
| --- | 
| \[80, 10\] | 
| \[80, 50\] | 
| \[80, 100\] | 
| \[175 to 2000, 100 to 2000\] | 

You can expand the root and user volumes whether they are encrypted or unencrypted, and you can expand both volumes once in a 6\-hour period\. However, you can't increase the size of the root and user volumes at the same time\. For more information, see [Limitations for Increasing Volumes](#limitations_increasing_volumes)\.

**Note**  
When you expand a volume for a WorkSpace, Amazon WorkSpaces automatically extends the volume's partition within Windows or Linux\. When the process is finished, you must reboot the WorkSpace for the changes to take effect\. 

To ensure that your data is preserved, you cannot decrease the size of the root or user volumes after you launch a WorkSpace\. Instead, make sure that you specify the minimum sizes for these volumes when launching a WorkSpace\. You can launch a Value, Standard, Performance, Power, or PowerPro WorkSpace with a minimum of 80 GB for the root volume and 10 GB for the user volume\. You can launch a Graphics or GraphicsPro WorkSpace with a minimum of 100 GB for the root volume and 100 GB for the user volume\.

While a WorkSpace disk size increase is in progress, users can perform most tasks on their WorkSpace\. However, they can't change their WorkSpace compute type, switch the WorkSpace running mode, rebuild their WorkSpace, or restart their WorkSpace\.

The disk size increase process might take up to an hour\.

**Limitations for Increasing Volumes**
+ You can resize only SSD volumes\.
+ When you launch a WorkSpace, you must wait 6 hours before you can modify the sizes of its volumes\.
+ You cannot increase the size of the root and user volumes at the same time\. To increase the root volume, you must first change the user volume to 100 GB\. After that change is made, you can then update the root volume to any value between 175 and 2000 GB\. After the root volume has been changed to any value between 175 and 2000 GB, you can then update the user volume further, to any value between 100 and 2000 GB\.
**Note**  
If you want to increase both volumes, you must wait 20\-30 minutes for the first operation to finish before you can start the second operation\.
+ Unless the WorkSpace is a Graphics or GraphicsPro WorkSpace, the root volume cannot be less than 175 GB when the user volume is 100 GB\. Graphics and GraphicsPro WorkSpaces can have the root and user volumes both set to 100 GB minimum\.
+ If the user volume is 50 GB, you cannot update the root volume to anything other than 80 GB\. If the root volume is 80 GB, the user volume can only be 10, 50, or 100 GB\.

**To change the volume sizes of a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Modify WorkSpace**\.

1. To increase the size of the root volume or user volume, choose **Modify Volume Sizes** and enter the new value\.

1. Choose **Modify**\.

1. When the disk size increase is finished, you must [ restart the WorkSpace](reboot-workspaces.md) for the changes to take effect\. To avoid data loss, make sure the user saves any open files before you restart the WorkSpace\.

## Changing Bundle Types<a name="change_bundles"></a>

You can switch a WorkSpace between the Value, Standard, Performance, Power, and PowerPro bundles\. When you request a bundle change, Amazon WorkSpaces restarts the WorkSpace using the new bundle\. Amazon WorkSpaces preserves the operating system, applications, data, and storage settings for the WorkSpace\.

You can request a larger bundle once in a 1\-hour period or a smaller bundle once every 30 days\. For a newly launched WorkSpace, you must wait 1 hour before requesting a larger bundle\.

When a WorkSpace compute type change is in progress, users are disconnected from their WorkSpace, and they can't use or change the WorkSpace\. The WorkSpace is automatically rebooted during the compute type change process\.

**Important**  
To avoid data loss, make sure users save any open documents and other application files before you change the WorkSpace compute type\.

The compute type change process might take up to an hour\.

**To change the bundle type of a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Modify WorkSpace**\.

1. To change the bundle, choose **Change Compute Type** and select the new bundle type\.

1. Choose **Modify**\.