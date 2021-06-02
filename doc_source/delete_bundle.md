# Delete a Custom WorkSpaces Bundle or Image<a name="delete_bundle"></a>

You can delete unused custom bundles or custom images as needed\.

## Deleting Bundles<a name="delete_bundle_console"></a>

To delete a bundle, you must first delete all of the WorkSpaces that are based on the bundle\.

**To delete a bundle using the console**

1. Open the Workspaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Bundles**\.

1. Select the bundle and choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.

**To delete a bundle programmatically**  
To delete a bundle programmatically, use the DeleteWorkspaceBundle API action\. For more information, see [ DeleteWorkspaceBundle](https://docs.aws.amazon.com/workspaces/latest/api/API_DeleteWorkspaceBundle.html) in the *Amazon Workspaces API Reference*\.

## Deleting Images<a name="delete_images"></a>

After you delete a custom bundle, you can delete the image that you used to create or update the bundle\.

**Note**  
To delete an image, you must first either delete any bundles that are associated with the image, or you must update those bundles to use another source image\. You must also unshare the image if it is shared with other accounts\. The image also can't be in the **Pending** or **Validating** state\.

**To delete an image using the console**

1. Open the Workspaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Select the image and choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.

**To delete an image programmatically**  
To delete an image programmatically, use the DeleteWorkspaceImage API action\. For more information, see [ DeleteWorkspaceImage](https://docs.aws.amazon.com/workspaces/latest/api/API_DeleteWorkspaceImage.html) in the *Amazon Workspaces API Reference*\.