# Update Directory Details for Your WorkSpaces<a name="update-directory-details"></a>

You can complete the following directory management tasks using the Amazon WorkSpaces console\.


+ [Select an Organizational Unit](#select-ou)
+ [Configure Automatic IP Addresses](#automatic-assignment)
+ [Control Device Access](#control-device-access)
+ [Manage Local Administrator Permissions](#local-admin-setting)
+ [Update the AD Connector Account \(AD Connector\)](#connect-account)
+ [Multi\-factor Authentication \(AD Connector\)](#connect-mfa)

## Select an Organizational Unit<a name="select-ou"></a>

WorkSpace machine accounts are placed in the default organizational unit \(OU\) for the WorkSpaces directory\. Initially, the machine accounts are placed in the Computers OU of your directory or the directory that your AD Connector is connected to\. You can select a different OU from your directory or connected directory, or specify an OU in a separate target domain\. Note that you can select only one OU per directory\.

After you select a new OU, the machine accounts for all WorkSpaces that are created or rebuilt are placed in the newly selected OU\.

**To select an organizational unit**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory and then choose **Actions**, **Update Details**\.

1. Expand **Target Domain and Organizational Unit**\.

1. To find an OU, you can type all or part of the OU name and choose **Search OU**\. Alternatively, you can choose **List all OU** to list all OUs\.

1. Select the OU and choose **Update and Exit**\.

1. \(Optional\) Rebuild the existing WorkSpaces to update the OU\. For more information, see [Rebuild a WorkSpace](reset-workspace.md)\.

**To specify a target domain and organizational unit**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory and then choose **Actions**, **Update Details**\.

1. Expand **Target Domain and Organizational Unit**\.

1. For **Selected OU**, type the full LDAP distinguished name for the target domain and OU and then choose **Update and Exit**\. For example, **OU=WorkSpaces\_machines,DC=machines,DC=example,DC=com**\.

1. \(Optional\) Rebuild the existing WorkSpaces to update the OU\. For more information, see [Rebuild a WorkSpace](reset-workspace.md)\.

## Configure Automatic IP Addresses<a name="automatic-assignment"></a>

After you enable automatic assignment of public IP addresses, each WorkSpace that you launch is assigned a public IP address\. This allows the WorkSpaces to access the Internet\. WorkSpaces that exist at the time you enable automatic assignment do not receive a public IP address until you rebuild them\.

Note that you do not need to enable automatic assignment of public IP access if you configured your VPC with a NAT gateway\. For more information, see [Configure a VPC for Amazon WorkSpaces](amazon-workspaces-vpc.md)\.

**Prerequisites**

+ Your VPC must have an attached Internet gateway\. For more information, see [Attaching an Internet Gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon VPC User Guide*\.

+ The route table for the WorkSpaces subnets must have one route for local traffic and another route that sends all other traffic to the Internet gateway\.

**To configure public IP addresses**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory for your WorkSpaces\.

1. Choose **Actions**, **Update Details**\.

1. Expand **Access to Internet** and select **Enable** or **Disable**\.

1. Choose **Update**\.

## Control Device Access<a name="control-device-access"></a>

You can specify the types of devices that have access to WorkSpaces\. In addition, you restrict access to WorkSpaces to trusted devices \(also known as managed devices\)\.

**To control device access to WorkSpaces**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and then choose **Actions**, **Update Details**\.

1. Expand **Access Control Options** and find the **Other Platforms** section\. By default, WorkSpaces Web Access is disabled and users can access their WorkSpaces from their iOS devices, Android devices, Chromebooks, and PCoIP zero client devices\.

1. Select the device types to enable and clear the device types to disable\. To block access from all selected device types, choose **Block**\.

1. \(Optional\) You can restrict access to trusted devices\. For more information, see [Restrict WorkSpaces Access to Trusted Devices](trusted-devices.md)\.

1. Choose **Update and Exit**\.

## Manage Local Administrator Permissions<a name="local-admin-setting"></a>

You can specify whether users are local administrators on their WorkSpaces, which enables them to install application and modify settings on their WorkSpaces\. Users are local administrators by default\. If you modify this setting, the change applies to all new WorkSpaces that you create and any WorkSpaces that you rebuild\.

**To modify local administrator permissions**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory and then choose **Actions**, **Update Details**\.

1. Expand **Local Administrator Setting**\.

1. To ensure that users are local administrators, choose **Enable**\. Otherwise, choose **Disable**\.

1. Choose **Update and Exit**\.

## Update the AD Connector Account \(AD Connector\)<a name="connect-account"></a>

You can update the AD Connector account that is used to read users and groups and join Amazon WorkSpaces machine accounts to your AD Connector directory\.

**To update the AD Connector account**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory and then choose **Actions**, **Update Details**\.

1. Expand **Update AD Connector Account**\.

1. Type the username and password for the new account\.

1. Choose **Update and Exit**\.

## Multi\-factor Authentication \(AD Connector\)<a name="connect-mfa"></a>

You can enable multi\-factor authentication for your AD Connector directory\.

**To enable multi\-factor authentication**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select your directory and then choose **Actions**, **Update Details**\.

1. Expand **Multi\-Factor Authentication** and then select **Enable Multi\-Factor Authentication**\.

1. For **RADIUS server IP address\(es\)**, type the IP addresses of your RADIUS server endpoints separated by commas, or type the IP address of your RADIUS server load balancer\.

1. For **Port**, type the port that your RADIUS server is using for communications\. Your on\-premises network must allow inbound traffic over the default RADIUS server port \(1812\) from AD Connector\.

1. For **Shared secret code** and **Confirm shared secret code**, type the shared secret code for your RADIUS server\.

1. For **Protocol**, choose the protocol for your RADIUS server\.

1. For **Server timeout**, type the time, in seconds, to wait for the RADIUS server to respond\. This value must be between 1 and 20\.

1. For **Max retries**, type the number of times to attempt communication with the RADIUS server\. This value must be between 0 and 10\.

1. Choose **Update and Exit**\.

Multi\-factor authentication is available when **RADIUS status** is **Enabled**\. While multi\-factor authentication is being set up, users cannot log in to their WorkSpaces\.