# Upgrade Windows 10 BYOL WorkSpaces<a name="upgrade-windows-10-byol-workspaces"></a>

On your Windows 10 Bring Your Own License \(BYOL\) WorkSpaces, you can upgrade to a newer version of Windows 10 by using the in\-place upgrade process\. Follow the instructions in this topic to do so\.

The in\-place upgrade process applies only to Windows 10 BYOL WorkSpaces\.

**Important**  
Do not run Sysprep on an upgraded WorkSpace\. If you do so, an error that prevents Sysprep from finishing might occur\. If you plan to run Sysprep, do so only on a WorkSpace that hasn't been upgraded\.

**Prerequisites**
+ If you have deferred or paused Windows 10 upgrades by using Group Policy or System Center Configuration Manager \(SCCM\), enable operating system upgrades for your Windows 10 WorkSpaces\.
+ If the WorkSpace is an AutoStop WorkSpace, change it to an AlwaysOn WorkSpace before the in\-place upgrade process so that it won't be stopped automatically while updates are being applied\. For more information, see [Modify the Running Mode](running-mode.md#modify-running-mode)\. To keep the WorkSpace set to AutoStop, change the AutoStop time to three hours or more while the upgrade takes place\.
+ The in\-place upgrade process recreates the user profile by making a copy of a special profile named Default User \(`C:\Users\Default`\)\. To preserve custom Windows preferences and application settings stored in the Windows registry, modify `C:\Users\Default\NTUSER.DAT` on the WorkSpace before you perform the in\-place upgrade\.

**To perform an in\-place upgrade of Windows 10**

1. Check the version of Windows currently running on the Windows 10 BYOL WorkSpaces that you are updating, and then reboot them\.

1. Update the following Windows system registry keys to change the value data for **Enabled** from 0 to 1\. These registry changes enable in\-place upgrades for the WorkSpace\.

    You can do this manually\. If you have multiple WorkSpaces to update, you can use Group Policy or SCCM to push a [PowerShell script](#update-windows-10-byol-script)\.
   + **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1**
   + **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\update\-pvdrivers\.ps1**
**Note**  
If these keys do not exist, reboot the WorkSpace\. The keys should be added when the system is rebooted\.

   \(Optional\) If you are using a managed workflow, such as SCCM Task Sequences, to perform the upgrade, set the following key value to 1 to prevent the computer from rebooting\.

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\NoReboot**

   \(Optional\) By default, WorkSpaces redirects user shell folders to drive D\. To prevent user shell folder redirection, set the following key value to 0\.

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\UserShellFoldersRedirection**

1. After saving the changes to the registry, reboot the WorkSpace again so that the changes are applied\.
**Note**  
After the reboot, logging in to the WorkSpace creates a new user profile\. You may see placeholder icons in the **Start** menu\. This behavior is automatically resolved after the in\-place upgrade is complete\.

   \(Optional\) Confirm that the following key value is set to 1, which unblocks the WorkSpace for updating\.

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\profileImagePathDeleted**

1. Perform the in\-place upgrade\. You can use whichever method you like, such as SCCM, ISO, or Windows Update \(WU\)\. Depending on your original Windows 10 version and how many apps were installed, this process can take 40\-120 minutes\.

1. After the update process is finished, confirm that the Windows version has been updated\.
**Note**  
If the in\-place upgrade fails, Windows automatically rolls back to use the Windows 10 version that was in place before you started the upgrade\. For more information about troubleshooting, see the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)\.

   \(Optional\) To confirm that the update scripts were successfully executed, verify that the following key value is set to 1\.

   **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\WorkSpacesConfig\\enable\-inplace\-upgrade\.ps1\\scriptExecutionComplete**

   \(Optional\) If you modified the running mode of the WorkSpace by setting it to AlwaysOn or by changing the AutoStop time period so that the in\-place upgrade process could run without interruption, set the running mode back to your original settings\. For more information, see [Modify the Running Mode](running-mode.md#modify-running-mode)\.

After in\-place upgrades, the `NTUSER.DAT` file of the user is regenerated and placed on the C drive, so that you do not have to go through the above steps again for future Windows 10 in\-place upgrades\. By default, WorkSpaces redirects the following shell folders to the D drive\.
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

## Known Limitations<a name="byol-known-limitations"></a>

The `NTUSER.DAT` location change does not happen during WorkSpace rebuilds\. If you perform an in\-place upgrade on a Windows 10 BYOL WorkSpace and then rebuild it, the new WorkSpace uses the `NTUSER.DAT` file on the D drive\. Also, if your default BYOL bundle contains an image that is based on an earlier release of Windows 10, you need to perform the in\-place upgrade again after the WorkSpace is rebuilt\.

## Troubleshooting<a name="byol-troubleshooting"></a>

If you encounter any issues with the update, you can check the following items to troubleshoot\.
+ Windows Logs, which are located, by default, in the following locations:

  `C:\Program Files\Amazon\WorkSpacesConfig\Logs\`

  `C:\Program Files\Amazon\WorkSpacesConfig\Logs\TRANSMITTED`
+ Windows Event Viewer

  Windows Logs > Application > Source: Amazon WorkSpaces
+ During the in\-place upgrade process, if you see that some icon shortcuts on the desktop no longer work, it's because WorkSpaces moves any user profiles located on drive D to drive C to prepare for the upgrade\. After the upgrade is completed, the shortcuts work as expected\.

## Update Your WorkSpace Registry Using a PowerShell Script<a name="update-windows-10-byol-script"></a>

You can use the following sample PowerShell script to update the registry on your WorkSpaces to enable in\-place upgrades\. Follow the steps in the previous section, but use this script to update the registry on each WorkSpace\.

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