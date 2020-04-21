# Security Groups for Your WorkSpaces<a name="amazon-workspaces-security-groups"></a>

When you register a directory with Amazon WorkSpaces, it creates two security groups, one for directory controllers and another for WorkSpaces in the directory\. The security group for directory controllers has a name that consists of the directory identifier followed by \_controllers \(for example, d\-92673056e8\_controllers\) and the security group for WorkSpaces has a name that consists of the directory identifier followed by \_workspacesMembers \(for example, d\-926720fc18\_workspacesMembers\)\.

**Important**  
Do not delete the **\_workspacesMembers** security group\. If you delete this security group, your WorkSpaces won't function correctly and you won't be able to recreate this group and add it back\. 



You can add the security group to the directory. Once a new security group is associated with a directory, new WorkSpaces that you launch or existing WorkSpaces that you rebuild will have the new group\.

**To add a security group to a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

2. In the navigation pane, choose **Directories**\.

3. Select the directory and choose **Actions**, **Update Details**\.

4. Expand **Security Group** and select a security group\.

5. Choose **Update and Exit**\.


You can also add a security group to the network interface of the an existing WorkSpace.  \.

**To add a security group to an existing WorkSpace**
Workspaces have Elastic Network Interfaces (ENI) which can controlled within the account. To add security groups to existing WorkSpaces the Security Group will need to be assigned to the ENI.  

	1. Find the IP Address for each WorkSpace which needs to be updated.
		a. Go to the WorkSpace console. https://console.aws.amazon.com/workspaces/
		b. Expand the 1st WorkSpace and record the WorkSpace IP.
		c. Repeat for the other WorkSpace.
	2. Find the ENI for the WorkSpace and update the Security Group assignment. 
		a. Go to the EC2 console. https://console.aws.amazon.com/ec2/.
		b. Under Network & Security choose **Network Interfaces**.
		c. Search for the IP addresses found in Step 1.
		d. For the WorkSpace, select the ENI and choose **Actions**, then choose **Change Security Groups**.
		d. Select the new Security Group(s).