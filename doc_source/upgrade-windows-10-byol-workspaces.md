# Upgrade Windows 10 BYOL WorkSpaces<a name="upgrade-windows-10-byol-workspaces"></a>

On your Windows 10 Bring Your Own License \(BYOL\) WorkSpaces, you can upgrade to a newer version of Windows 10 by using the in\-place upgrade process\. Follow the instructions in this topic to do so\.

The in\-place upgrade process applies only to Windows 10 BYOL WorkSpaces\.

**Important**  
Do not run Sysprep on an upgraded WorkSpace\. If you do so, an error that prevents Sysprep from finishing might occur\. If you plan to run Sysprep, do so only on a WorkSpace that hasn't been upgraded\.

**Topics**
+ [Prerequisites](#upgrade_byol_prerequisites)
+ [Important Considerations](#upgrade_byol_important_considerations)
+ [Known Limitations](#byol-known-limitations)
+ [Summary of Registry Key Settings](#upgrade_byol_registry_summary)
+ [Steps to Perform an In\-place Upgrade](#upgrade_byol_procedure)
+ [Troubleshooting](#byol-troubleshooting)
+ [Update Your WorkSpace Registry Using a PowerShell Script](#update-windows-10-byol-script)

## Prerequisites<a name="upgrade_byol_prerequisites"></a>
+ If you have deferred or paused Windows 10 upgrades by using Group Policy or System Center Configuration Manager \(SCCM\), enable operating system upgrades for your Windows 10 WorkSpaces\.
+ If the WorkSpace is an AutoStop WorkSpace, change it to an AlwaysOn WorkSpace before the in\-place upgrade process so that it won't be stopped automatically while updates are being applied\. For more information, see [Modify the Running Mode](running-mode.md#modify-running-mode)\. If you prefer to keep the WorkSpace set to AutoStop, change the AutoStop time to three hours or more while the upgrade takes place\.
+ The in\-place upgrade process recreates the user profile by making a copy of a special profile named Default User \(`C:\Users\Default`\)\. Do not use this default user profile to make customizations\. We recommend making any customizations to the user profile through Group Policy Objects \(GPOs\) instead\. Customizations made through GPOs can be easily modified or rolled back and are less prone to error\.

## Important Considerations<a name="upgrade_byol_important_considerations"></a>

The in\-place upgrade process uses two registry scripts \(`enable-inplace-upgrade.ps1` and `update-pvdrivers.ps1`\) to make the necessary changes to your WorkSpaces that enable the Windows Update process to run\. These changes involve creating a \(temporary\) user profile on drive C instead of drive D\. If a user profile already exists on drive D, the data in that original user profile remains on drive D\.

By default, WorkSpaces creates the user profile in `D:\Users\%USERNAME%`\. The `enable-inplace-upgrade.ps1` script configures Windows to create a new user profile in `C:\Users\%USERNAME%` and redirects the user shell folders to `D:\Users\%USERNAME%`\. This new user profile is created when a user logs on the first time\.

After the in\-place upgrade, you have the choice of leaving your user profiles on drive C to allow your users to use the Windows Update process to upgrade their machines in the future\. However, be aware that WorkSpaces with profiles stored on drive C can't be rebuilt or migrated without losing all of the data in the user's profile unless you back up and restore that data yourself\. If you decide to leave the profiles on drive C, you can use the **UserShellFoldersRedirection** registry key to redirect the user shell folders to drive D, as explained later in this topic\.

To ensure that you can rebuild or migrate your WorkSpaces and to avoid any potential problems with user shell folder redirection, we recommend that you choose to restore your user profiles to drive D after the in\-place upgrade\. You can do so by using the **PostUpgradeRestoreProfileOnD** registry key, as explained later in this topic\. 

## Known Limitations<a name="byol-known-limitations"></a>
+ The user profile location change from drive D to drive C does not happen during WorkSpace rebuilds or migrations\. If you perform an in\-place upgrade on a Windows 10 BYOL WorkSpace and then rebuild or migrate it, the new WorkSpace will have the user profile on drive D\.
**Warning**  
If you leave the user profile on drive C after the in\-place upgrade, the user profile data stored on drive C will be lost during rebuilds or migrations unless you manually back up the user profile data prior to rebuilding or migrating, and then manually restore the user profile data after running the rebuild or migration process\.
+ If your default BYOL bundle contains an image that is based on an earlier release of Windows 10, you must perform the in\-place upgrade again after the WorkSpace is rebuilt or migrated\.

## Summary of Registry Key Settings<a name="upgrade_byol_registry_summary"></a>

To enable the in\-place upgrade process and to specify where you would like the user profile to be located after the upgrade, you must set a number of registry keys\.


**Registry path: **HKLM:\\Software\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1****  

| Registry key | Type | Values | 
| --- | --- | --- | 
| Enabled | DWORD |  **0** – \(Default\) Disables in\-place upgrade **1** – Enables in\-place upgrade  | 
| PostUpgradeRestoreProfileOnD | DWORD |  **0** – \(Default\) Does not attempt to restore the user profile path after the in\-place upgrade **1** – Restores the user profile path \(**ProfileImagePath**\) after the in\-place upgrade  | 
| UserShellFoldersRedirection | DWORD |  **0** – Does not enable redirection of user shell folders **1** – \(Default\) Enables redirection of user shell folders to `D:\Users\%USERNAME%` after the user profile is regenerated on `C:\Users\%USERNAME%`  | 
| NoReboot | DWORD |  **0** – \(Default\) Does not allow the script to reboot the WorkSpace after modifying the registry for the user profile **1** – Allows you to control when a reboot occurs after modifying the registry for the user profile  | 


**Registry path: **HKLM:\\Software\\Amazon\\WorkSpacesConfig\\update\-pvdrivers\.ps1****  

| Registry key | Type | Values | 
| --- | --- | --- | 
| Enabled | DWORD |  **0** – \(Default\) Disables AWS PV drivers update **1** – Enables AWS PV drivers update  | 

## Steps to Perform an In\-place Upgrade<a name="upgrade_byol_procedure"></a>

To enable in\-place Windows upgrades on your BYOL WorkSpaces, you must set certain registry keys, as described in the following procedure\. You must also set certain registry keys to indicate the drive \(C or D\) where you want the user profiles to be located after the in\-place upgrades are finished\.

You can make these registry changes manually\. If you have multiple WorkSpaces to update, you can use Group Policy or SCCM to push a PowerShell script\. For a sample PowerShell script, see [Update Your WorkSpace Registry Using a PowerShell Script](#update-windows-10-byol-script)\.

**To perform an in\-place upgrade of Windows 10**

1. Make note of which version of Windows is currently running on the Windows 10 BYOL WorkSpaces that you are updating, and then reboot them\.

1. Update the following Windows system registry keys to change the value data for **Enabled** from **0** to **1**\. These registry changes enable in\-place upgrades for the WorkSpace\.
   + **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1**
   + **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\update\-pvdrivers\.ps1**
**Note**  
If these keys do not exist, reboot the WorkSpace\. The keys should be added when the system is rebooted\.

   \(Optional\) If you are using a managed workflow such as SCCM Task Sequences to perform the upgrade, set the following key value to **1** to prevent the computer from rebooting:

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\NoReboot**

1. Decide which drive you want user profiles to be located on after the in\-place upgrade process \(for more information, see [Important Considerations](#upgrade_byol_important_considerations)\), and set the registry keys as follows:
   + Settings if you want the user profile on drive C after the upgrade:

     **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1**

     Key name: **PostUpgradeRestoreProfileOnD**

     Key value: **0**

     Key name: **UserShellFoldersRedirection**

     Key value: **1**
   + Settings if you want the user profile on drive D after the upgrade:

     **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1**

     Key name: **PostUpgradeRestoreProfileOnD**

     Key value: **1**

     Key name: **UserShellFoldersRedirection**

     Key value: **0**

1. After saving the changes to the registry, reboot the WorkSpace again so that the changes are applied\.
**Note**  
After the reboot, logging in to the WorkSpace creates a new user profile\. You might see placeholder icons in the **Start** menu\. This behavior is automatically resolved after the in\-place upgrade is complete\.

   \(Optional\) Confirm that the following key value is set to **1**, which unblocks the WorkSpace for updating:

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\profileImagePathDeleted**

1. Perform the in\-place upgrade\. You can use whichever method you like, such as SCCM, ISO, or Windows Update \(WU\)\. Depending on your original Windows 10 version and how many apps were installed, this process can take from 40 to 120 minutes\.

1. After the update process is finished, confirm that the Windows version has been updated\.
**Note**  
If the in\-place upgrade fails, Windows automatically rolls back to use the Windows 10 version that was in place before you started the upgrade\. For more information about troubleshooting, see the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)\.

   \(Optional\) To confirm that the update scripts were successfully executed, verify that the following key value is set to **1**:

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\scriptExecutionComplete**

1. If you modified the running mode of the WorkSpace by setting it to AlwaysOn or by changing the AutoStop time period so that the in\-place upgrade process could run without interruption, set the running mode back to your original settings\. For more information, see [Modify the Running Mode](running-mode.md#modify-running-mode)\.

If you haven't set the **PostUpgradeRestoreProfileOnD** registry key to **1**, the user profile is regenerated by Windows and placed in `C:\Users\%USERNAME%` after the in\-place upgrade, so that you do not have to go through the above steps again for future Windows 10 in\-place upgrades\. By default, the `enable-inplace-upgrade.ps1` script redirects the following shell folders to drive D:
+ `D:\Users\%USERNAME%\Downloads`
+ `D:\Users\%USERNAME%\Desktop`
+ `D:\Users\%USERNAME%\Favorites`
+ `D:\Users\%USERNAME%\Music`
+ `D:\Users\%USERNAME%\Pictures`
+ `D:\Users\%USERNAME%\Videos`
+ `D:\Users\%USERNAME%\Documents`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Network Shortcuts`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Printer Shortcuts`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Recent`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\SendTo`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Start Menu`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`
+ `D:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Templates`

If you redirect the shell folders to other locations on your WorkSpaces, perform the necessary operations on the WorkSpaces after the in\-place upgrades\.

## Troubleshooting<a name="byol-troubleshooting"></a>

If you encounter any issues with the update, you can check the following items to assist with troubleshooting:
+ Windows Logs, which are located, by default, in the following locations:

  `C:\Program Files\Amazon\WorkSpacesConfig\Logs\`

  `C:\Program Files\Amazon\WorkSpacesConfig\Logs\TRANSMITTED`
+ Windows Event Viewer

  Windows Logs > Application > Source: Amazon WorkSpaces

**Tip**  
During the in\-place upgrade process, if you see that some icon shortcuts on the desktop no longer work, it's because WorkSpaces moves any user profiles located on drive D to drive C to prepare for the upgrade\. After the upgrade is completed, the shortcuts will work as expected\.

## Update Your WorkSpace Registry Using a PowerShell Script<a name="update-windows-10-byol-script"></a>

You can use the following sample PowerShell script to update the registry on your WorkSpaces to enable in\-place upgrades\. Follow the [Steps to Perform an In\-place Upgrade](#upgrade_byol_procedure), but use this script to update the registry on each WorkSpace\.

```
# AWS WorkSpaces 1.28.20
# Enable In-Place Update Sample Scripts
# These registry keys and values will enable scripts to execute on the next reboot of the WorkSpace.
 
$scriptlist = ("update-pvdrivers.ps1","enable-inplace-upgrade.ps1")
$wsConfigRegistryRoot="HKLM:\Software\Amazon\WorkSpacesConfig"
$Enabled = 1
$script:ErrorActionPreference = "Stop"
 
foreach ($scriptName in $scriptlist)
{
    $scriptRegKey = "$wsConfigRegistryRoot\$scriptName"
    
    try
    {
        if (-not(Test-Path $scriptRegKey))
        {        
            Write-Host "Registry key not found. Creating registry key '$scriptRegKey' with 'Update' enabled."
            New-Item -Path $wsConfigRegistryRoot -Name $scriptName | Out-Null
            New-ItemProperty -Path $scriptRegKey -Name Enabled -PropertyType DWord -Value $Enabled | Out-Null
            Write-Host "Value created. '$scriptRegKey' Enabled='$((Get-ItemProperty -Path $scriptRegKey).Enabled)'"
        }
        else
        {
            Write-Host "Registry key is already present with value '$scriptRegKey' Enabled='$((Get-ItemProperty -Path $scriptRegKey).Enabled)'"
            if((Get-ItemProperty -Path $scriptRegKey).Enabled -ne $Enabled)
            {
                Set-ItemProperty -Path $scriptRegKey -Name Enabled -Value $Enabled
                Write-Host "Value updated. '$scriptRegKey' Enabled='$((Get-ItemProperty -Path $scriptRegKey).Enabled)'"
            }
        }
    }
    catch
    {
        write-host "Stopping script, the following error was encountered:" `r`n$_ -ForegroundColor Red
        break
    }
}
```