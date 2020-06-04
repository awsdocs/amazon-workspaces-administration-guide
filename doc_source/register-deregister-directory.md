# Register a Directory with Amazon WorkSpaces<a name="register-deregister-directory"></a>

To allow Amazon WorkSpaces to use an existing AWS Directory Service directory, you must register it with Amazon WorkSpaces\. After you register a directory, you can launch WorkSpaces in the directory\.

**To register a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory\.

1. Choose **Actions**, **Register**\.

1. Select two subnets that are not from the same Availability Zone\.

1. For **Enable Self Service Permissions**, choose **Yes** to enable your users to rebuild their WorkSpaces, change volume size, compute type and running mode\. Enabling may impact how much you pay for Amazon WorkSpaces\. Choose **No** otherwise\.

1. For **Enable Amazon WorkDocs**, choose **Yes** to register the directory for use with Amazon WorkDocs or **No** otherwise\.
**Note**  
This option is displayed only if Amazon WorkDocs is available in the Region and if you're not using Microsoft Active Directory \(AD\)\. If you're using Microsoft AD, finish registering your directory, and then see [Enable Amazon WorkDocs for Microsoft Active Directory](enable-workdocs-active-directory.md)\.

1. Choose **Register**\. Initially the value of **Registered** is `REGISTERING`\. After registration is complete, the value is `Yes`\.

When you are finished using the directory with Amazon WorkSpaces, you can deregister it\. Note that you must deregister a directory before you can delete it\. If you want to deregister and delete a directory, you must first find and remove all the applications and services that are registered to the directory\. For more information, see [Delete Your Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_delete.html) in the *AWS Directory Service Administration Guide*\. 

**To deregister a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory\.

1. Choose **Actions**, **Deregister**\.

1. When prompted for confirmation, choose **Deregister**\. After deregistration is complete, the value of **Registered** is `No`\.