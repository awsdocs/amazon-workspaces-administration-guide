# Configure a VPC for Amazon WorkSpaces<a name="amazon-workspaces-vpc"></a>

Amazon WorkSpaces launches your WorkSpaces in a virtual private cloud \(VPC\)\. If you use AWS Directory Service to create an AWS Managed Microsoft or a Simple AD, we recommend that you configure the VPC with one public subnet and two private subnets\. Configure your directory to launch your WorkSpaces in the private subnets\.

Note that you can associate an IPv6 CIDR block with your VPC and subnets\. However, if you configure your subnets to automatically assign IPv6 addresses to instances launched in the subnet, then you cannot launch WorkSpaces using the Performance or Graphics bundles\. By default, this setting is disabled\. To check this setting, open the Amazon VPC console, select your subnet, and choose **Subnet Actions**, **Modify auto\-assign IP settings**\.

To provide internet access to WorkSpaces in a private subnet, configure a NAT gateway in the public subnet\. For alternative methods of providing internet access for your WorkSpaces, see [Provide Internet Access from Your WorkSpace](amazon-workspaces-internet-access.md)\. Note that Windows WorkSpaces require an internet connection to receive applications through Amazon WAM\.

![\[Configure your WorkSpaces VPC\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/vpc-configuration-new.png)

To configure your VPC for use with Amazon WorkSpaces, complete the following tasks\. For more information about Amazon VPC, see the [Amazon VPC User Guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/)\.

**Topics**
+ [Step 1: Allocate an Elastic IP Address](#allocate-eip)
+ [Step 2: Create a VPC](#create-vpc)
+ [Step 3: Add a Second Private Subnet](#add-subnet)
+ [Step 4: Verify and Name the Route Tables](#verify-route-tables)

## Step 1: Allocate an Elastic IP Address<a name="allocate-eip"></a>

Allocate an Elastic IP address for your NAT gateway as follows\. Note that if you are using an alternative method of providing internet access, you can skip this step\.

**To allocate an Elastic IP address**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Elastic IPs**\.

1. Choose **Allocate new address**\.

1. On the **Allocate new address** page, choose **Allocate** and make a note of the Elastic IP address, then choose **Close**\.

## Step 2: Create a VPC<a name="create-vpc"></a>

Create a VPC with one public subnet and two private subnets as follows\.

**To set up a VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **VPC Dashboard**\.

1. Choose **Create VPC**\.

1. Choose **VPC with Public and Private Subnets** and then choose **Select**\.

1. Configure the VPC as follows:

   1. For **IPv4 CIDR block**, type the CIDR block for the VPC\.

   1. For **VPC name**, type a name for the VPC\.

1. Configure the public subnet as follows:

   1. For **IPv4 CIDR block**, type the CIDR block for the subnet\.

   1. For **Availability Zone**, keep `No Preference`\.

   1. For **Public subnet name**, type a name for the subnet \(for example, `WorkSpaces Public Subnet`\)\.

1. Configure the first private subnet as follows:

   1. For **Private subnet's IPv4 CIDR**, type the CIDR block for the subnet\.

   1. For **Availability Zone**, select the first one in the list \(for example, `us-west-2a`\)\.

   1. For **Private subnet name**, type a name for the subnet \(for example, `WorkSpaces Private Subnet 1`\)\.

1. For **Elastic IP Allocation ID**, choose the Elastic IP address that you created\. Note that if you are using an alternative method of providing internet access, you can skip this step\.

1. Choose **Create VPC**\. Note that it takes several minutes to set up your VPC\. After the VPC is created, choose **OK**\.

## Step 3: Add a Second Private Subnet<a name="add-subnet"></a>

In the previous step, you created a VPC with one public subnet and one private subnet\. Use the following procedure to add a second private subnet\.

**To add a subnet**

1. In the navigation pane, choose **Subnets**\.

1. Choose **Create Subnet**\.

1. For **Name tag**, type a name for the private subnet \(for example, `WorkSpaces Private Subnet 2`\)\.

1. For **VPC**, select the VPC that you created\.

1. For **Availability Zone**, select the second one in the list \(for example, `us-west-2b`\)\.

1. For **IPv4 CIDR block**, type the CIDR block for the subnet\.

1. Choose **Yes, Create**\.

## Step 4: Verify and Name the Route Tables<a name="verify-route-tables"></a>

You can verify and name the route tables that you created\.

**To verify the route tables**

1. In the navigation pane, choose **Subnets**, and select the public subnet that you created\.

1. On the **Route Table** tab, choose the ID of the route table \(for example, rtb\-12345678\)\.

1. Select the route table\. Type a name \(for example, `workspaces-public-routetable`\) and choose the check mark to save the name\.

1.  On the **Routes** tab, verify that there is one route for local traffic and another route that sends all other traffic to the internet gateway for the VPC\.

1. In the navigation pane, choose **Subnets**, and select the first private subnet that you created \(for example, `WorkSpaces Private Subnet 1`\)\.

1. On the **Route Table** tab, choose the ID of the route table\.

1. Select the route table\. Type a name \(for example, `workspaces-private-routetable`\) and choose the check mark to save the name\.

1.  On the **Routes** tab, verify that there is one route for local traffic and another route that sends all other traffic to the NAT gateway\.

1. In the navigation pane, choose **Subnets**, and select the second private subnet that you created \(for example, `WorkSpaces Private Subnet 2`\)\.

1.  On the **Routes** tab, verify that the route table is the private route table \(for example, `workspaces-privateroutetable)` \. If the route table is different, choose **Edit** and select this route table\.