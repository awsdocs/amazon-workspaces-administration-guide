# Share or Unshare a Custom WorkSpaces Image<a name="share-custom-image"></a>

You can share custom WorkSpaces images across AWS accounts within the same AWS Region\. After an image has been shared, the recipient account can copy the image to other AWS Regions as needed\. For more information about copying images, see [Copy a Custom WorkSpaces Image](copy-custom-image.md)\.

**Note**  
In the China \(Ningxia\) Region, you can copy images only within the same Region\.  
In the AWS GovCloud \(US\-West\) Region, to copy images to and from other AWS Regions, contact AWS Support\.

There are no additional charges for sharing an image\. However, the quota for the number of images in the AWS Region applies\. A shared image doesn't count against the recipient account's quota until the recipient copies the image\. For more information about Amazon WorkSpaces quotas, see [Amazon WorkSpaces Quotas](workspaces-limits.md)\.

To delete a shared image, you must unshare the image before you can delete it\.

**Sharing Bring Your Own License Images**  
You can share Bring Your Own License \(BYOL\) images only with AWS accounts that are enabled for BYOL\. The AWS account that you want to share BYOL images with must also be part of your organization \(under the same payer account\)\.

**Note**  
Sharing BYOL images across AWS accounts isn't supported at this time in the AWS GovCloud \(US\-West\) Region\. To share BYOL images across accounts in the AWS GovCloud \(US\-West\) Region, contact AWS Support\. 

**Images Shared with You**  
If images are shared with you, you can copy them\. You can then use your copies of the shared images to create bundles for launching new WorkSpaces\.

**Important**  
Before copying a shared image, be sure to verify that it has been shared from the correct AWS account\. To programmatically determine if an image has been shared, use the [DescribeWorkSpaceImages](https://docs.aws.amazon.com/workspaces/latest/api/API_DescribeWorkspaceImages.html) and [DescribeWorkspaceImagePermissions](https://docs.aws.amazon.com/workspaces/latest/api/API_DescribeWorkspaceImagePermissions.html) API operations or the [describe\-workspace\-images](https://docs.aws.amazon.com/cli/latest/reference/workspaces/describe-workspace-images.html) and [describe\-workspace\-image\-permissions](https://docs.aws.amazon.com/cli/latest/reference/workspaces/describe-workspace-image-permissions.html) commands in the AWS command line interface \(CLI\)\. 

The creation date shown for an image that has been shared with you is the date that the image was originally created, not the date that the image was shared with you\.

If an image has been shared with you, you can't further share that image with other accounts\.

**To share an image**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Select the image and choose **Actions**, **View details**\.

1. On the image detail page, in the **Shared accounts** section, choose **Add account**\.

1. On the **Add account** page, under **Add account to share with**, enter the account ID of the account that you want to share the image with\.
**Important**  
Before sharing the image, confirm that you are sharing to the correct AWS account ID\.

1. Choose **Share image**\.

**To stop sharing an image**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Select the image and choose **Actions**, **View details**\.

1. On the image detail page, in the **Shared accounts** section, select the AWS account that you want to stop sharing with, and then choose **Unshare**\.

1. When prompted to confirm unsharing the image, choose **Unshare**\.
**Note**  
If you want to delete the image after unsharing it, you must first unshare it from all of the accounts that it has been shared with\.

After you stop sharing an image, the recipient account can no longer make copies of the image\. However, any copies of shared images that are already in the recipient account remain in that account, and new WorkSpaces can be launched from those copies\.

**To share or unshare images programmatically**  
To share or unshare images programmatically, use the [UpdateWorkspaceImagePermission](https://docs.aws.amazon.com/workspaces/latest/api/API_UpdateWorkspaceImagePermission.html) API operation or the [update\-workspace\-image\-permission](https://docs.aws.amazon.com/cli/latest/reference/workspaces/update-workspace-image-permission.html) AWS command line interface \(CLI\) command\. To determine if an image has been shared, use the [DescribeWorkspaceImagePermissions](https://docs.aws.amazon.com/workspaces/latest/api/API_DescribeWorkspaceImagePermissions.html) API operation or the [describe\-workspace\-image\-permissions](https://docs.aws.amazon.com/cli/latest/reference/workspaces/describe-workspace-image-permissions.html) CLI command\. 