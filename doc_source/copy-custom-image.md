# Copy a Custom WorkSpaces Image<a name="copy-custom-image"></a>

You can copy a custom WorkSpaces image within or across AWS Regions\. Copying an image results in the creation of an identical image with its own unique identifier\.

You can copy a Bring Your Own License \(BYOL\) image to another Region as long as the destination Region is enabled for BYOL\. 

**Note**  
In the China \(Ningxia\) Region, you can copy images only within the same Region\.  
In the AWS GovCloud \(US\-West\) Region, to copy images to and from other AWS Regions, contact AWS Support\.

You can also copy an image that has been shared with you by another AWS account\. For more information about shared images, see [Share or Unshare a Custom WorkSpaces Image](share-custom-image.md)\.

There are no additional charges for copying an image within or across Regions\. However, the quota for the number of images in the destination Region applies\. For more information about Amazon WorkSpaces quotas, see [Amazon WorkSpaces Quotas](workspaces-limits.md)\.

**IAM Permissions for Copying an Image**  
If you use an IAM user to copy an image, the user must have permissions for `workspaces:DescribeWorkspaceImages` and `workspaces:CopyWorkspaceImage`\.

The following example policy allows the user to copy the specified image to the specified account in the specified Region\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "workspaces:DescribeWorkspaceImages",
        "workspaces:CopyWorkspaceImage"
      ],
      "Resource": [
          "arn:aws:workspaces:us-east-1:123456789012:workspaceimage/wsi-a1bcd2efg"
      ]
    }
  ]
}
```

For more information about working with IAM, see [Identity and Access Management for Amazon WorkSpaces](workspaces-access-control.md)\.

**Copy an Image**  
You can copy images one by one using the console\. To bulk copy images, use the CopyWorkspaceImage API operation or the copy\-workspace\-image command in the AWS command line interface \(CLI\)\. For more information, see [ CopyWorkspaceImage](https://docs.aws.amazon.com/workspaces/latest/api/API_CopyWorkspaceImage.html) in the *Amazon WorkSpaces API Reference* or see [ copy\-workspace\-image](https://docs.aws.amazon.com/cli/latest/reference/workspaces/copy-workspace-image.html) in the *AWS CLI Command Reference*\.

**Important**  
Before copying a shared image, be sure to verify that it has been shared from the correct AWS account\. To determine if an image has been shared and to see the AWS account ID that owns an image, use the [DescribeWorkSpaceImages](https://docs.aws.amazon.com/workspaces/latest/api/API_DescribeWorkspaceImages.html) and [DescribeWorkspaceImagePermissions](https://docs.aws.amazon.com/workspaces/latest/api/API_DescribeWorkspaceImagePermissions.html) API operations or the [describe\-workspace\-images](https://docs.aws.amazon.com/cli/latest/reference/workspaces/describe-workspace-images.html) and [describe\-workspace\-image\-permissions](https://docs.aws.amazon.com/cli/latest/reference/workspaces/describe-workspace-image-permissions.html) commands in the AWS CLI\. 

**To copy an image using the console**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Select the image and choose **Actions**, **Copy Image**\.

1. Provide a name, description and Region for the copied image, and then choose **Copy Image**\.