# Create a Custom WorkSpaces Bundle<a name="create-custom-bundle"></a>

After you've launched a WorkSpace and customized it, you can create an image from the WorkSpace and then create a custom bundle from the image\. You can specify this bundle when you launch new WorkSpaces to ensure that they have the same configuration and software as the WorkSpace you used to create the bundle\.

**Requirements**

+ All applications to be included in the image must be installed on the `C:\` drive, or the user profile in `D:\Users\`*username*\. They must also be compatible with Microsoft Sysprep\.

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

**Best Practices**

Before you create an image from a WorkSpace, do the following:

+ Install all operating system and application updates on the WorkSpace\.

+ Delete cached data from the WorkSpace that shouldn't be included in the bundle \(for example, browser history, cached files, and browser cookies\)\.

+ Delete configuration settings from the WorkSpace that shouldn't be included in the bundle \(for example, email profiles\)\.

**To create a custom bundle**

1. If you are still connected to the WorkSpace, disconnect\.

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Create Image**\.

1. Type an image name and a description that will help you identify the image, and then choose **Create Image**\. The WorkSpace is unavailable while the image is being created\.

1. In the navigation pane, choose **Images**\. The image is complete when the status changes to **Available**\.

1. Select the image and choose **Actions**, **Create Bundle**\.

1. Type a bundle name and a description, select a hardware type, and choose **Create Bundle**\.

**Image Creation**

When you create an image, the entire contents of the `C:\` drive are included\. The entire contents of the user profile in `D:\Users\`*username* are included except for the following:

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