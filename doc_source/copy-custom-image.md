# Copy a Custom WorkSpaces Image<a name="copy-custom-image"></a>

You can copy a custom WorkSpaces image within or across AWS Regions\. Copying an image results in the creation of an identical image with its own unique identifier\.

You can copy a Bring Your Own License \(BYOL\) image to another Region as long as the destination Region is enabled for BYOL\. 

There are no additional charges for copying an image across Regions\. However, the quota for the number of images in the destination Region applies\.

You can copy images one by one using the console\. To bulk copy images, use the CopyWorkspaceImage API operation or the copy\-workspace\-image command in the AWS command line interface \(CLI\)\. For more information, see [ CopyWorkspaceImage](https://docs.aws.amazon.com/workspaces/latest/api/API_CopyWorkspaceImage.html) in the *Amazon WorkSpaces API Reference* or see [ copy\-workspace\-image](https://docs.aws.amazon.com/cli/latest/reference/workspaces/copy-workspace-image.html) in the *AWS CLI Command Reference*\.

**To copy an image**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Select the image and choose **Actions**, **Copy Image**\.

1. Provide a name, description and Region for the copied image, and then choose **Copy Image**\.