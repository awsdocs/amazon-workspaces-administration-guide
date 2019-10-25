# Create a Custom WorkSpaces Bundle<a name="create-custom-bundle"></a>

After you've launched a Windows or Amazon Linux WorkSpace and customized it, you can create an image from the WorkSpace and then create a custom bundle from the image\. You can specify this bundle when you launch new WorkSpaces to ensure that they have the same configuration and software as the WorkSpace you used to create the bundle\.<a name="important_note"></a>

**Important**  
If you plan to create an image from a Windows 10 WorkSpace, note that image creation is not supported on Windows 10 systems that have been upgraded from one version of Windows 10 to a newer version of Windows 10 \(a Windows feature/version upgrade\)\. However, Windows cumulative or security updates are supported by the WorkSpaces image\-creation process\.

**Requirements to create Windows custom images**
+ All applications and user profiles on WorkSpaces images must be compatible with Microsoft Sysprep\.
+ All applications to be included in the image must be installed on the `C:` drive\.
+ Customized Windows user profiles for Windows 7 bring your own Windows License \(BYOL\) or Windows Server 2008 R2 custom images must be placed in `D:\Users\username` before you run **Create Image** on the WorkSpace\.
+ Customized Windows user profiles for Windows 10 BYOL or Windows Server 2016 custom images must be placed in `C:\Users\Default` before you run **Create Image** on the WorkSpace\.
+ The user profile must exist and its total size \(files and data\) must be less than 10 GB\.
+ The `C:\` drive must have enough available space for the contents of the user profile, plus an additional 2 GB\.
+ All application services running on the WorkSpace must use a local system account instead of domain user credentials\. For example, you cannot have a Microsoft SQL Server Express installation running with a domain user's credentials\.
+ The following components are required in an image; otherwise, the WorkSpaces you launch from the image will not function correctly:
  + PowerShell
  + Remote Desktop Services
  + AWS PV drivers
  + EC2Config or EC2Launch \(Windows Server 2016\)
  + \[EC2Launch 1\.2\.0 or earlier\] Windows Remote Management \(WinRM\)
  + Teradici PCoIP agents and drivers
  + STXHD agents and drivers
  + AWS and WorkSpaces certificates
  + Skylight agent

**Requirements to create Amazon Linux custom images**
+ All applications to be included in the image must be installed outside of the /home directory \(or user volume\)\.
+ The root volume \(/\) should be less than 97% full\.
+ The following components are required in an image; otherwise, the WorkSpaces you launch from the image will not function correctly:
  + Cloud\-init
  + Teradici PCoIP agents and drivers
  + Skylight agent

**Best Practices**

Before you create an image from a WorkSpace, do the following:
+ Install all operating system updates \(except Windows feature/version updates\) and all application updates on the WorkSpace\. For more information, see the [Important note](#important_note) at the start of this topic\.
+ Delete cached data from the WorkSpace that shouldn't be included in the bundle \(for example, browser history, cached files, and browser cookies\)\.
+ Delete configuration settings from the WorkSpace that shouldn't be included in the bundle \(for example, email profiles\)\.
+ Switch to Dynamic IP Address settings using DHCP\.

**To create a custom bundle**

1. If you are still connected to the WorkSpace, disconnect\.

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Create Image**\.

1. A message displays prompting you to restart your WorkSpace before continuing, to update your WorkSpaces software to the latest version necessary\. 

   Restart your WorkSpace if needed by closing the message and following the steps in [Restart a WorkSpace](reboot-workspaces.md)\. When you're done, repeat the previous step, and choose **Next** when this message appears\. To create an image, the status of the WorkSpace must be **Available** and its modification state must be **None**\.

1. Enter an image name and a description that will help you identify the image, and then choose **Create Image**\. While the image is being created, the status of the WorkSpace is **Suspended** and the WorkSpace is unavailable\.

1. In the navigation pane, choose **Images**\. The image is complete when the status changes to **Available**\.

1. Select the image and choose **Actions**, **Create Bundle**\.

1. Enter a bundle name and a description, and then do the following: 
   + For **Bundle Type**, choose the hardware from which your WorkSpace is launched\. 
   + For **Root Volume Size**, leave the default value or enter a new value that is equal or greater than the current size\. Then, enter a value for **User Volume Size**\.

      The available sizes for the root volume \(for Microsoft Windows, the C: drive, for Linux, /\) and the user volume \(for Windows, the D: drive; for Linux, /home\) are as follows: 
     + Root: 80 GB, User: 10 GB, 50 GB, or 100 GB
     + Root: 175 GB, User: 100 GB

     Alternatively, you can expand the root and user volumes up to 1000 GB each\.

1. Choose **Create Bundle**\.

**Image Creation for Windows WorkSpaces**

When you create an image from a Windows WorkSpace, the entire contents of the `C:\` drive are included\. The entire contents of the user profile in `D:\Users\username` are included except for the following:
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

**Image Creation for Amazon Linux WorkSpaces**

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