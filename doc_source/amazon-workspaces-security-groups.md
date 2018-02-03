# Security Groups for Your WorkSpaces<a name="amazon-workspaces-security-groups"></a>

When you register a directory with Amazon WorkSpaces, it creates two security groups, one for directory controllers and another for WorkSpaces in the directory\. The security group for directory controllers has a name that consists of the directory identifier followed by \_controllers \(for example, d\-92673056e8\_controllers\) and the security group for WorkSpaces has a name that consists of the directory identifier followed by \_workspacesMembers \(for example, d\-926720fc18\_workspacesMembers\)\.

You have the option to have an additional security group for WorkSpaces\. After you add the security group to the directory, it is associated with new WorkSpaces that you launch or existing WorkSpaces that you rebuild\.

**To add a security group to a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Update Details**\.

1. Expand **Security Group** and select a security group\.

1. Choose **Update and Exit**\.