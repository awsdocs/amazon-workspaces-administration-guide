# Launch a virtual desktop using WorkSpaces<a name="launch-workspaces-tutorials"></a>

With WorkSpaces, you can provision virtual, cloud\-based Microsoft Windows or Amazon Linux desktops for your users, known as *WorkSpaces*\.

**Note**  
The **Computer Name** value shown for a WorkSpace in the Amazon WorkSpaces console varies, depending on which type of WorkSpace you've launched \(Linux or Windows\)\. The computer name for a WorkSpace can be in one of these formats:   
**Linux**: A\-1*xxxxxxxxxxxx*
**Windows**: IP\-C*xxxxxx* or WSAMZN\-*xxxxxxx* or EC2AMAZ\-*xxxxxxx*
For Windows WorkSpaces, the computer name format is determined by the bundle type, and in the case of WorkSpaces created from public bundles or from custom bundles based on public images, by when the public images were created\.  
Starting June 22, 2020, Windows WorkSpaces launched from public bundles have the WSAMZN\-*xxxxxxx* format for their computer names instead of the IP\-C*xxxxxx* format\.  
For custom bundles based on a public image, if the public image was created before June 22, 2020, the computer names are in the EC2AMAZ\-*xxxxxxx* format\. If the public image was created on or after June 22, 2020, the computer names are in the WSAMZN\-*xxxxxxx* format\.   
For Bring Your Own License \(BYOL\) bundles, either the DESKTOP\-*xxxxxxx* or the EC2AMAZ\-*xxxxxxx* format is used for the computer names by default\.  
If you've specified a custom format for the computer names in your custom or BYOL bundles, your custom format overrides these defaults\. To specify a custom format, see [Create a custom WorkSpaces image and bundle](create-custom-bundle.md)\.  
**Important** — If you change the computer name for a WorkSpace through the Windows system settings, you will no longer be able to access the WorkSpace\.

WorkSpaces uses a directory to store and manage information for your WorkSpaces and users\. You can do any of the following:
+ Create a Simple AD directory\.
+ Create an AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD\.
+ Connect to an existing Microsoft Active Directory by using Active Directory Connector\.
+ Create a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

**Note**  
Shared directories are not currently supported for use with Amazon WorkSpaces\.
If you configure your AWS Managed Microsoft AD directory for multi\-Region replication, only the directory in the primary Region can be registered for use with Amazon WorkSpaces\. Attempts to register the directory in a replicated Region for use with Amazon WorkSpaces will fail\. Multi\-Region replication with AWS Managed Microsoft AD isn't supported for use with Amazon WorkSpaces within replicated Regions\.
Simple AD and AD Connector are made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD or AD Connector directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon WorkSpaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
To delete empty directories, see [Delete the directory for your WorkSpaces](delete-workspaces-directory.md)\. If you delete your Simple AD or AD Connector directory, you can always create a new one when you want to start using WorkSpaces again\.

The following tutorials show you how to launch a WorkSpace by using the supported directory service options\.

**Topics**
+ [Launch a WorkSpace using AWS Managed Microsoft AD](launch-workspace-microsoft-ad.md)
+ [Launch a WorkSpace using Simple AD](launch-workspace-simple-ad.md)
+ [Launch a WorkSpace using AD Connector](launch-workspace-ad-connector.md)
+ [Launch a WorkSpace using a trusted domain](launch-workspace-trusted-domain.md)