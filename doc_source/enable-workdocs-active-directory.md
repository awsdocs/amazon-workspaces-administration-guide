# Enable Amazon WorkDocs for Microsoft Active Directory<a name="enable-workdocs-active-directory"></a>

If you're using Microsoft Active Directory \(AD\) with Amazon WorkSpaces, you can enable Amazon WorkDocs for your directory through either the Amazon WorkDocs console or the AWS Directory Service console\. 

**To enable WorkDocs through the Amazon WorkDocs console**

1. Open the Amazon WorkDocs console at [https://console\.aws\.amazon\.com/zocalo/](https://console.aws.amazon.com/zocalo/)\.

1. Choose **Create a New WorkDocs Site**\.

1. Under **Standard Setup**, choose **Launch**\.

1. Select the directory and create your site name\.

1. Specify the user who will administer the WorkDocs site\. You can use the admin or any user created in the directory\.

For more information, see [ Getting Started with AWS Managed Microsoft AD](https://docs.aws.amazon.com/workdocs/latest/adminguide/connect_directory_microsoft.html) in the *Amazon WorkDocs Administration Guide*\.

**To enable WorkDocs through the AWS Directory Service console**

1. Open the AWS Directory Service console at [https://console\.aws\.amazon\.com/directoryservicev2/](https://console.aws.amazon.com/directoryservicev2/)\.

1. In the navigation pane, choose **Directories**\.

1. On the **Directories** page, choose your directory\.

1. On the **Directory details** page, choose the **Application management** tab\.

1. In the **Application access URL** section, if an access URL has not been assigned to the directory, the **Create** button is displayed\. Enter a directory alias and choose **Create**\. For more information, see [ Creating an Access URL](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_create_access_url.html) in the *AWS Directory Service Administration Guide*\.

1. In the **Application access URL** section, choose **Enable** to enable single sign\-on for Amazon WorkDocs\. For more information, see [ Single Sign\-On](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_single_sign_on.html) in the *AWS Directory Service Administration Guide*\.