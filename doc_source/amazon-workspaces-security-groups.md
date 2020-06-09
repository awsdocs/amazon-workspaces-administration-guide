# Security Groups for Your WorkSpaces<a name="amazon-workspaces-security-groups"></a>

When you register a directory with Amazon WorkSpaces, it creates two security groups, one for directory controllers and another for WorkSpaces in the directory\. The security group for directory controllers has a name that consists of the directory identifier followed by **\_controllers** \(for example, d\-12345678e1\_controllers\)\. The security group for WorkSpaces has a name that consists of the directory identifier followed by **\_workspacesMembers** \(for example, d\-123456fc11\_workspacesMembers\)\.

**Warning**  
Do not delete the **\_workspacesMembers** security group\. If you delete this security group, your WorkSpaces won't function correctly, and you won't be able to recreate this group and add it back\. 

You can add a default WorkSpaces security group to a directory\. After you associate a new security group with a WorkSpaces directory, new WorkSpaces that you launch or existing WorkSpaces that you rebuild will have the new security group\. You can also [add this new default security group to existing WorkSpaces without rebuilding them](#security_group_existing_workspace), as explained later in this topic\.

When you associate multiple security groups with a WorkSpaces directory, the rules from each security group are effectively aggregated to create one set of rules\. We recommend condensing your security group rules as much as possible\.

For more information about security groups, see [ Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

**To add a security group to a WorkSpaces directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Update Details**\.

1. Expand **Security Group** and select a security group\.

1. Choose **Update and Exit**\.

To add a security group to an existing WorkSpace without rebuilding it, you assign the new security group to the elastic network interface \(ENI\) of the WorkSpace\.

**To add a security group to an existing WorkSpace**

1. Find the IP address for each WorkSpace that needs to be updated\.

   1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

   1. Expand each WorkSpace and record its WorkSpace IP address\.

1. Find the ENI for each WorkSpace and update its security group assignment\.

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. Under **Network & Security**, choose **Network Interfaces**\.

   1. Search for the first IP address that you recorded in Step 1\.

   1. Select the ENI associated with the IP address, choose **Actions**, and then choose **Change Security Groups**\.

   1. Select the new security group, and choose **Save**\.

   1. Repeat this process as needed for any other WorkSpaces\. 