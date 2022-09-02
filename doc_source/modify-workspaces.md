# Modify a WorkSpace<a name="modify-workspaces"></a>

After you launch a WorkSpace, you can modify its configuration in two ways: 
+ You can change the size of its root volume \(for Windows, drive C; for Linux, /\) and its user volume \(for Windows, drive D; for Linux /home\)\.
+ You can change its compute type to select a new bundle\.

To see the current modification state of a WorkSpace, select the arrow to show more details about that WorkSpace\. The possible values for **State** are **Modifying Compute**, **Modifying Storage,** and **None**\.

If you want to modify a WorkSpace, it must have a status of `AVAILABLE` or `STOPPED`\. When you are modifying the volume size, you can't change the compute type at the same time, and vice versa\.

Changing the volume size or compute type of a WorkSpace will change the billing rate for the WorkSpace\.

To allow your users to modify their volumes and compute types themselves, see [Enable self\-service WorkSpace management capabilities for your users](enable-user-self-service-workspace-management.md)\.

## Change volume sizes<a name="change_volume_sizes"></a>

You can increase the size of the root and user volumes for a WorkSpace, up to 2000 GB each\. WorkSpace root and user volumes come in set groups that can't be changed\. The available groups are:


| \[Root \(GB\), User \(GB\)\] | 
| --- | 
| \[80, 10\] | 
| \[80, 50\] | 
| \[80, 100\] | 
| \[175 to 2000, 100 to 2000\] | 

You can expand the root and user volumes whether they are encrypted or unencrypted, and you can expand both volumes once in a 6\-hour period\. However, you can't increase the size of the root and user volumes at the same time\. For more information, see [Limitations for Increasing Volumes](#limitations_increasing_volumes)\.

**Note**  
When you expand a volume for a WorkSpace, WorkSpaces automatically extends the volume's partition within Windows or Linux\. When the process is finished, you must reboot the WorkSpace for the changes to take effect\. 

To ensure that your data is preserved, you cannot decrease the size of the root or user volumes after you launch a WorkSpace\. Instead, make sure that you specify the minimum sizes for these volumes when launching a WorkSpace\. You can launch a Value, Standard, Performance, Power, or PowerPro WorkSpace with a minimum of 80 GB for the root volume and 10 GB for the user volume\. You can launch a Graphics\.g4dn, GraphicsPro\.g4dn, Graphics, or GraphicsPro WorkSpace with a minimum of 100 GB for the root volume and 100 GB for the user volume\.

While a WorkSpace disk size increase is in progress, users can perform most tasks on their WorkSpace\. However, they can't change their WorkSpace compute type, switch the WorkSpace running mode, rebuild their WorkSpace, or reboot \(restart\) their WorkSpace\.

**Note**  
If you want your users to be able to use their WorkSpaces while the disk size increase is in progress, make sure the WorkSpaces have a status of `AVAILABLE` instead of `STOPPED` before you resize the volumes of the WorkSpaces\. If the WorkSpaces are `STOPPED`, they can't be started while the disk size increase is in progress\.

In most cases, the disk size increase process might take up to two hours\. However, if you're modifying the volume sizes for a large number of WorkSpaces, the process can take significantly longer\. If you have a large number of WorkSpaces to modify, we recommend contacting AWS Support for assistance\.

**Limitations for increasing volumes**
+ You can resize only SSD volumes\.
+ When you launch a WorkSpace, you must wait 6 hours before you can modify the sizes of its volumes\.
+ You cannot increase the size of the root and user volumes at the same time\. To increase the root volume, you must first change the user volume to 100 GB\. After that change is made, you can then update the root volume to any value between 175 and 2000 GB\. After the root volume has been changed to any value between 175 and 2000 GB, you can then update the user volume further, to any value between 100 and 2000 GB\.
**Note**  
If you want to increase both volumes, you must wait 20\-30 minutes for the first operation to finish before you can start the second operation\.
+ Unless the WorkSpace is a Graphics\.g4dn, GraphicsPro\.g4dn, Graphics, or GraphicsPro WorkSpace, the root volume cannot be less than 175 GB when the user volume is 100 GB\. Graphics\.g4dn, GraphicsPro\.g4dn, Graphics, and GraphicsPro WorkSpaces can have the root and user volumes both set to 100 GB minimum\.
+ If the user volume is 50 GB, you cannot update the root volume to anything other than 80 GB\. If the root volume is 80 GB, the user volume can only be 10, 50, or 100 GB\.

**To change the volume sizes of a WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Modify WorkSpace**\.

1. To increase the size of the root volume or user volume, choose **Modify Volume Sizes** and enter the new value\.

1. Choose **Modify**\.

1. When the disk size increase is finished, you must [ reboot the WorkSpace](reboot-workspaces.md) for the changes to take effect\. To avoid data loss, make sure the user saves any open files before you reboot the WorkSpace\.

**To change the volume sizes of a WorkSpace**  
Use the [modify\-workspace\-properties](https://docs.aws.amazon.com/cli/latest/reference/workspaces/modify-workspace-properties.html) command with the `RootVolumeSizeGib` or `UserVolumeSizeGib` property\.

## Change bundle types<a name="change_bundles"></a>

You can switch a WorkSpace between the Value, Standard, Performance, Power, and PowerPro bundles\. For more information about these bundle types, see [Amazon WorkSpaces Bundles](http://aws.amazon.com/workspaces/features/#Amazon_WorkSpaces_Bundles)\.

**Note**  
You can change the compute type from Graphics\.g4dn to GraphicsPro\.g4dn and vice versa\. You cannot change the compute type of Graphics\.g4dn and GraphicsPro\.g4dn to any other value\.
You cannot change the compute type of Graphics and GraphicsPro to any other value\.

When you request a bundle change, WorkSpaces reboots the WorkSpace using the new bundle\. WorkSpaces preserves the operating system, applications, data, and storage settings for the WorkSpace\.

You can request a larger bundle once in a 6\-hour period or a smaller bundle once every 30 days\. For a newly launched WorkSpace, you must wait 6 hours before requesting a larger bundle\.

When a WorkSpace compute type change is in progress, users are disconnected from their WorkSpace, and they can't use or change the WorkSpace\. The WorkSpace is automatically rebooted during the compute type change process\.

**Important**  
To avoid data loss, make sure users save any open documents and other application files before you change the WorkSpace compute type\.

The compute type change process might take up to an hour\.

**To change the bundle type of a WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Modify WorkSpace**\.

1. To change the bundle, choose **Change Compute Type** and select the new bundle type\.

1. Choose **Modify**\.

**To change the bundle type of a WorkSpace**  
Use the [modify\-workspace\-properties](https://docs.aws.amazon.com/cli/latest/reference/workspaces/modify-workspace-properties.html) command with the `ComputeTypeName` property\.