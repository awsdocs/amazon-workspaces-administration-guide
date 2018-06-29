# Manage the WorkSpace Running Mode<a name="running-mode"></a>

The *running mode* of a WorkSpaces determines its immediate availability and how you pay for it\. You can choose between the following running modes when you create the WorkSpace:
+ **AlwaysOn** — Use when paying a fixed monthly fee for unlimited usage of your WorkSpaces\. This mode is best for users who use their WorkSpace full time as their primary desktop\.
+ **AutoStop** — Use when paying for your WorkSpaces by the hour\. With this mode, your WorkSpaces stop after a specified period of inactivity and the state of apps and data is saved\. To set the automatic stop time, use **AutoStop Time \(hours\)**\.

  When possible, the state of the desktop is saved to the root volume of the WorkSpace\. The WorkSpace resumes when a user logs in, and all open documents and running programs return to their saved state\.

For more information, see [Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

## Modify the Running Mode<a name="modify-running-mode"></a>

You can switch between running modes at any time\.

**To modify the running mode of a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpaces to modify and choose **Actions**, **Modify Running Mode Properties**\.

1. Select the new running mode, **AlwaysOn** or **AutoStop**, and then choose **Modify**\.

## Stop and Start an AutoStop WorkSpace<a name="stop-start-workspace"></a>

When your AutoStop WorkSpaces are not in use, they are automatically stopped after a specified period of inactivity, and hourly metering is suspended\. To further optimize costs, you can suspend the hourly charges associated with AutoStop WorkSpaces\. The WorkSpace is stopped and all apps and data saved for the next time a user logs in to the WorkSpace\.

When a user reconnects to a stopped WorkSpace, it resumes from where it left off, typically in under 90 seconds\.

You can restart AutoStop WorkSpaces that are available or in an error state\.

**To stop an AutoStop WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpaces to be stopped and choose **Actions**, **Stop WorkSpaces**\.

1. When prompted for confirmation, choose **Stop**\.

**To start an AutoStop WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpaces to be started and choose **Actions**, **Start WorkSpaces**\.

1. When prompted for confirmation, choose **Start**\.

To remove the fixed infrastructure costs associated with AutoStop WorkSpaces, remove the WorkSpace from your account\. For more information, see [Delete a WorkSpace](delete-workspaces.md)\.

## Set Maintenance Mode<a name="set-maintenance-mode"></a>

If you enable maintenance mode for your AutoStop WorkSpaces, they are started automatically one time a month in order to download and install important service, security, and Windows updates\. Unless the updates are delayed, the maintenance window is typically between 00:00 and 05:00 every day, starting on the third Monday of the month\. The maintenance window might take up to two weeks\. A WorkSpace can be maintained on any day in the maintenance window\. 

After you enable maintenance mode, ensure that your WorkSpaces are in the stopped state between 00:00 and 02:00 of the maintenance window\. Otherwise, they are not maintained automatically\.

If you manage updates to your WorkSpaces on a regular basis, you can disable maintenance mode\.

**To set maintenance mode**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory, and choose **Actions**, **Update Details**\.

1. Expand **Maintenance Mode**\.

1. To enable automatic updates, choose **Enabled**\. If you manage updates manually, choose **Disabled**\.

1. Choose **Update and Exit**\.