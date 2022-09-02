# Manage the WorkSpace running mode<a name="running-mode"></a>

The *running mode* of a WorkSpace determines its immediate availability and how you pay for it \(monthly or hourly\)\. You can choose between the following running modes when you create the WorkSpace:
+ **AlwaysOn** — Use when paying a fixed monthly fee for unlimited usage of your WorkSpaces\. This mode is best for users who use their WorkSpace full time as their primary desktop\.
+ **AutoStop** — Use when paying for your WorkSpaces by the hour\. With this mode, your WorkSpaces stop after a specified period of disconnection, and the state of apps and data is saved\.

For more information, see [WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

## AutoStop WorkSpaces<a name="autostop-workspaces"></a>

 To set the automatic stop time, select the WorkSpace in the Amazon WorkSpaces console, choose **Actions**, **Modify Running Mode Properties**, and then set **AutoStop Time \(hours\)**\. By default, **AutoStop Time \(hours\)** is set to 1 hour, which means that the WorkSpace is automatically stopped 1 hour after the WorkSpace has been disconnected\.

After a WorkSpace is disconnected and the AutoStop Time period has expired, it might take several additional minutes for the WorkSpace to be automatically stopped\. However, billing stops as soon as the AutoStop Time period expires, and you aren't charged for that additional time\.

When possible, the state of the desktop is saved to the root volume of the WorkSpace\. The WorkSpace resumes when a user logs in, and all open documents and running programs return to their saved state\.

AutoStop Graphics\.g4dn, GraphicsPro\.g4dn, Graphics, and GraphicsPro WorkSpaces do not preserve the state of data and programs when they stop\. For these Autostop WorkSpaces, we recommend saving your work when you’re done using them each time\.

For Bring Your Own License \(BYOL\) AutoStop WorkSpaces, a large number of concurrent logins could result in significantly increased time for WorkSpaces to be available\. If you expect many users to log into your BYOL AutoStop WorkSpaces at the same time, please consult your account manager for advice\.

**Important**  
AutoStop WorkSpaces are automatically stopped only if the WorkSpaces are disconnected\.

A WorkSpace is disconnected only in the following circumstances:
+ If the user manually disconnects from the WorkSpace or quits the Amazon WorkSpaces client application\.
+ If the client device is shut down\.
+ If there's no connection between the client device and the WorkSpace for more than 20 minutes\.

As a best practice, AutoStop WorkSpace users should manually disconnect from their WorkSpaces when they're done using them each day\. To manually disconnect, choose **Disconnect WorkSpace** or **Quit Amazon WorkSpaces** from the **Amazon WorkSpaces** menu in the WorkSpaces client applications for Linux, macOS, or Windows\. For Android or iPad, choose **Disconnect** from the sidebar menu\.

**AutoStop WorkSpaces might not be automatically stopped in the following situations**
+ If the client device is only locked, sleeping, or otherwise inactive \(for example, the laptop lid is closed\) instead of shut down, the WorkSpaces application might still be running in the background\. As long as the WorkSpaces application is still running, the WorkSpace might not be disconnected, and therefore the WorkSpace might not automatically stop\.
+ WorkSpaces can detect disconnection only when users are using WorkSpaces clients\. If users are using third\-party clients, WorkSpaces might not be able to detect disconnection, and therefore the WorkSpaces might not automatically stop and billing might not be suspended\.

## Modify the running mode<a name="modify-running-mode"></a>

You can switch between running modes at any time\.

**To modify the running mode of a WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to modify and choose **Actions**, **Modify Running Mode Properties**\.

1. Select the new running mode, **AlwaysOn** or **AutoStop**, and then choose **Modify**\.

**To modify the running mode of a WorkSpace using the AWS CLI**  
Use the [modify\-workspace\-properties](https://docs.aws.amazon.com/cli/latest/reference/workspaces/modify-workspace-properties.html) command\.

## Stop and start an AutoStop WorkSpace<a name="stop-start-workspace"></a>

When your AutoStop WorkSpaces are disconnected, they are automatically stopped after a specified period of disconnection, and hourly billing is suspended\. To further optimize costs, you can manually suspend the hourly charges associated with AutoStop WorkSpaces\. The WorkSpace is stopped and all apps and data are saved for the next time a user logs in to the WorkSpace\.

When a user reconnects to a stopped WorkSpace, it resumes from where it left off, typically in under 90 seconds\.

You can reboot \(restart\) AutoStop WorkSpaces that are available or in an error state\.

**To stop an AutoStop WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace to be stopped and choose **Actions**, **Stop WorkSpaces**\.

1. When prompted for confirmation, choose **Stop**\.

**To start an AutoStop WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpaces to be started and choose **Actions**, **Start WorkSpaces**\.

1. When prompted for confirmation, choose **Start**\.

To remove the fixed infrastructure costs that are associated with AutoStop WorkSpaces, remove the WorkSpace from your account\. For more information, see [Delete a WorkSpace](delete-workspaces.md)\.

**To stop and start an AutoStop WorkSpace using the AWS CLI**  
Use the [stop\-WorkSpaces](https://docs.aws.amazon.com/cli/latest/reference/workspaces/stop-workspaces.html) and [start\-WorkSpaces](https://docs.aws.amazon.com/cli/latest/reference/workspaces/start-workspaces.html) commands\.