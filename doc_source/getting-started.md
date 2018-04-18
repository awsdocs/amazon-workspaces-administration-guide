# Get Started with Amazon WorkSpaces Quick Setup<a name="getting-started"></a>

In this tutorial, you'll learn how to provision a virtual, cloud\-based Microsoft Windows desktop, known as a *WorkSpace*, using Amazon WorkSpaces and AWS Directory Service\.

This tutorial uses the Quick Setup option to launch your WorkSpace\. This option is available only if you have never launched a WorkSpace\.

**Note**  
Quick Setup is not supported in the EU \(Frankfurt\) region\.

Alternatively, see [Launch a Virtual Desktop Using Amazon WorkSpaces](launch-workspaces-tutorials.md)\.

**Topics**
+ [Before You Begin](#quick-setup-prereqs)
+ [Step 1: Launch the WorkSpace](#quick-setup-launch-workspace)
+ [Step 2: Connect to the WorkSpace](#quick-setup-connect-workspace)
+ [Step 3: Clean Up \(Optional\)](#quick-setup-clean-up)

## Before You Begin<a name="quick-setup-prereqs"></a>
+ You must have an AWS account to create or administer a WorkSpace\. Users do not need an AWS account to connect to and use their WorkSpaces\.
+ When you launch a WorkSpace, you must select a WorkSpace bundle\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.
+ When you launch a WorkSpace, you must specify profile information for the user, including a username and email address\. The user completes their profile by specifying a password\. Information about WorkSpaces and users is stored in a directory\.
+ Amazon WorkSpaces is not available in every region\. Verify the supported regions and select a region for your WorkSpaces\. For more information about the supported regions, see [Amazon WorkSpaces Pricing by AWS Region](https://aws.amazon.com/workspaces/pricing/#Amazon_WorkSpaces_Pricing_by_AWS_Region)\.

## Step 1: Launch the WorkSpace<a name="quick-setup-launch-workspace"></a>

Using Quick Setup, you can launch your first WorkSpace in minutes\.

**To launch a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. Choose **Get Started Now**\.

1. On the **Get Started with Amazon WorkSpaces** page, next to **Quick Setup**, choose **Launch**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-options.png)

1. For **Bundles**, select a bundle for the user\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-bundles.png)

1. For **Enter User Details**, complete **Username**, **First Name**, **Last Name**, and **Email**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-user-details.png)

1. Choose **Launch WorkSpaces**\.

1. On the confirmation page, choose **View the WorkSpaces Console**\. The initial status of the WorkSpace is `PENDING`\. When the launch is complete, the status is `AVAILABLE` and an invitation is sent to the email address that you specified for the user\.

**Quick Setup**

Quick Setup completes the following tasks on your behalf:
+ Creates an IAM role to allow the Amazon WorkSpaces service to create elastic network interfaces and list your Amazon WorkSpaces directories\. This role has the name `workspaces_DefaultRole`\.
+ Creates a virtual private cloud \(VPC\)\.
+ Sets up a Simple AD directory in the VPC that is used to store user and WorkSpace information\. The directory has an administrator account and it is enabled for Amazon WorkDocs\.
+ Creates the specified user accounts and adds them to the directory\.
+ Creates WorkSpace instances\. Each WorkSpace receives a public IP address to provide Internet access\. The running mode is AlwaysOn\. For more information, see [Manage the WorkSpace Running Mode](running-mode.md)\.
+ Sends invitation emails to the specified users\.

## Step 2: Connect to the WorkSpace<a name="quick-setup-connect-workspace"></a>

After you receive the invitation email, you can connect to the WorkSpace using the client of your choice\. After you sign in, the client displays the WorkSpace desktop\.

**To connect to the WorkSpace**

1. If you haven't set up credentials for the user already, open the link in the invitation email and follow the directions\. Remember the password that you specify as you will need it to connect to your WorkSpace\.

   Note that passwords are case\-sensitive and must be between 8 and 64 characters in length, inclusive\. Passwords must contain at least one character from three of the following categories: lowercase letters \(a\-z\), uppercase letters \(A\-Z\), numbers \(0\-9\), and the set \~\!@\#$%^&\*\_\-\+=`\|\\\(\)\{\}\[\]:;"'<>,\.?/\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-user-profile.png)

1. When prompted, download one of the client applications or launch Web Access\. For more information about the requirements for each client, see [Amazon WorkSpaces Clients](http://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide*\.

   If you aren't prompted and you haven't installed a client application already, open [https://clients\.amazonworkspaces\.com](https://clients.amazonworkspaces.com/) and follow the directions\.

1. Start the client, enter the registration code from the invitation email, and choose **Register**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-register.png)

1. When prompted to sign in, type the username and password, and then choose **Sign In**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-sign-in.png)

1. \(Optional\) When prompted to save your credentials, choose **Yes**\.

## Step 3: Clean Up \(Optional\)<a name="quick-setup-clean-up"></a>

If you are finished with the WorkSpace that you created for this tutorial, you can delete it\.

**To delete the WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select your WorkSpace and choose **Actions**, **Remove WorkSpaces**\.

1. When prompted for confirmation, choose **Remove WorkSpaces**\.

1. \(Optional\) If you are not using the directory with another application, such as Amazon WorkDocs, Amazon WorkMail, or Amazon Chime, you can delete it as follows:

   1. In the navigation pane, choose **Directories**\.

   1. Select your directory and choose **Actions**, **Deregister**\.

   1. Select your directory again and choose **Actions**, **Delete**\.

   1. When prompted for confirmation, choose **Delete**\.