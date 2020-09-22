# Create a Custom WorkSpaces Image and Bundle<a name="create-custom-bundle"></a>

If you've launched a Windows or Amazon Linux WorkSpace and have customized it, you can create a custom image and custom bundles from that WorkSpace\.

A *custom image* contains only the OS, software, and settings for the WorkSpace\. A *custom bundle* is a combination of both that custom image and the hardware from which a WorkSpace can be launched\.

After you create a custom image, you can build a custom bundle that combines the custom image and the underlying compute and storage configuration that you select\. You can then specify this custom bundle when you launch new WorkSpaces to ensure that the new WorkSpaces have the same consistent configuration \(hardware and software\)\.

You can use the same custom image to create various custom bundles by selecting different compute and storage options for each bundle\. <a name="important_note"></a>

**Important**  
If you plan to create an image from a Windows 10 WorkSpace, note that image creation is not supported on Windows 10 systems that have been upgraded from one version of Windows 10 to a newer version of Windows 10 \(a Windows feature/version upgrade\)\. However, Windows cumulative or security updates are supported by the WorkSpaces image\-creation process\.
After January 14, 2020, images cannot be created from public Windows 7 bundles\. You might want to consider migrating your Windows 7 WorkSpaces to Windows 10\. For more information, see [Migrate a WorkSpace](migrate-workspaces.md)\.

Custom bundles cost as same as the public bundles they are created from\. For more information about pricing, see [Amazon WorkSpaces Pricing](http://aws.amazon.com/workspaces/pricing/)\.

**Topics**
+ [Requirements to Create Windows Custom Images](#windows_custom_image_requirements)
+ [Requirements to Create Amazon Linux Custom Images](#linux_custom_image_requirements)
+ [Best Practices](#custom_image_best_practices)
+ [Step 1: Run the Image Checker](#run_image_checker)
+ [Step 2: Create a Custom Image and Custom Bundle](#create_custom_image_bundle)
+ [What's Included with Windows WorkSpaces Custom Images](#image_creation_windows)
+ [What's Included with Amazon Linux WorkSpace Custom Images](#image_creation_linux)

## Requirements to Create Windows Custom Images<a name="windows_custom_image_requirements"></a>
+ The status of the WorkSpace must be **Available** and its modification state must be **None**\.
+ All applications and user profiles on WorkSpaces images must be compatible with Microsoft Sysprep\.
+ All applications to be included in the image must be installed on the `C` drive\.
+ The user profile must exist, must be located at `D:\Users\username`, and its total size \(files and data\) must be less than 10 GB\.
+ The `C` drive must have at least 12 GB of available space\.
+ All application services running on the WorkSpace must use a local system account instead of domain user credentials\. For example, you cannot have a Microsoft SQL Server Express installation running with a domain user's credentials\.
+ The WorkSpace must not be encrypted\. Image creation from an encrypted WorkSpace is not currently supported\.
+ The following components are required in an image\. Without these components, the WorkSpaces that you launch from the image will not function correctly:
  + Windows PowerShell version 3\.0 or later
  + Remote Desktop Services
  + AWS PV drivers
  + Windows Remote Management \(WinRM\)
  + Teradici PCoIP agents and drivers
  + STXHD agents and drivers
  + AWS and WorkSpaces certificates
  + Skylight agent

## Requirements to Create Amazon Linux Custom Images<a name="linux_custom_image_requirements"></a>
+ The status of the WorkSpace must be **Available** and its modification state must be **None**\.
+ All applications to be included in the image must be installed outside of the user volume \(the `/home` directory\)\.
+ The root volume \(/\) should be less than 97% full\.
+ The WorkSpace must not be encrypted\. Image creation from an encrypted WorkSpace is not currently supported\.
+ The following components are required in an image\. Without these components, the WorkSpaces that you launch from the image will not function correctly:
  + Cloud\-init
  + Teradici PCoIP agents and drivers
  + Skylight agent

## Best Practices<a name="custom_image_best_practices"></a>

Before you create an image from a WorkSpace, do the following:
+ Use a separate VPC that is not connected to your production environment\.
+ Deploy the WorkSpace in a private subnet and use a NAT instance for outbound traffic\.
+ Use a small Simple AD directory\.
+ Use the smallest volume size for the source WorkSpace, and then adjust the volume size as needed when creating the custom bundle\.
+ Install all operating system updates \(except Windows feature/version updates\) and all application updates on the WorkSpace\. For more information, see the [Important note](#important_note) at the start of this topic\.
+ Delete cached data from the WorkSpace that shouldn't be included in the bundle \(for example, browser history, cached files, and browser cookies\)\.
+ Delete configuration settings from the WorkSpace that shouldn't be included in the bundle \(for example, email profiles\)\.
+ Switch to dynamic IP address settings using DHCP\.
+ Make sure that you haven't exceeded your quota for WorkSpace images allowed in a Region\. By default, you're allowed 40 WorkSpace images per Region\. If you've reached this quota, new attempts to create an image will fail\. To request a quota increase, use the [Amazon WorkSpaces Limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=workspaces)\.
+ Make sure that you aren't trying to create an image from an encrypted WorkSpace\. Image creation from an encrypted WorkSpace is not currently supported\.
+ If you're running any antivirus software on the WorkSpace, disable it while you're attempting to create an image\.
+ If you have a firewall enabled on your WorkSpace, make sure that it isn't blocking any necessary ports\. For more information, see [IP Address and Port Requirements for Amazon WorkSpaces](workspaces-port-requirements.md)\.
+ For Windows WorkSpaces, don't configure any Group Policy Objects \(GPOs\) before image creation\.
+ For Windows WorkSpaces, do not customize the default user profile \(`C:\Users\Default`\) before creating an image\. We recommend making any customizations to the user profile through GPOs, and applying them after image creation\. GPOs can be easily modified or rolled back, and are therefore less prone to error than customizations made to the default user profile\.
+ For Linux WorkSpaces, see also the [ "Best Practices to Prepare Your Amazon WorkSpaces for Linux Images"](https://docs.aws.amazon.com/whitepapers/latest/workspaces-linux-best-practices/welcome.html) whitepaper\.

## Step 1: Run the Image Checker<a name="run_image_checker"></a>

**Note**  
The Image Checker is available only for Windows WorkSpaces\. If you are creating an image from a Linux WorkSpace, skip to [Step 2: Create a Custom Image and Custom Bundle](#create_custom_image_bundle)\.

To confirm that your Windows WorkSpace meets the requirements for image creation, we recommend running the Image Checker\. The Image Checker performs a series of tests on the WorkSpace that you want to use to create your image, and provides guidance on how to resolve any issues it finds\.

**Important**  
The WorkSpace must pass all of the tests run by the Image Checker before you can use it for image creation\. 
Before you run the Image Checker, verify that the latest Windows security and cumulative updates are installed on your WorkSpace\.
The Image Checker does not check the user profile size for Windows 10 WorkSpaces\. If you have a Windows 10 WorkSpace, make sure that the user profile size is less than 10 GB\.

To get the Image Checker, do one of the following:
+ Reboot your WorkSpace\. The Image Checker is downloaded automatically during the reboot and installed at `C:\Program Files\Amazon\ImageChecker.exe`\.
+ Download the Amazon WorkSpaces Image Checker from [https://tools\.amazonworkspaces\.com/ImageChecker\.zip](https://tools.amazonworkspaces.com/ImageChecker.zip) and extract the `ImageChecker.exe` file\. Copy this file to `C:\Program Files\Amazon\`\.

**To run the Image Checker**

1. On the Windows **Start** menu, choose **Windows System**, then choose **Command Prompt**\.

1. In the Command Prompt window, enter the following commands, one at a time, and press Enter after each command\.

   ```
   c:
   cd C:\Program Files\Amazon
   ImageChecker.exe
   ```

1. When asked "Do you want to allow this app to make changes to your device?" choose **Yes**\.

1. In the **Amazon WorkSpaces Image Checker** dialog box, choose **Run**\.

1. After each test is completed, you can view the status of the test\.

   For any test with a status of **FAILED**, choose **Info** to display information about how to resolve the issue that caused the failure\. For more information about how to resolve these issues, see [Tips for Resolving Issues Detected by the Image Checker](#image_checker_tips)\.

   If any tests display a status of **WARNING**, choose the **Fix All Warnings** button\.

   The tool generates an output log file in the same directory where the Image Checker is located\. By default, this file is located at `C:\Program Files\Amazon\ImageChecker_yyyyMMddhhmmss.log`\.
**Tip**  
Do not delete this log file\. If an issue occurs, this log file might be helpful in troubleshooting\.

1. If applicable, resolve any issues that cause test failures and warnings, and repeat the process of running the Image Checker until the WorkSpace passes all tests\. All failures and warnings must be resolved before you can create an image\.

1. After your WorkSpace passes all tests, you see a **Validation Successful** message\. You are now ready to create a custom bundle\.

### Tips for Resolving Issues Detected by the Image Checker<a name="image_checker_tips"></a>

In addition to consulting the following tips for resolving issues that are detected by the Image Checker, be sure to review the Image Checker log file at `C:\Program Files\Amazon\ImageChecker_yyyyMMddhhmmss.log`\.

#### PowerShell version 3\.0 or later must be installed<a name="tips_powershell"></a>

Install the latest version of [ Microsoft Windows PowerShell](https://docs.microsoft.com/powershell)\.

**Important**  
The PowerShell execution policy for a WorkSpace must be set to allow **RemoteSigned** scripts\. To check the execution policy, run the **Get\-ExecutionPolicy** PowerShell command\. If the execution policy is not set to **Unrestricted** or **RemoteSigned**, run the **Set\-ExecutionPolicy –ExecutionPolicy RemoteSigned** command to change the value of the execution policy\. The **RemoteSigned** setting allows the execution of scripts on Amazon WorkSpaces, which is required to create an image\.

#### Only the C and D drives can be present<a name="tips_local_drives"></a>

Only the `C` and `D` drives can be present on a WorkSpace that's used for imaging\. Remove all other drives, including virtual drives\.

#### No pending reboot due to Windows Updates can be detected<a name="tips_pending_updates"></a>
+ The Create Image process can't be run until Windows has been rebooted to finish installing security or cumulative updates\. Reboot Windows to apply these updates, and make sure that no other pending Windows security or cumulative updates need to be installed\.
+ Image creation is not supported on Windows 10 systems that have been upgraded from one version of Windows 10 to a newer version of Windows 10 \(a Windows feature/version upgrade\)\. However, Windows cumulative or security updates are supported by the WorkSpaces image\-creation process\.

#### The Sysprep file must exist and can't be blank<a name="tips_blank_sysprep"></a>

If there are problems with your Sysprep file, contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/) to get your EC2Config or EC2Launch repaired\.

#### The user profile size must be less than 10 GB<a name="tips_large_profile"></a>

The user profile \(`D:\Users\username`\) must be less than 10 GB total\. Remove files as needed to reduce the size of the user profile\.

#### Drive C must have enough free space<a name="tips_drive_c_full"></a>

You must have at least 12 GB of free space on drive `C`\. Remove files as needed to free up space on drive `C`\.

#### No services can be running under a domain account<a name="tips_services_domain_accounts"></a>

To run the Create Image process, no services on the WorkSpace can be running under a domain account\. All services must be running under a local account\.

**To run services under a local account**

1. Open `C:\Program Files\Amazon\ImageChecker_yyyyMMddhhmmss.log` and find the list of services that are running under a domain account\.

1. In the Windows search box, enter **services\.msc** to open the Windows Services Manager\.

1. Under **Log On As**, look for the services that are running under domain accounts\. \(Services running as **Local System**, **Local Service**, or **Network Service** do not interfere with image creation\.\)

1. Select a service that is running under a domain account, and then choose **Action**, **Properties**\.

1. Open the **Log On** tab\. Under **Log on as**, choose **Local System account**\. 

1. Choose **OK**\.

#### Amazon WorkSpaces Application Manager \(Amazon WAM\) must be installed<a name="tips_wam_not_installed"></a>

If you have used Amazon WAM to assign applications to your users, you must [set up the Amazon WAM installer on your WorkSpace](https://docs.aws.amazon.com/wam/latest/adminguide/setup_wam.html)\. When you are finished, the **Amazon WAM** shortcut will appear on your WorkSpace desktop\.

#### The WorkSpace must be configured to use DHCP<a name="tips_static_ip"></a>

You must configure all network adapters on the WorkSpace to use DHCP instead of static IP addresses\.

**To set all network adapters to use DHCP**

1. In the Windows search box, enter **control panel** to open the Control Panel\.

1. Choose **Network and Internet**\.

1. Choose **Network and Sharing Center**\.

1. Choose **Change adapter settings**, and select an adapter\.

1. Choose **Change settings of this connection**\.

1. On the **Networking** tab, select **Internet Protocol Version 4 \(TCP/IPv4\)**, and then choose **Properties**\.

1. In the **Internet Protocol Version 4 \(TCP/IPv4\) Properties** dialog box, select **Obtain an IP address automatically**\.

1. Choose **OK**\.

1. Repeat this process for all network adapters on the WorkSpace\.

#### Remote Desktop Services must be enabled<a name="tips_enable_rds"></a>

The Create Image process requires Remote Desktop Services to be enabled\.

**To enable Remote Desktop Services**

1. In the Windows search box, enter **services\.msc** to open the Windows Services Manager\.

1. In the **Name** column, find **Remote Desktop Services**\.

1. Select **Remote Desktop Services**, and then choose **Action**, **Properties**\.

1. On the **General** tab, for **Startup type**, choose **Manual** or **Automatic**\.

1. Choose **OK**\.

#### A user profile must exist<a name="tips_user_profile_missing"></a>

The WorkSpace that you're using to create images must have a user profile \(`D:\Users\username`\)\. If this test fails, contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/) for assistance\. 

#### The environment variable path must be properly configured<a name="tips_environment_variables"></a>

The environment variable path for the local machine is missing entries for System32 and for Windows PowerShell\. These entries are required for Create Image to run\.

**To configure your environment variable path**

1. In the Windows search box, enter **environment variables** and then choose **Edit the system environment variables**\.

1. In the **System Properties** dialog box, open the **Advanced** tab, and choose **Environment Variables**\.

1. In the **Environment Variables** dialog box, under **System variables**, select the **Path** entry and then choose **Edit**\.

1. Choose **New**, and add the following path:

   `C:\Windows\System32`

1. Choose **New** again, and add the following path:

   `C:\Windows\System32\WindowsPowerShell\v1.0\`

1. Choose **OK**\.

1. Restart the WorkSpace\.
**Tip**  
The order in which items appear in the environment variable path matters\. To determine the correct order, you might want to compare the environment variable path of your WorkSpace with one from a newly created WorkSpace or a new Windows instance\.

#### Windows Modules Installer must be enabled<a name="tips_enable_wmi"></a>

The Create Image process requires the Windows Modules Installer service to be enabled\.

**To enable the Windows Modules Installer service**

1. In the Windows search box, enter **services\.msc** to open the Windows Services Manager\.

1. In the **Name** column, find **Windows Modules Installer**\.

1. Select **Windows Modules Installer**, and then choose **Action**, **Properties**\.

1. On the **General** tab, for **Startup type**, choose **Manual** or **Automatic**\.

1. Choose **OK**\.

#### Amazon SSM Agent must be disabled<a name="tips_disable_ssm"></a>

The Create Image process requires the Amazon SSM Agent service to be disabled\.

**To disable the Amazon SSM Agent service**

1. In the Windows search box, enter **services\.msc** to open the Windows Services Manager\.

1. In the **Name** column, find **Amazon SSM Agent**\.

1. Select **Amazon SSM Agent**, and then choose **Action**, **Properties**\.

1. On the **General** tab, for **Startup type**, choose **Disabled**\.

1. Choose **OK**\.

#### SSL3 and TLS version 1\.2 must be enabled<a name="tips_enable_ssl_tls"></a>

To configure SSL/TLS for Windows, see [ How to Enable TLS 1\.2](https://docs.microsoft.com/configmgr/core/plan-design/security/enable-tls-1-2) in the Microsoft Windows documentation\. 

#### Only one user profile can exist on the WorkSpace<a name="tips_remove_extra_profiles"></a>

There can be only one WorkSpaces user profile \(`D:\Users\username`\) on the WorkSpace that you're using to create images\. Delete any user profiles that don't belong to the intended user of the WorkSpace\.

For image creation to work, your WorkSpace can have only three user profiles on it:
+ The user profile of the intended user of the WorkSpace \(`D:\Users\username`\)
+ The default user profile \(also known as Default Profile\)
+ The Administrator user profile

If there are additional user profiles, you can delete them through the advanced system properties in the Windows Control Panel\.

**To delete a user profile**

1. To access the advanced system properties, do one of the following:
   + Press the **Windows key\+Pause Break**, and then choose **Advanced system settings** in the left pane of the **Control Panel** > **System and Security** > **System** dialog box\.
   + In the Windows search box, enter **control panel**\. In the Control Panel, choose **System and Security**, then choose System, and then choose **Advanced system settings** in the left pane of the **Control Panel** > **System and Security** > **System** dialog box\.

1. In the **System Properties** dialog box, on the **Advanced** tab, choose **Settings** under **User Profiles**\.

1. If any profile is listed other than the Administrator profile, the Default Profile, and the profile of the intended WorkSpaces user, select that additional profile and choose **Delete**\.

1. When asked if you want to delete the profile, choose **Yes**\.

1. If necessary, repeat Steps 3 and 4 to remove any other profiles that don't belong on the WorkSpace\.

1. Choose **OK** twice and close the Control Panel\.

1. Restart the WorkSpace\.

#### No AppX packages can be in a staged state<a name="tips_unstage_appx"></a>

One or more AppX packages are in a staged state\. This might cause a Sysprep error during image creation\.

**To remove all staged AppX packages**

1. In the Windows search box, enter **powershell**\. Choose **Run as Administrator**\.

1. When asked "Do you want to allow this app to make changes to your device?", choose **Yes**\.

1. In the Windows PowerShell window, enter the following commands to list all staged AppX packages, and press Enter after each one\.

   ```
   $workSpaceUserName = $env:username
   ```

   ```
   $allAppxPackages = Get-AppxPackage -AllUsers
   ```

   ```
   $packages = $allAppxPackages |    Where-Object { `
                                   (($_.PackageUserInformation -like "*S-1-5-18*" -and !($_.PackageUserInformation -like "*$workSpaceUserName*")) -and `
                                   ($_.PackageUserInformation -like "*Staged*" -or $_.PackageUserInformation -like "*Installed*")) -or `
                                   ((!($_.PackageUserInformation -like "*S-1-5-18*") -and $_.PackageUserInformation -like "*$workSpaceUserName*") -and `
                                   $_.PackageUserInformation -like "*Staged*")
                                   }
   ```

1. Enter the following command to remove all staged AppX packages, and press Enter\.

   ```
   $packages | Remove-AppxPackage -ErrorAction SilentlyContinue
   ```

1. Run the Image Checker again\. If this test still fails, enter the following commands to remove all AppX packages, and press Enter after each one\.

   ```
   Get-AppxProvisionedPackage -Online | Remove-AppxProvisionedPackage -Online -ErrorAction SilentlyContinue
   ```

   ```
   Get-AppxPackage -AllUsers | Remove-AppxPackage -ErrorAction SilentlyContinue
   ```

#### Windows must not have been upgraded from a previous version<a name="tips_version_upgrade"></a>

Image creation is not supported on Windows systems that have been upgraded from one version of Windows 10 to a newer version of Windows 10 \(a Windows feature/version upgrade\)\.

To create images, use a WorkSpace that has not undergone a Windows feature/version upgrade\.

#### The Windows rearm count must not be 0<a name="tips_reset_rearm_count"></a>

The rearm feature allows you to extend the activation period for the trial version of Windows\. The Create Image process requires that the rearm count be a value other than 0\.

**To check the Windows rearm count**

1. On the Windows **Start** menu, choose **Windows System**, then choose **Command Prompt**\.

1. In the Command Prompt window, enter the following command, and then press Enter\.

   `cscript C:\Windows\System32\slmgr.vbs /dlv`

To reset the rearm count to a value other than 0, see [ Sysprep \(Generalize\) a Windows installation](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) in the Microsoft Windows documentation\.

#### Other Troubleshooting Tips<a name="images_troubleshooting_tips"></a>

If your WorkSpace passes all of the tests run by the Image Checker, but you are still unable to create an image from the WorkSpace, check for the following issues:
+ Make sure that the WorkSpace isn't assigned to a user within a **Domain Guests** group\. To check if there are any domain accounts, run the following PowerShell command\.

  ```
  Get-WmiObject -Class Win32_Service | Where-Object { $_.StartName -like "*$env:USERDOMAIN*" }
  ```
+ For Windows 7 WorkSpaces only: If problems occur while the user profile is being copied during image creation, check for the following issues:
  + Long profile paths can cause image creation errors\. Make sure that the paths of all folders within the user profile are less than 261 characters\.
  + Make sure to grant full permissions on the profile folder to the system and all application packages\.
  + If any files in the user profile are locked by a process or are in use during image creation, copying the profile might fail\.
+ Some Group Policy Objects \(GPOs\) restrict access to the RDP certificate thumbprint when it is requested by the EC2Config service or the EC2Launch scripts during Windows instance configuration\. Before you try to create an image, move the WorkSpace to a new organizational unit \(OU\) with blocked inheritance and no GPOs applied\.
+ Make sure that the Windows Remote Management \(WinRM\) service is configured to start automatically\. Do the following:

  1. In the Windows search box, enter **services\.msc** to open the Windows Services Manager\.

  1. In the **Name** column, find **Windows Remote Management \(WS\-Management\)**\. 

  1. Select **Windows Remote Management \(WS\-Management\)**, and then choose **Action**, **Properties**\.

  1. On the **General** tab, for **Startup type**, choose **Automatic**\.

  1. Choose **OK**\.

## Step 2: Create a Custom Image and Custom Bundle<a name="create_custom_image_bundle"></a>

After you have validated your WorkSpace image, you can proceed with creating your custom image and custom bundle\.

**To create a custom image and custom bundle**

1. If you are still connected to the WorkSpace, disconnect\.

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. <a name="step_create_image"></a>Select the WorkSpace and choose **Actions**, **Create Image**\.

1. A message displays, prompting you to restart your WorkSpace before continuing\. Restarting your WorkSpace updates your Amazon WorkSpaces software to the latest version\.

   Restart your WorkSpace by closing the message and following the steps in [Restart a WorkSpace](reboot-workspaces.md)\. When you're done, repeat [Step 4](#step_create_image) of this procedure, but this time choose **Next** when the restart message appears\. To create an image, the status of the WorkSpace must be **Available** and its modification state must be **None**\.

1. Enter an image name and a description that will help you identify the image, and then choose **Create Image**\. While the image is being created, the status of the WorkSpace is **Suspended** and the WorkSpace is unavailable\.

1. In the navigation pane, choose **Images**\. The image is complete when the status of the WorkSpace changes to **Available**\.

1. Select the image and choose **Actions**, **Create Bundle**\.

1. Enter a bundle name and a description, and then do the following: 
   + For **Bundle Type**, choose the hardware to use when launching WorkSpaces from this custom bundle\.
   + For **Root Volume Size**, leave the default value or enter a new value that is equal to or greater than the current size\. Then, enter a value for **User Volume Size**\.

     The default available sizes for the root volume \(for Microsoft Windows, the `C` drive, for Linux, /\) and the user volume \(for Windows, the `D` drive; for Linux, /home\) are as follows: 
     + Root: 80 GB, User: 10 GB, 50 GB, or 100 GB
     + Root: 175 GB, User: 100 GB
     + For Graphics and GraphicsPro WorkSpaces only: Root: 100 GB, User: 100 GB

     Alternatively, you can expand the root and user volumes up to 2000 GB each\.
**Note**  
To ensure that your data is preserved, you cannot decrease the size of the root or user volumes after you launch a WorkSpace\. Instead, make sure that you specify the minimum sizes for these volumes when launching a WorkSpace\. You can launch a Value, Standard, Performance, Power, or PowerPro WorkSpace with a minimum of 80 GB for the root volume and 10 GB for the user volume\. You can launch a Graphics or GraphicsPro WorkSpace with a minimum of 100 GB for the root volume and 100 GB for the user volume\.

1. Choose **Create Bundle**\.

## What's Included with Windows WorkSpaces Custom Images<a name="image_creation_windows"></a>

When you create an image from a Windows 7 or 10 WorkSpace, the entire contents of the `C` drive are included\.

For Windows 10 WorkSpaces, the user profile in `D:\Users\username` is not included in the custom image\.

For Windows 7 WorkSpaces, the entire contents of the user profile in `D:\Users\username` are included, except for the following:
+ Contacts
+ Downloads
+ Music
+ Pictures
+ Saved games
+ Videos
+ Podcasts
+ Virtual machines
+ \.virtualbox
+ Tracing
+ appdata\\local\\temp
+ appdata\\roaming\\apple computer\\mobilesync\\
+ appdata\\roaming\\apple computer\\logs\\
+ appdata\\roaming\\apple computer\\itunes\\iphone software updates\\
+ appdata\\roaming\\macromedia\\flash player\\macromedia\.com\\support\\flashplayer\\sys\\
+ appdata\\roaming\\macromedia\\flash player\\\#sharedobjects\\
+ appdata\\roaming\\adobe\\flash player\\assetcache\\
+ appdata\\roaming\\microsoft\\windows\\recent\\
+ appdata\\roaming\\microsoft\\office\\recent\\
+ appdata\\roaming\\microsoft office\\live meeting
+ appdata\\roaming\\microsoft shared\\livemeeting shared\\
+ appdata\\roaming\\mozilla\\firefox\\crash reports\\
+ appdata\\roaming\\mcafee\\common framework\\
+ appdata\\local\\microsoft\\feeds cache
+ appdata\\local\\microsoft\\windows\\temporary internet files\\
+ appdata\\local\\microsoft\\windows\\history\\
+ appdata\\local\\microsoft\\internet explorer\\domstore\\
+ appdata\\local\\microsoft\\internet explorer\\imagestore\\
+ appdata\\locallow\\microsoft\\internet explorer\\iconcache\\
+ appdata\\locallow\\microsoft\\internet explorer\\domstore\\
+ appdata\\locallow\\microsoft\\internet explorer\\imagestore\\
+ appdata\\local\\microsoft\\internet explorer\\recovery\\
+ appdata\\local\\mozilla\\firefox\\profiles\\

## What's Included with Amazon Linux WorkSpace Custom Images<a name="image_creation_linux"></a>

When you create an image from an Amazon Linux WorkSpace, the entire contents of the user volume \(/home\) are removed\. The contents of the root volume \(/\) are included, except the following folders and keys, which are removed:
+ /tmp
+ /var/spool/mail
+ /var/tmp
+ /var/lib/dhcp
+ /var/lib/cloud
+ /var/cache
+ /var/backups
+ /etc/sudoers\.d
+ /etc/udev/rules\.d/70\-persistent\-net\.rules
+ /etc/network/interfaces\.d/50\-cloud\-init\.cfg
+ /etc/security/access\.conf
+ /var/log/amazon/ssm
+ /var/log/pcoip\-agent
+ /var/log/skylight
+ /var/lock/\.skylight\.domain\-join\.lock
+ /var/lib/skylight/domain\-join\-status
+ /var/lib/skylight/configuration\-data
+ /var/lib/skylight/config\-data\.json
+ /home

The following keys are shredded during custom image creation:
+ /etc/ssh/ssh\_host\_\*\_key
+ /etc/ssh/ssh\_host\_\*\_key\.pub
+ /var/lib/skylight/tls\.\*
+ /var/lib/skylight/private\.key
+ /var/lib/skylight/public\.key