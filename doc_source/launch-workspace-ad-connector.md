# Launch a WorkSpace Using AD Connector<a name="launch-workspace-ad-connector"></a>

Amazon WorkSpaces enables you to provision virtual, cloud\-based Microsoft Windows desktops for your users, known as *WorkSpaces*\.

Amazon WorkSpaces uses directories to store and manage information for your WorkSpaces and users\. For your directory, you can choose from Simple AD, AD Connector, or AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD\. In addition, you can establish a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

In this tutorial, we launch a WorkSpace that uses AD Connector\. For tutorials that use the other options, see [Launch a Virtual Desktop Using Amazon WorkSpaces](launch-workspaces-tutorials.md)\.

**Topics**
+ [Before You Begin](#prereqs-ad-connector)
+ [Step 1: Create an AD Connector](#create-ad-connector)
+ [Step 2: Create a WorkSpace](#create-workspace-ad-connector)
+ [Step 3: Connect to the WorkSpace](#connect-workspace-ad-connector)
+ [Next Steps](#next-steps-ad-connector)

## Before You Begin<a name="prereqs-ad-connector"></a>
+ Amazon WorkSpaces is not available in every Region\. Verify the supported Regions and select a Region for your WorkSpaces\. For more information about the supported Regions, see [Amazon WorkSpaces Pricing by AWS Region](https://aws.amazon.com/workspaces/pricing/)\.
+ When you launch a WorkSpace, you must select a WorkSpace bundle\. A bundle is a combination of an operating system, and storage, compute, and software resources\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.
+ Create a virtual private cloud with at least two private subnets\. For more information, see [Configure a VPC for Amazon WorkSpaces](amazon-workspaces-vpc.md)\. The VPC must be connected to your on\-premises network through a virtual private network \(VPN\) connection or AWS Direct Connect\. For more information, see [AD Connector Prerequisites](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/prereq_connector.html) in the *AWS Directory Service Administration Guide*\.
+ Provide access to the internet from the WorkSpace\. For more information, see [Provide Internet Access from Your WorkSpace](amazon-workspaces-internet-access.md)\.

## Step 1: Create an AD Connector<a name="create-ad-connector"></a>

**To create an AD Connector**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Choose **Set up Directory**, **Create AD Connector**\.

1. For **Organization name**, enter a unique organization name for your directory \(for example, my\-example\-directory\)\. This name must be at least four characters in length, consist of only alphanumeric characters and hyphens \(\-\), and begin or end with a character other than a hyphen\.

1. For **Connected directory DNS**, enter the fully\-qualified name of your on\-premises directory \(for example, example\.com\)\.

1. For **Connected directory NetBIOS name**, enter the short name of your on\-premises directory \(for example, example\)\.

1. For **Connector account username**, enter the user name of a user in your on\-premises directory\. The user must have permissions to read users and groups, create computer objects, and join computers to the domain\.

1. For **Connector account password** and **Confirm password**, enter the password for the on\-premises user account\.

1. For **DNS address**, enter the IP address of at least one DNS server in your on\-premises directory\.
**Important**  
If you need to update your DNS server IP address after launching your WorkSpaces, follow the procedure in [Update DNS Servers for Amazon WorkSpaces](update-dns-server.md) to ensure that your WorkSpaces get properly updated\.

1. \(Optional\) For **Description**, enter a description for the directory\.

1. Keep **Size** as **Small**\.

1. For **VPC**, select your VPC\.

1. For **Subnets**, select your subnets\. The DNS servers that you specified must be accessible from each subnet\.

1. Choose **Next Step**\.

1. Choose **Create AD Connector**\. It takes several minutes for your directory to be connected\. The initial status of the directory is `Requested` and then `Creating`\. When directory creation is complete, the status is `Active`\.

## Step 2: Create a WorkSpace<a name="create-workspace-ad-connector"></a>

Now you are ready to launch WorkSpaces for one or more users in your on\-premises directory\.

**To launch a WorkSpace for an existing user**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Choose **Launch WorkSpaces**\.

1. For **Directory**, choose the directory that you created\.

1. \(Optional\) If this is the first time you have launched a WorkSpace in this directory, and Amazon WorkDocs is supported in the Region, you can enable or disable Amazon WorkDocs for all users in the directory\. For more information, see [Amazon WorkDocs Sync Client Help](https://docs.aws.amazon.com/workdocs/latest/userguide/sync_client_help.html) in the *Amazon WorkDocs Administration Guide*\.

1. Choose **Next**\. Amazon WorkSpaces registers your AD Connector\.

1. Select one or more existing users from your on\-premises directory\. Do not add new users to an on\-premises directory through the Amazon WorkSpaces console\.

   To find users to select, you can enter all or part of the user's name and choose **Search** or choose **Show All Users**\. Note that you cannot select a user that does not have an email address\.

   After you select the users, choose **Add Selected** and then choose **Next Step**\.

1. Under **Select Bundle**, choose the default WorkSpace bundle to be used for the WorkSpaces\. Under **Assign WorkSpace Bundles**, you can choose a different the bundle for an individual WorkSpace if needed\. When you have finished, choose **Next Step**\.

1. Choose a running mode for your WorkSpaces and then choose **Next Step**\. For more information, see [Manage the WorkSpace Running Mode](running-mode.md)\.

1. Choose **Launch WorkSpaces**\. The initial status of the WorkSpace is `PENDING`\. When the launch is complete, the status is `AVAILABLE`\.

1. Send invitations to the email address for each user\. \(These invitations aren't sent automatically if you're using AD Connector\.\) For more information, see [Send an Invitation Email](manage-workspaces-users.md#send-invitation)\.

## Step 3: Connect to the WorkSpace<a name="connect-workspace-ad-connector"></a>

You can connect to your WorkSpace using the client of your choice\. After you sign in, the client displays the WorkSpace desktop\.

**To connect to the WorkSpace**

1. Open the link in the invitation email\.

1. Review [Amazon WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide* for more information about the requirements for each client, and then do one of the following: 
   + When prompted, download one of the client applications or launch Web Access\.
   + If you aren't prompted and you haven't installed a client application already, open [https://clients\.amazonworkspaces\.com/](https://clients.amazonworkspaces.com/) and download one of the client applications or launch Web Access\.
**Note**  
You cannot use a web browser \(Web Access\) to connect to Amazon Linux WorkSpaces\.

1. Start the client, enter the registration code from the invitation email, and choose **Register**\.

1. When prompted to sign in, enter the user name and password for the user, and then choose **Sign In**\.

1. \(Optional\) When prompted to save your credentials, choose **Yes**\.

**Note**  
Because you're using AD Connector, your users won't be able to reset their own passwords\. \(The **Forgot password?** option on the WorkSpaces client application login screen won't be available\.\) For information about how to reset user passwords, see [Set Up Active Directory Administration Tools for Amazon WorkSpaces](directory_administration.md)\.

## Next Steps<a name="next-steps-ad-connector"></a>

You can continue to customize the WorkSpace that you just created\. For example, you can install software and then create a custom bundle from your WorkSpace\. If you are finished with your WorkSpace, you can delete it\. For more information, see the following documentation\.
+ [Create a Custom WorkSpaces Image and Bundle](create-custom-bundle.md)
+ [Administer Your WorkSpaces](administer-workspaces.md)
+ [Manage Directories for Amazon WorkSpaces](manage-workspaces-directory.md)
+ [Delete a WorkSpace](delete-workspaces.md)