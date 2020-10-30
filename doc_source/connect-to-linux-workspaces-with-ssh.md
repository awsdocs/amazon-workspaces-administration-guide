# Enable SSH Connections for Your Linux WorkSpaces<a name="connect-to-linux-workspaces-with-ssh"></a>

If you or your users want to connect to your Amazon Linux WorkSpaces by using the command line, you can enable SSH connections\. You can enable SSH connections to all WorkSpaces in a directory or to individual WorkSpaces in a directory\. 

To enable SSH connections, you create a new security group or update an existing security group and add a rule to allow inbound traffic for this purpose\. Security groups act as a firewall for associated instances, controlling both inbound and outbound traffic at the instance level\. After you create or update your security group, your users and others can use PuTTY or other terminals to connect from their devices to your Amazon Linux WorkSpaces\.

For a video tutorial, see [ How can I connect to my Linux Amazon WorkSpaces using SSH?](https://aws.amazon.com/premiumsupport/knowledge-center/linux-workspace-ssh/) on the AWS Knowledge Center\.

**Topics**
+ [Prerequisites for SSH Connections to Amazon Linux WorkSpaces](#before-you-begin-enable-ssh-linux-workspaces)
+ [Enable SSH Connections to All Amazon Linux WorkSpaces in a Directory](#enable-ssh-directory-level-access-linux-workspaces)
+ [Enable SSH Connections to a Specific Amazon Linux WorkSpace](#enable-ssh-access-specific-linux-workspace)
+ [Connect to an Amazon Linux WorkSpace by Using Linux or PuTTY](#ssh-connection-linux-workspace-using-linux-or-putty)

## Prerequisites for SSH Connections to Amazon Linux WorkSpaces<a name="before-you-begin-enable-ssh-linux-workspaces"></a>
+ Enabling inbound SSH traffic to a WorkSpace — To add a rule to allow inbound SSH traffic to one or more Amazon Linux WorkSpaces, make sure that you have the public or private IP addresses of the devices that require SSH connections to your WorkSpaces\. For example, you can specify the public IP addresses of devices outside your virtual private cloud \(VPC\) or the private IP address of another EC2 instance in the same VPC as your WorkSpace\. 

  If you plan to connect to a WorkSpace from your local device, you can use the search phrase "what is my IP address" in an internet browser or use the following service: [Check IP](https://checkip.amazonaws.com/)\. 
+ Connecting to a WorkSpace — The following information is required to initiate an SSH connection from a device to an Amazon Linux WorkSpace\.
  + The NetBIOS name of the Active Directory domain that you are connected to\.
  + Your WorkSpace user name\.
  + The public or private IP address of the WorkSpace that you want to connect to\.

    Private: If your VPC is attached to a corporate network and you have access to that network, you can specify the private IP address of the WorkSpace\.

    Public: If your WorkSpace has a public IP address, you can use the WorkSpaces console to find the public IP address, as described in the following procedure\.

**To find the IP addresses for the Amazon Linux WorkSpace you want to connect to and your user name**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. In the list of WorkSpaces, choose the WorkSpace that you want to enable SSH connections to\.

1. In the **Running mode** column, confirm that the WorkSpace status is **Available**\.

1. Click the arrow to the left of the WorkSpace name to display the inline summary, and note the following information:
   + The **WorkSpace IP**\. This is the private IP address of the WorkSpace\.

     The private IP address is required for obtaining the elastic network interface associated with the WorkSpace\. The network interface is required to retrieve information such as the security group or public IP address associated with the WorkSpace\.
   + The WorkSpace **Username**\. This is the user name that you specify to connect to the WorkSpace\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Network Interfaces**\.

1. In the search box, type the **WorkSpace IP** that you noted in Step 5\. 

1. Select the network interface associated with the **WorkSpace IP**\. 

1. If your WorkSpace has a public IP address, it is displayed in the **IPv4 Public IP** column\. Make a note of this address, if applicable\.

**To find the NetBIOS name of the Active Directory domain that you are connected to**

1. Open the AWS Directory Service console at [https://console\.aws\.amazon\.com/directoryservicev2/](https://console.aws.amazon.com/directoryservicev2/)\.

1. In the list of directories, click the **Directory ID** link of the directory for the WorkSpace\.

1. In the **Directory details** section, note the **Directory NetBIOS name**\.

## Enable SSH Connections to All Amazon Linux WorkSpaces in a Directory<a name="enable-ssh-directory-level-access-linux-workspaces"></a>

To enable SSH connections to all Amazon Linux WorkSpaces in a directory, do the following\.

**To create a security group with a rule to allow inbound SSH traffic to all Amazon Linux WorkSpaces in a directory**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose **Create Security Group**\.

1. Type a name and optionally, a description for your security group\.

1. For **VPC**, choose the VPC that contains the WorkSpaces that you want to enable SSH connections to\.

1. On the **Inbound **tab, choose **Add Rule**, and do the following:
   + For **Type**, choose **SSH**\.
   + For **Protocol**, TCP is automatically specified when you choose **SSH**\.
   + For **Port Range**, 22 is automatically specified when you choose **SSH**\.
   + For **Source**, choose **My IP** or **Custom**, and specify a single IP address or an IP address range in CIDR notation\. For example, if your IPv4 address is `203.0.113.25`, specify `203.0.113.25/32` to list this single IPv4 address in CIDR notation\. If your company allocates addresses from a range, specify the entire range, such as `203.0.113.0/24`\.
   + For **Description** \(optional\), type a description for the rule\.

1. Choose **Create**\.

## Enable SSH Connections to a Specific Amazon Linux WorkSpace<a name="enable-ssh-access-specific-linux-workspace"></a>

To enable SSH connections to a specific Amazon Linux WorkSpace, do the following\.

**To add a rule to an existing security group to allow inbound SSH traffic to a specific Amazon Linux WorkSpace**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Network & Security**, choose **Network Interfaces**\.

1. In the search bar, type the private IP address of the WorkSpace that you want to enable SSH connections to\.

1. In the **Security groups** column, click the link for the security group\.

1. On the **Inbound **tab, choose **Edit**\.

1. Choose** Add Rule**, and then do the following:
   + For **Type**, choose **SSH**\.
   + For **Protocol**, TCP is automatically specified when you choose **SSH**\.
   + For **Port Range**, 22 is automatically specified when you choose **SSH**\.
   + For **Source**, choose **My IP** or **Custom**, and specify a single IP address or an IP address range in CIDR notation\. For example, if your IPv4 address is `203.0.113.25`, specify `203.0.113.25/32` to list this single IPv4 address in CIDR notation\. If your company allocates addresses from a range, specify the entire range, such as `203.0.113.0/24`\.
   + For **Description** \(optional\), type a description for the rule\.

1. Choose **Save**\.

## Connect to an Amazon Linux WorkSpace by Using Linux or PuTTY<a name="ssh-connection-linux-workspace-using-linux-or-putty"></a>

After you create or update your security group and add the required rule, your users and others can use Linux or PuTTY to connect from their devices to your WorkSpaces\. 

**Note**  
Before completing either of the following procedures, make sure that you have the following:  
The NetBIOS name of the Active Directory domain that you are connected to\.
The username that you use to connect to the WorkSpace\.
The public or private IP address of the WorkSpace that you want to connect to\.
For instructions on how to obtain this information, see "Prerequisites for SSH Connections to Amazon Linux WorkSpaces" earlier in this topic\.

**To connect to an Amazon Linux WorkSpace by using Linux**

1. Open the command prompt as an administrator and enter the following command\. For *NetBIOS name*, *Username*, and *WorkSpace IP*, enter the applicable values\. 

   ```
   ssh "NetBIOS_NAME\Username"@WorkSpaceIP
   ```

   The following is an example of the SSH command where:
   + The *NetBIOS\_NAME* is anycompany
   + The *Username *is janedoe
   + The *WorkSpace IP* is 203\.0\.113\.25

   ```
   ssh "anycompany\janedoe"@203.0.113.25
   ```

1. When prompted, enter the same password that you use when authenticating with the WorkSpaces client \(your Active Directory password\)\.

**To connect to an Amazon Linux WorkSpace by using PuTTY**

1. Open PuTTY\.

1. In the **PuTTY Configuration** dialog box, do the following:
   + For **Host Name \(or IP address\)**, enter the following command\. Replace the values with the NetBIOS name of the Active Directory domain that you are connected to, the user name that you use to connect to the WorkSpace, and the IP address of the WorkSpace that you want to connect to\.

     ```
     NetBIOS_NAME\Username@WorkSpaceIP
     ```
   + For **Port**, enter **22**\.
   + For **Connection type**, choose **SSH**\.

   For an example of the SSH command, see step 1 in the previous procedure\.

1.  Choose **Open**\.

1. When prompted, enter the same password that you use when authenticating with the WorkSpaces client \(your Active Directory password\)\.