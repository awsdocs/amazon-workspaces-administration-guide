# Register a Directory with Amazon WorkSpaces<a name="register-deregister-directory"></a>

To allow Amazon WorkSpaces to use an existing AWS Directory Service directory, you must register it with Amazon WorkSpaces\. After you register a directory, you can launch WorkSpaces in the directory\.

**Requirements**  
To register a directory for use with Amazon WorkSpaces, it must meet the following requirements:
+ The directory that you want to register for use with Amazon WorkSpaces must be present in every virtual private cloud \(VPC\) subnet where you want to launch WorkSpaces\.
+ If you're using AD Connector, your AD Connector must be directly attached to the VPC subnets that will be used for WorkSpaces deployments\.
+ If you're using AWS Managed Microsoft AD or Simple AD, your directory can be in a dedicated private subnet, as long as the directory has access to the VPC where the WorkSpaces are located\.

For more information about directory and VPC design, see the [ *Best Practices for Deploying Amazon WorkSpaces*](https://d1.awsstatic.com/whitepapers/Best-Practices-for-Deploying-Amazon-WorkSpaces.pdf) whitepaper\.

**Note**  
Simple AD and AD Connector are made available to you free of charge to use with WorkSpaces\. If there are no WorkSpaces being used with your Simple AD or AD Connector directory for 30 consecutive days, this directory will be automatically deregistered for use with Amazon WorkSpaces, and you will be charged for this directory as per the [AWS Directory Service pricing terms](http://aws.amazon.com/directoryservice/pricing/)\.  
To delete empty directories, see [Delete the Directory for Your WorkSpaces](delete-workspaces-directory.md)\. If you delete your Simple AD or AD Connector directory, you can always create a new one when you want to start using WorkSpaces again\.

**To register a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory\.

1. Choose **Actions**, **Register**\.
**Note**  
Shared directories are not currently supported for use with Amazon WorkSpaces\.
If your AWS Managed Microsoft AD directory has been configured for multi\-Region replication, only the directory in the primary Region can be registered for use with Amazon WorkSpaces\. Attempts to register the directory in a replicated Region for use with Amazon WorkSpaces will fail\. Multi\-Region replication with AWS Managed Microsoft AD isn't supported for use with Amazon WorkSpaces within replicated Regions\.

1. Select two subnets that are not from the same Availability Zone\.

1. For **Enable Self Service Permissions**, choose **Yes** to enable your users to rebuild their WorkSpaces, change volume size, compute type and running mode\. Enabling may impact how much you pay for Amazon WorkSpaces\. Choose **No** otherwise\.

1. For **Enable Amazon WorkDocs**, choose **Yes** to register the directory for use with Amazon WorkDocs or **No** otherwise\.
**Note**  
This option is displayed only if Amazon WorkDocs is available in the Region and if you're not using AWS Managed Microsoft AD\. If you're using AWS Managed Microsoft AD, finish registering your directory, and then see [Enable Amazon WorkDocs for AWS Managed Microsoft AD](enable-workdocs-active-directory.md)\.

1. Choose **Register**\. Initially the value of **Registered** is `REGISTERING`\. After registration is complete, the value is `Yes`\.

When you are finished using the directory with Amazon WorkSpaces, you can deregister it\. Note that you must deregister a directory before you can delete it\. If you want to deregister and delete a directory, you must first find and remove all the applications and services that are registered to the directory\. For more information, see [Delete Your Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_delete.html) in the *AWS Directory Service Administration Guide*\. 

**To deregister a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory\.

1. Choose **Actions**, **Deregister**\.

1. When prompted for confirmation, choose **Deregister**\. After deregistration is complete, the value of **Registered** is `No`\.