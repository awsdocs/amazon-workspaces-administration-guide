# Manage Directories for Workspaces<a name="manage-workspaces-directory"></a>

Workspaces uses a directory to store and manage information for your WorkSpaces and users\. You can use one of the following options:
+ AD Connector — Use your existing on\-premises Microsoft Active Directory\. Users can sign into their WorkSpaces using their on\-premises credentials and access on\-premises resources from their WorkSpaces\.
+ AWS Managed Microsoft AD — Create a Microsoft Active Directory hosted on AWS\.
+ Simple AD — Create a directory that is compatible with Microsoft Active Directory, powered by Samba 4, and hosted on AWS\.
+ Cross trust — Create a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

For tutorials that demonstrate how to set up these directories and launch WorkSpaces, see [Launch a Virtual Desktop Using Workspaces](launch-workspaces-tutorials.md)\.

**Tip**  
For a detailed exploration of directory and virtual private cloud \(VPC\) design considerations for various deployment scenarios, see the [ *Best Practices for Deploying Amazon Workspaces*](https://d1.awsstatic.com/whitepapers/Best-Practices-for-Deploying-Amazon-WorkSpaces.pdf) whitepaper\.

After you create a directory, you'll perform most directory administration tasks using tools such as the Active Directory Administration Tools\. You can perform some directory administration tasks using the Workspaces console and other tasks using Group Policy\. For more information about managing users and groups, see [Manage WorkSpaces Users](manage-workspaces-users.md) and [Set Up Active Directory Administration Tools for Workspaces](directory_administration.md)\.

**Note**  
Shared directories are not currently supported for use with Amazon Workspaces\.
If you configure your AWS Managed Microsoft AD directory for multi\-Region replication, only the directory in the primary Region can be registered for use with Amazon Workspaces\. Attempts to register the directory in a replicated Region for use with Amazon Workspaces will fail\. Multi\-Region replication with AWS Managed Microsoft AD isn't supported for use with Amazon Workspaces within replicated Regions\.
Simple AD and AD Connector are made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD or AD Connector directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon Workspaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
To delete empty directories, see [Delete the Directory for Your WorkSpaces](delete-workspaces-directory.md)\. If you delete your Simple AD or AD Connector directory, you can always create a new one when you want to start using WorkSpaces again\.

**Topics**
+ [Register a Directory with Workspaces](register-deregister-directory.md)
+ [Update Directory Details for Your WorkSpaces](update-directory-details.md)
+ [Update DNS Servers for Amazon Workspaces](update-dns-server.md)
+ [Delete the Directory for Your WorkSpaces](delete-workspaces-directory.md)
+ [Enable Amazon WorkDocs for AWS Managed Microsoft AD](enable-workdocs-active-directory.md)
+ [Set Up Active Directory Administration Tools for Workspaces](directory_administration.md)