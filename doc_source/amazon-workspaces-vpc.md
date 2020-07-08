# Configure a VPC for Amazon WorkSpaces<a name="amazon-workspaces-vpc"></a>

Amazon WorkSpaces launches your WorkSpaces in a virtual private cloud \(VPC\)\. Your WorkSpaces must have access to the internet so that you can install updates to the operating system and deploy applications using Amazon WorkSpaces Application Manager \(Amazon WAM\)\.

You can create a VPC with two private subnets for your WorkSpaces and a NAT gateway in a public subnet\. Alternatively, you can create a VPC with two public subnets for your WorkSpaces and associate an Elastic IP address with each WorkSpace\.

**VPC Requirements**  
Your VPC's subnets must reside in different Availability Zones in the Region where you're launching WorkSpaces\. Availability Zones are distinct locations that are engineered to be isolated from failures in other Availability Zones\. By launching instances in separate Availability Zones, you can protect your applications from the failure of a single location\. Each subnet must reside entirely within one Availability Zone and cannot span zones\.

**Note**  
Amazon WorkSpaces is available in a subset of the Availability Zones in each supported Region\. To determine which Availability Zones you can use for the subnets of the VPC that you're using for WorkSpaces, see [Availability Zones for Amazon WorkSpaces](azs-workspaces.md)\. 

**Topics**
+ [Configure a VPC with Private Subnets and a NAT Gateway](#configure-vpc-nat-gateway)
+ [Configure a VPC with Public Subnets](#configure-vpc-public-subnets)

## Configure a VPC with Private Subnets and a NAT Gateway<a name="configure-vpc-nat-gateway"></a>

If you use AWS Directory Service to create an AWS Managed Microsoft or a Simple AD, we recommend that you configure the VPC with one public subnet and two private subnets\. Configure your directory to launch your WorkSpaces in the private subnets\. To provide internet access to WorkSpaces in a private subnet, configure a NAT gateway in the public subnet\.

![\[Configure your WorkSpaces VPC\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/vpc-configuration-new.png)

**Prerequisites**  
If you aren't already familiar with working with VPCs and subnets, we recommend reading [ VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide* before performing the following tasks\.

**Topics**
+ [Step 1: Allocate an Elastic IP Address](#allocate-eip)
+ [Step 2: Create a VPC](#create-vpc)
+ [Step 3: Add a Second Private Subnet](#add-subnet)
+ [Step 4: Verify and Name the Route Tables](#verify-route-tables)

### Step 1: Allocate an Elastic IP Address<a name="allocate-eip"></a>

Allocate an [ Elastic IP address](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html) for your NAT gateway as follows\. Note that if you are using an alternative method of providing internet access, you can skip this step\.

**To allocate an Elastic IP address**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Elastic IPs**\.

1. Choose **Allocate new address**\.

1. On the **Allocate new address** page, for **iPv4 address pool**, choose **Amazon pool** or **Owned by me**, and then choose **Allocate**\.

1. Make a note of the Elastic IP address, then choose **Close**\.

### Step 2: Create a VPC<a name="create-vpc"></a>

Create a VPC with one public subnet and two private subnets as follows\.

**To create the VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **VPC Dashboard** in the upper\-left corner\.

1. Choose **Launch VPC Wizard**\.

1. Choose **VPC with Public and Private Subnets** and then choose **Select**\.

1. Configure the VPC as follows:

   1. For **IPv4 CIDR block**, enter the CIDR block for the VPC\. We recommend that you use a CIDR block from the private \(non\-publicly routable\) IP address ranges specified in [RFC 1918](http://www.faqs.org/rfcs/rfc1918.html)\. For example, `10.0.0.0/16`\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

   1. For **IPv6 CIDR Block**, keep **No IPv6 CIDR Block\.**

   1. For **VPC name**, enter a name for the VPC\.

1. Configure the public subnet as follows:

   1. For **IPv4 CIDR block**, enter the CIDR block for the subnet\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

   1. For **Availability Zone**, keep **No Preference**\.

   1. For **Public subnet name**, enter a name for the subnet \(for example, `WorkSpaces Public Subnet`\)\.

1. <a name="step_private_first_az"></a>Configure the first private subnet as follows:

   1. For **Private subnet's IPv4 CIDR**, enter the CIDR block for the subnet\.

   1. To make an appropriate selection for **Availability Zone**, see [Availability Zones for Amazon WorkSpaces](azs-workspaces.md)\.

   1. For **Private subnet name**, enter a name for the subnet \(for example, `WorkSpaces Private Subnet 1`\)\.

1. For **Elastic IP Allocation ID**, choose the Elastic IP address that you created\. Note that if you are using an alternative method of providing internet access, you can skip this step\.

1. For **Service endpoints**, do nothing\.

1. For **Enable DNS hostnames**, keep **Yes**\.

1. For **Hardware tenancy**, keep **Default**\.

1. Choose **Create VPC**\. Note that it takes several minutes to set up your VPC\. After the VPC is created, choose **OK**\.

**Note**  
You can associate an IPv6 CIDR block with your VPC and subnets\. However, if you configure your subnets to automatically assign IPv6 addresses to instances launched in the subnet, then you cannot use Graphics bundles\. \(You can use GraphicsPro bundles, however\.\) This restriction arises from a hardware limitation of previous\-generation instance types that do not support IPv6\.  
To work around this issue, you can temporarily disable the **auto\-assign IPv6 addresses** setting on the WorkSpaces subnets before launching Graphics bundles, and then reenable this setting \(if needed\) after launching Graphics bundles so that any other bundles receive the desired IP addresses\.  
By default, the **auto\-assign IPv6 addresses** setting is disabled\. To check this setting from the Amazon VPC console, in the navigation pane, choose **Subnets**\. Select the subnet, and choose **Actions**, **Modify auto\-assign IP settings**\.  
For more information about working with IPv6 addresses, see [IP Addressing in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#ipv6-addresses) in the *Amazon VPC User Guide*\.

### Step 3: Add a Second Private Subnet<a name="add-subnet"></a>

In the previous step, you created a VPC with one public subnet and one private subnet\. Use the following procedure to add a second private subnet\.

**To add a private subnet**

1. In the navigation pane, choose **Subnets**\.

1. Choose **Create Subnet**\.

1. For **Name tag**, enter a name for the private subnet \(for example, `WorkSpaces Private Subnet 2`\)\.

1. For **VPC**, select the VPC that you created\.

1. To make an appropriate selection for **Availability Zone**, see [Availability Zones for Amazon WorkSpaces](azs-workspaces.md)\. Make sure you select a different Availability Zone from the one you selected for [Step 7](#step_private_first_az) earlier\.

1. For **IPv4 CIDR block**, enter the CIDR block for the subnet\.

1. Choose **Create**\.

### Step 4: Verify and Name the Route Tables<a name="verify-route-tables"></a>

You can verify and name the route tables for each subnet\.

**To verify and name the route tables**

1. In the navigation pane, choose **Subnets**, and select the public subnet that you created\.

   1. On the **Route Table** tab, choose the ID of the route table \(for example, rtb\-12345678\)\.

   1. Select the route table\. Under **Name**, choose the edit icon \(the pencil\), and enter a name \(for example, `workspaces-public-routetable`\), and then choose the check mark to save the name\.

   1. On the **Routes** tab, verify that there is one route for local traffic and another route that sends all other traffic to the internet gateway for the VPC\.

1. In the navigation pane, choose **Subnets**, and select the first private subnet that you created \(for example, `WorkSpaces Private Subnet 1`\)\.

   1. On the **Route Table** tab, choose the ID of the route table\.

   1. Select the route table\. Under **Name**, choose the edit icon \(the pencil\), and enter a name \(for example, `workspaces-private-routetable`\), and then choose the check mark to save the name\.

   1. On the **Routes** tab, verify that there is one route for local traffic and another route that sends all other traffic to the NAT gateway\.

1. In the navigation pane, choose **Subnets**, and select the second private subnet that you created \(for example, `WorkSpaces Private Subnet 2`\)\. On the **Route Table** tab, verify that the route table is the private route table \(for example, `workspaces-private-routetable`\)\. If the route table is different, choose **Edit** and select this route table\.

## Configure a VPC with Public Subnets<a name="configure-vpc-public-subnets"></a>

If you prefer, you can create a VPC with two public subnets\. To provide internet access to WorkSpaces in public subnets, configure the directory to assign Elastic IP addresses automatically or manually assign an Elastic IP address to each WorkSpace\.

**Prerequisites**  
If you aren't already familiar with working with VPCs and subnets, we recommend reading [ VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide* before performing the following tasks\.

**Topics**
+ [Step 1: Create a VPC](#create-vpc-public-subnet)
+ [Step 2: Add a Second Public Subnet](#add-second-public-subnet)
+ [Step 3: Assign the Elastic IP Address](#assign-eip)

### Step 1: Create a VPC<a name="create-vpc-public-subnet"></a>

Create a VPC with one public subnet as follows\.

**To create the VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **VPC Dashboard** in the upper\-left corner\.

1. Choose **Launch VPC Wizard**\.

1. Choose **VPC with a Single Public Subnet** and then choose **Select**\.

1. For **IPv4 CIDR block**, enter the CIDR block for the VPC\. We recommend that you use a CIDR block from the private \(non\-publicly routable\) IP address ranges specified in [RFC 1918](http://www.faqs.org/rfcs/rfc1918.html)\. For example, `10.0.0.0/16`\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

1. For **IPv6 CIDR block**, keep **No IPv6 CIDR Block**\.

1. For **VPC name**, enter a name for the VPC\.

1. For **Public subnet's IPv4 CIDR**, enter the CIDR block for the subnet\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

1. <a name="step_public_first_az"></a>To make an appropriate selection for **Availability Zone**, see [Availability Zones for Amazon WorkSpaces](azs-workspaces.md)\.

1. \(Optional\) For **Subnet name**, enter a name for the subnet\.

1. For **Service endpoints**, do nothing\.

1. For **Enable DNS hostnames**, keep **Yes**\.

1. For **Hardware tenancy**, keep **Default**\.

1. Choose **Create VPC**\. After the VPC is created, choose **OK**\.

**Note**  
You can associate an IPv6 CIDR block with your VPC and subnets\. However, if you configure your subnets to automatically assign IPv6 addresses to instances launched in the subnet, then you cannot use Graphics bundles\. \(You can use GraphicsPro bundles, however\.\) This restriction arises from a hardware limitation of previous\-generation instance types that do not support IPv6\.  
To work around this issue, you can temporarily disable the **auto\-assign IPv6 addresses** setting on the WorkSpaces subnets before launching Graphics bundles, and then reenable this setting \(if needed\) after launching Graphics bundles so that any other bundles receive the desired IP addresses\.  
By default, the **auto\-assign IPv6 addresses** setting is disabled\. To check this setting from the Amazon VPC console, in the navigation pane, choose **Subnets**\. Select the subnet, and choose **Actions**, **Modify auto\-assign IP settings**\.  
For more information about working with IPv6 addresses, see [IP Addressing in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#ipv6-addresses) in the *Amazon VPC User Guide*\.

### Step 2: Add a Second Public Subnet<a name="add-second-public-subnet"></a>

In the previous step, you created a VPC with one public subnet\. Use the following procedure to add a second public subnet and associate it with the route table for the first public subnet, which has a route to the internet gateway for the VPC\.

**To add a public subnet**

1. In the navigation pane, choose **Subnets**\.

1. Choose **Create Subnet**\.

1. For **Name tag**, enter a name for the subnet\.

1. For **VPC**, select the VPC that you created\.

1. To make an appropriate selection for **Availability Zone**, see [Availability Zones for Amazon WorkSpaces](azs-workspaces.md)\. Make sure you select a different Availability Zone from the one you selected for [Step 9](#step_public_first_az) earlier\.

1. For **IPv4 CIDR block**, enter the CIDR block for the subnet\.

1. Choose **Create**\. After the subnet is created, choose **Close**\.

1. Associate the new public subnet with the route table created for the first subnet as follows:

   1. In the navigation pane, choose **Subnets**\.

   1. Select the first subnet\.

   1. On the **Route Table** tab, choose the ID of the route table\.

   1. On the **Subnet Associations** tab, choose **Edit subnet associations**\.

   1. Select the check box for the second subnet \(the public subnet you just created\) and choose **Save**\.

### Step 3: Assign the Elastic IP Address<a name="assign-eip"></a>

You can assign [ Elastic IP addresses](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html) \(static public IP addresses\) to your WorkSpaces automatically or manually\. To use automatic assignment, see [Configure Automatic IP Addresses](update-directory-details.md#automatic-assignment)\. To assign Elastic IP addresses manually, use the following procedure\.

For a video tutorial about how to assign an Elastic IP address to a WorkSpace, see [ How do I associate an Elastic IP Address with a WorkSpace?](https://aws.amazon.com/premiumsupport/knowledge-center/associate-elastic-ip-workspace/) on the AWS Knowledge Center\.

**Warning**  
We recommend that you not modify the elastic network interface of the WorkSpace after it is launched\. If you have enabled automatic assignment of Elastic IP addresses at the directory level, an Elastic IP address \(from the Amazon\-provided pool\) is assigned to your WorkSpace when it is launched\. However, if you associate an Elastic IP address that you own to a WorkSpace, and then you later disassociate that Elastic IP address from the WorkSpace, the WorkSpace loses its public IP address, and it doesn't automatically get a new one from the Amazon\-provided pool\.  
To associate a new public IP address from the Amazon\-provided pool with the WorkSpace, you must [rebuild the WorkSpace](rebuild-workspace.md)\. If you don't want to rebuild the WorkSpace, you must associate another Elastic IP address that you own to the WorkSpace\.

**To assign an Elastic IP address to a WorkSpace manually**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Expand the row \(choose the arrow icon\) for the WorkSpace and note the value of **WorkSpace IP**\. This is the primary private IP address of the WorkSpace\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Elastic IPs**\. If you do not have an available Elastic IP address, choose **Allocate new address** and choose **Amazon pool** or **Owned by me**, and then choose **Allocate**\. Make note of the new IP address\.

1. In the navigation pane, choose **Network Interfaces**\.

1. Select the network interface for your WorkSpace\. Note that the value of **VPC ID** matches the ID of your WorkSpaces VPC and the value of **Primary private IPv4 IP** matches the primary private IP address of the WorkSpace that you noted earlier\.

1. Choose **Actions**, **Manage IP Addresses**\. Choose **Assign new IP**, and then choose **Yes, Update**\. Make note of the new IP address\.

1. Choose **Actions**, **Associate Address**\.

1. On the **Associate Elastic IP Address** page, choose an Elastic IP address from **Address**\. For **Associate to private IP address**, specify the new private IP address, and then choose **Associate Address**\.