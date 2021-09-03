# Get started with WorkSpaces Quick Setup<a name="getting-started"></a>

In this tutorial, you learn how to provision a virtual, cloud\-based Microsoft Windows or Amazon Linux desktop, known as a *WorkSpace*, by using WorkSpaces and AWS Directory Service\.

This tutorial uses the Quick Setup option to launch your WorkSpace\. This option is available only if you have never launched a WorkSpace\. Alternatively, see [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\.

**Note**  
Quick Setup is supported in the following AWS Regions:   
US East \(N\. Virginia\)
US West \(Oregon\)
Europe \(Ireland\)
Asia Pacific \(Singapore\)
Asia Pacific \(Sydney\)
Asia Pacific \(Tokyo\)
To change your Region, see [ Choosing a Region](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region)\.

**Topics**
+ [Before you begin](#quick-setup-prereqs)
+ [What Quick Setup does](#quick-setup-what-it-does)
+ [Step 1: Launch the WorkSpace](#quick-setup-launch-workspace)
+ [Step 2: Connect to the WorkSpace](#quick-setup-connect-workspace)
+ [Step 3: Clean up \(Optional\)](#quick-setup-clean-up)
+ [Next steps](#quick-setup-next-steps)

## Before you begin<a name="quick-setup-prereqs"></a>

Before you begin, make sure that you meet the following requirements:
+ You must have an AWS account to create or administer a WorkSpace\. Users do not need an AWS account to connect to and use their WorkSpaces\.
+ WorkSpaces is not available in every Region\. Verify the supported Regions and [ select a Region](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region) for your WorkSpaces\. For more information about the supported Regions, see [WorkSpaces Pricing by AWS Region](https://aws.amazon.com/workspaces/pricing/#Amazon_WorkSpaces_Pricing_by_AWS_Region)\.

It's also helpful to review and understand the following concepts before you proceed:
+ When you launch a WorkSpace, you must select a WorkSpace bundle\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.
+ When you launch a WorkSpace, you must select which protocol \(PCoIP or WorkSpaces Streaming Protocol \[WSP\]\) you want to use with your bundle\. For more information, see [Protocols for Amazon WorkSpaces](amazon-workspaces-protocols.md)\.
+ When you launch a WorkSpace, you must specify profile information for the user, including a user name and email address\. Users complete their profiles by specifying a password\. Information about WorkSpaces and users is stored in a directory\. For more information, see [Manage directories for WorkSpaces](manage-workspaces-directory.md)\.

## What Quick Setup does<a name="quick-setup-what-it-does"></a>

Quick Setup completes the following tasks on your behalf:
+ **Creates an IAM role** to allow the WorkSpaces service to create elastic network interfaces and list your WorkSpaces directories\. This role has the name `workspaces_DefaultRole`\.
+ **Creates a virtual private cloud \(VPC\)**\. If you want to use an existing VPC instead, make sure it meets the requirements noted in [Configure a VPC for WorkSpaces](amazon-workspaces-vpc.md), and then follow the steps in one of the tutorials listed in [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\. Choose the tutorial that corresponds to the type of Active Directory that you want to use\.
+ **Sets up a Simple AD directory** in the VPC\. This Simple AD directory is used to store user and WorkSpace information\. The directory has an administrator account and it is enabled for Amazon WorkDocs\.
+ **Creates the specified user accounts and adds them to the directory**\.
+ **Creates WorkSpaces**\. Each WorkSpace receives a public IP address to provide internet access\. The running mode is AlwaysOn\. For more information, see [Manage the WorkSpace running mode](running-mode.md)\.
+ **Sends invitation emails to the specified users**\. If your users don't receive their invitation emails, see [Send an invitation email](manage-workspaces-users.md#send-invitation)\. 

**Note**  
The first user account created by Quick Setup is your Admin user account\. You can't update this user account from the WorkSpaces Console\. Don't share the information for this Admin account with anyone else\. If you want to invite other users to use WorkSpaces, create new user accounts for them\.

## Step 1: Launch the WorkSpace<a name="quick-setup-launch-workspace"></a>

Using Quick Setup, you can launch your first WorkSpace in minutes\.

**To launch a WorkSpace**

1. Open the WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. Choose **Get Started Now**\. If you don't see this button, either you have already launched a WorkSpace in this Region, or you aren't using one of the [Regions that support Quick Setup](#quick-setup-regions)\. In this case, see [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\. 

1. On the **Get Started with WorkSpaces** page, next to **Quick Setup**, choose **Launch**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-options.png)

1. For **Bundles**, select a bundle \(hardware and software\) for the user with the appropriate protocol \(PCoIP or WSP\)\. For more information about the various public bundles available for Amazon WorkSpaces, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/bundles-linux2-windows.png)

1. For **Enter User Details**, complete **Username**, **First Name**, **Last Name**, and **Email**\.
**Note**  
If this is your first time using WorkSpaces, we recommend creating a user for yourself for testing purposes\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/get-started-user-details2.png)

1. Choose **Launch WorkSpaces**\.

1. On the confirmation page, choose **View the WorkSpaces Console**\. It takes approximately 20 minutes for your WorkSpace to be launched\. To monitor the progress, go to the left navigation pane and choose **Directories**\. You will see a directory being created with an initial status of `REQUESTED` and then `CREATING`\. 

   After the directory has been created and has a status of `ACTIVE`, you can choose **WorkSpaces** in the left navigation pane to monitor the progress of the WorkSpace launch process\. The initial status of the WorkSpace is `PENDING`\. When the launch is complete, the status is `AVAILABLE` and an invitation is sent to the email address that you specified for each user\. If your users don't receive their invitation emails, see [Send an invitation email](manage-workspaces-users.md#send-invitation)\.

## Step 2: Connect to the WorkSpace<a name="quick-setup-connect-workspace"></a>

After you receive the invitation email, you can connect to the WorkSpace using the client of your choice\. After you sign in, the client displays the WorkSpace desktop\.

**To connect to the WorkSpace**

1. If you haven't set up credentials for the user already, open the link in the invitation email and follow the directions\. Remember the password that you specify as you will need it to connect to your WorkSpace\.
**Note**  
Passwords are case\-sensitive and must be between 8 and 64 characters in length, inclusive\. Passwords must contain at least one character from each of the following categories: lowercase letters \(a\-z\), uppercase letters \(A\-Z\), numbers \(0\-9\), and the set \~\!@\#$%^&\*\_\-\+=`\|\\\(\)\{\}\[\]:;"'<>,\.?/\.

1. Review [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide* for more information about the requirements for each client, and then do one of the following: 
   + When prompted, download one of the client applications or launch **Web Access**\.
   + If you aren't prompted and you haven't installed a client application already, open [https://clients\.amazonworkspaces\.com/](https://clients.amazonworkspaces.com/) and download one of the client applications or launch **Web Access**\.
**Note**  
You cannot use a web browser \(Web Access\) to connect to Amazon Linux WorkSpaces\.

1. Start the client, enter the registration code from the invitation email, and choose **Register**\.

1. When prompted to sign in, enter the user name and password, and then choose **Sign In**\.

1. \(Optional\) When prompted to save your credentials, choose **Yes**\.

For more information about using the client applications, such as setting up multiple monitors or using peripheral devices, see [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) and [Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.

## Step 3: Clean up \(Optional\)<a name="quick-setup-clean-up"></a>

If you are finished with the WorkSpace that you created for this tutorial, you can delete it\. For more information, see [Delete a WorkSpace](delete-workspaces.md)\.

**Note**  
Simple AD is made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon WorkSpaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
To delete empty directories, see [Delete the directory for your WorkSpaces](delete-workspaces-directory.md)\. If you delete your Simple AD directory, you can always create a new one when you want to start using WorkSpaces again\.

## Next steps<a name="quick-setup-next-steps"></a>

You can continue to customize the WorkSpace that you just created\. For example, you can install software and then create a custom bundle from your WorkSpace\. You can also perform various administrative tasks for your WorkSpaces and your WorkSpaces directory\. For more information, see the following documentation\.
+ [Create a custom WorkSpaces image and bundle](create-custom-bundle.md)
+ [Administer your WorkSpaces](administer-workspaces.md)
+ [Manage directories for WorkSpaces](manage-workspaces-directory.md)

To create additional WorkSpaces, do one of the following:
+ If you want to continue using the VPC and the Simple AD directory that were created by Quick Setup, you can add WorkSpaces for additional users by following the steps in the [Step 2: Create a WorkSpace](launch-workspace-simple-ad.md#create-workspace-simple-ad) section of the Launch a WorkSpace Using Simple AD tutorial\.
+ If you need to use another directory type or if you need to use an existing Active Directory, see the appropriate tutorial in [Launch a virtual desktop using WorkSpaces](launch-workspaces-tutorials.md)\.

For more information about using the WorkSpaces client applications, such as setting up multiple monitors or using peripheral devices, see [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) and [Peripheral Device Support](https://docs.aws.amazon.com/workspaces/latest/userguide/peripheral_devices.html) in the *Amazon WorkSpaces User Guide*\.