# Delete a Custom WorkSpaces Bundle or Image<a name="delete_bundle"></a>

You can delete unused custom bundles as needed\. If you delete a bundle that is being used by a WorkSpace, the bundle is placed in a delete queue and will be deleted after all WorkSpaces that are based on the bundle have been deleted\.

**To delete a bundle using the console**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Bundles**\.

1. Select the bundle and choose **Actions**, **Delete Bundle**\.

1. When prompted for confirmation, choose **Delete Bundle**\.

**To delete a bundle programmatically**  
To delete a bundle programmatically, use the DeleteWorkspaceBundle API action\. For more information, see [ DeleteWorkspaceBundle](https://docs.aws.amazon.com/workspaces/latest/api/API_DeleteWorkspaceBundle.html) in the *Amazon WorkSpaces API Reference*\.

After you delete a custom bundle, you can delete the image that you used to create or update the bundle\.

**Note**  
To delete an image, you must first delete any bundles that are associated with the image and unshare the image if it is shared with other accounts\. The image also can't be in the `PENDING` or `VALIDATING` state\.

**To delete an image using the console**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Select the image and choose **Actions**, **Delete Image**\.

1. When prompted for confirmation, choose **Delete Image**\.

**To delete an image programmatically**  
To delete an image programmatically, use the DeleteWorkspaceImage API action\. For more information, see [ DeleteWorkspaceImage](https://docs.aws.amazon.com/workspaces/latest/api/API_DeleteWorkspaceImage.html) in the *Amazon WorkSpaces API Reference*\.