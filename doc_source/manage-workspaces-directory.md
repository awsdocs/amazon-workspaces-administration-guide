# Manage Directories for Amazon WorkSpaces<a name="manage-workspaces-directory"></a>

Amazon WorkSpaces uses a directory to store and manage information for your WorkSpaces and users\. You can use one of the following options:
+ AD Connector — Use your existing on\-premises Microsoft Active Directory\. Users can sign into their WorkSpaces using their on\-premises credentials and access on\-premises resources from their WorkSpaces\.
+ Microsoft AD — Create a Microsoft Active Directory hosted on AWS\.
+ Simple AD — Create a directory that is compatible with Microsoft Active Directory, powered by Samba 4, and hosted on AWS\.
+ Cross trust — Create a trust relationship between your Microsoft AD directory and your on\-premises domain\.

For tutorials that demonstrate how to set up these directories and launch WorkSpaces, see [Launch a Virtual Desktop Using Amazon WorkSpaces](launch-workspaces-tutorials.md)\.

After you create a directory, you'll perform most directory administration tasks using tools such as the Active Directory Administration Tools\. You can perform some directory administration tasks using the Amazon WorkSpaces console and other tasks using Group Policy\. For more information about managing users and groups, see [Manage WorkSpaces Users](manage-workspaces-users.md) and [Set Up Active Directory Administration Tools for Amazon WorkSpaces](directory_administration.md)\.

**Note**  
Shared directories are not currently supported with WorkSpaces\.

**Topics**
+ [Register a Directory with Amazon WorkSpaces](register-deregister-directory.md)
+ [Update Directory Details for Your WorkSpaces](update-directory-details.md)
+ [Update DNS Servers for Amazon WorkSpaces](update-dns-server.md)
+ [Delete the Directory for Your WorkSpaces](delete-workspaces-directory.md)
+ [Enable Amazon WorkDocs for Microsoft Active Directory](enable-workdocs-active-directory.md)
+ [Set Up Active Directory Administration Tools for Amazon WorkSpaces](directory_administration.md)
+ [Manage Your Windows WorkSpaces Using Group Policy](group_policy.md)
+ [Manage Your Amazon Linux WorkSpaces](manage_linux_workspace.md)