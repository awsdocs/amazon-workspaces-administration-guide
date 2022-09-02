# Manage directories for WorkSpaces<a name="manage-workspaces-directory"></a>

WorkSpaces uses a directory to store and manage information for your WorkSpaces and users\. You can use one of the following options:
+ AD Connector — Use your existing on\-premises Microsoft Active Directory\. Users can sign into their WorkSpaces using their on\-premises credentials and access on\-premises resources from their WorkSpaces\.
+ AWS Managed Microsoft AD — Create a Microsoft Active Directory hosted on AWS\.
+ Simple AD — Create a directory that is compatible with Microsoft Active Directory, powered by Samba 4, and hosted on AWS\.
+ Cross trust — Create a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

For tutorials that demonstrate how to set up these directories and launch WorkSpaces, see [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\.

**Tip**  
For a detailed exploration of directory and virtual private cloud \(VPC\) design considerations for various deployment scenarios, see [Best Practices for Deploying Amazon WorkSpaces](https://docs.aws.amazon.com/whitepapers/latest/best-practices-deploying-amazon-workspaces/best-practices-deploying-amazon-workspaces.html)\.

After you create a directory, you'll perform most directory administration tasks using tools such as the Active Directory Administration Tools\. You can perform some directory administration tasks using the WorkSpaces console and other tasks using Group Policy\. For more information about managing users and groups, see [Manage WorkSpaces users](manage-workspaces-users.md) and [Set up Active Directory Administration Tools for WorkSpaces](directory_administration.md)\.

**Note**  
Shared directories are not currently supported for use with Amazon WorkSpaces\.
If you configure your AWS Managed Microsoft AD directory for multi\-Region replication, only the directory in the primary Region can be registered for use with Amazon WorkSpaces\. Attempts to register the directory in a replicated Region for use with Amazon WorkSpaces will fail\. Multi\-Region replication with AWS Managed Microsoft AD isn't supported for use with Amazon WorkSpaces within replicated Regions\.
Simple AD and AD Connector are made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD or AD Connector directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon WorkSpaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
To delete empty directories, see [Delete the directory for your WorkSpaces](delete-workspaces-directory.md)\. If you delete your Simple AD or AD Connector directory, you can always create a new one when you want to start using WorkSpaces again\.

**Topics**
+ [Register a directory with WorkSpaces](register-deregister-directory.md)
+ [Update directory details for your WorkSpaces](update-directory-details.md)
+ [Update DNS servers for Amazon WorkSpaces](update-dns-server.md)
+ [Delete the directory for your WorkSpaces](delete-workspaces-directory.md)
+ [Enable Amazon WorkDocs for AWS Managed Microsoft AD](enable-workdocs-active-directory.md)
+ [Set up Active Directory Administration Tools for WorkSpaces](directory_administration.md)