# Launch a WorkSpace using a trusted domain<a name="launch-workspace-trusted-domain"></a>

WorkSpaces enables you to provision virtual, cloud\-based Microsoft Windows desktops for your users, known as *WorkSpaces*\.

WorkSpaces uses directories to store and manage information for your WorkSpaces and users\. For your directory, you can choose from Simple AD, AD Connector, or AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD\. In addition, you can establish a trust relationship between your AWS Managed Microsoft AD directory and your on\-premises domain\.

In this tutorial, we launch a WorkSpace that uses a trust relationship\. For tutorials that use the other options, see [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\.

**Topics**
+ [Before you begin](#prereqs-trusted-domain)
+ [Step 1: Establish a trust relationship](#create-trust-relationship)
+ [Step 2: Create a WorkSpace](#create-workspace-trusted-domain)
+ [Step 3: Connect to the WorkSpace](#connect-workspace-trusted-domain)
+ [Next steps](#next-steps-trusted-domain)

## Before you begin<a name="prereqs-trusted-domain"></a>
+ Launching WorkSpaces with user accounts in a separate trusted domain works with AWS Managed Microsoft AD when it is configured with a trust relationship to your on\-premises directory\. However, WorkSpaces using Simple AD or AD Connector cannot launch WorkSpaces for users from a trusted domain\.
+ WorkSpaces is not available in every Region\. Verify the supported Regions and select a Region for your WorkSpaces\. For more information about the supported Regions, see [WorkSpaces Pricing by AWS Region](https://aws.amazon.com/workspaces/pricing/)\.
+ When you launch a WorkSpace, you must select a WorkSpace bundle\. A bundle is a combination of storage, compute, and software resources\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.
+ When you create a directory using AWS Directory Service or launch a WorkSpace, you must create or select a virtual private cloud configured with a public subnet and two private subnets\. For more information, see [Configure a VPC for WorkSpaces](amazon-workspaces-vpc.md)\.

## Step 1: Establish a trust relationship<a name="create-trust-relationship"></a>

**To set up the trust relationship**

1. Set up AWS Managed Microsoft AD in your virtual private cloud \(VPC\)\. For more information, see [Create Your AWS Managed Microsoft AD directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/create_managed_ad.html) in the *AWS Directory Service Administration Guide*\.
**Note**  
Shared directories are not currently supported for use with Amazon WorkSpaces\.
If your AWS Managed Microsoft AD directory has been configured for multi\-Region replication, only the directory in the primary Region can be registered for use with Amazon WorkSpaces\. Attempts to register the directory in a replicated Region for use with Amazon WorkSpaces will fail\. Multi\-Region replication with AWS Managed Microsoft AD isn't supported for use with Amazon WorkSpaces within replicated Regions\.

1. Create a trust relationship between your AWS Managed Microsoft AD and your on\-premises domain\. Ensure that the trust is configured as a two\-way trust\. For more information, see [Tutorial: Create a Trust Relationship Between Your AWS Managed Microsoft AD and Your On\-Premises Domain](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/tutorial_setup_trust.html) in the *AWS Directory Service Administration Guide*\.

A one\-way or two\-way trust can be used to manage and authenticate with WorkSpaces, and so that WorkSpaces can be provisioned to on\-premises users and groups\. For more information, see [Deploy Amazon WorkSpaces using a One\-Way Trust Resource Domain with AWS Directory Service](http://aws.amazon.com/getting-started/hands-on/deploy-workspaces-one-way-trust/)\.

## Step 2: Create a WorkSpace<a name="create-workspace-trusted-domain"></a>

After you establish a trust relationship between your AWS Managed Microsoft AD and your on\-premises Microsoft Active Directory domain, you can provision WorkSpaces for users in the on\-premises domain\.

Note that you must ensure that GPO settings are replicated across domains before you can apply them to WorkSpaces\.

**To launch workspaces for users in a trusted on\-premises domain**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Choose **Launch WorkSpaces**\.

1. On the **Select a Directory** page, choose the directory that you just registered and then choose **Next Step**\.

1. On the **Identify Users** page, do the following:

   1. For **Select trust from forest**, select the trust relationship that you created\.

   1. Select the users from the on\-premises domain and then choose **Add Selected**\.

   1. Choose **Next Step**\.

1. Select the bundle to be used for the WorkSpaces and then choose **Next Step**\.

1. Choose the running mode, choose the encryption settings, and configure any tags\. When you are finished, choose **Next Step**\.

1. Choose **Launch WorkSpaces**\. Note that it can take up to 20 minutes for the WorkSpaces to become available, and up to 40 minutes if encryption is enabled\. The initial status of the WorkSpace is `PENDING`\. When the launch is complete, the status is `AVAILABLE`\.

1. Send invitations to the email address for each user\. \(These invitations aren't sent automatically if you're using a trust relationship\.\) For more information, see [Send an invitation email](manage-workspaces-users.md#send-invitation)\.

## Step 3: Connect to the WorkSpace<a name="connect-workspace-trusted-domain"></a>

After you receive the invitation email, you can connect to your WorkSpace\. Users can enter their user names as *username*, *corp*\\*username*, or *corp\.example\.com*\\*username*\)\.

**To connect to the WorkSpace**

1. Open the link in the invitation email\. When prompted, enter a password and activate the user\. Remember this password as you will need it to sign in to your WorkSpace\.
**Note**  
Passwords are case\-sensitive and must be between 8 and 64 characters in length, inclusive\. Passwords must contain at least one character from each of the following categories: lowercase letters \(a\-z\), uppercase letters \(A\-Z\), numbers \(0\-9\), and \~\!@\#$%^&\*\_\-\+=`\|\\\(\)\{\}\[\]:;"'<>,\.?/\.

1. Review [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide* for more information about the requirements for each client, and then do one of the following: 
   + When prompted, download one of the client applications or launch Web Access\.
   + If you aren't prompted and you haven't installed a client application already, open [https://clients\.amazonworkspaces\.com/](https://clients.amazonworkspaces.com/) and download one of the client applications or launch Web Access\.
**Note**  
You cannot use a web browser \(Web Access\) to connect to Amazon Linux WorkSpaces\.

1. Start the client, enter the registration code from the invitation email, and choose **Register**\.

1. When prompted to sign in, enter the user name and password for the user, and then choose **Sign In**\.

1. \(Optional\) When prompted to save your credentials, choose **Yes**\.

## Next steps<a name="next-steps-trusted-domain"></a>

You can continue to customize the WorkSpace that you just created\. For example, you can install software and then create a custom bundle from your WorkSpace\. You can also perform various administrative tasks for your WorkSpaces and your WorkSpaces directory\. If you are finished with your WorkSpace, you can delete it\. For more information, see the following documentation\.
+ [Create a custom WorkSpaces image and bundle](create-custom-bundle.md)
+ [Administer your WorkSpaces](administer-workspaces.md)
+ [Manage directories for WorkSpaces](manage-workspaces-directory.md)
+ [Delete a WorkSpace](delete-workspaces.md)

For more information about using the WorkSpaces client applications, such as setting up multiple monitors or using peripheral devices, see [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) and [Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.