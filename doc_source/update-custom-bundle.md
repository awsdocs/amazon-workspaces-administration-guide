# Update a Custom WorkSpaces Bundle<a name="update-custom-bundle"></a>

You can update an existing custom WorkSpaces bundle by modifying a WorkSpace based on the bundle, creating an image from the WorkSpace, and updating the bundle with the new image\. You can launch new WorkSpaces using the updated bundle\. To update existing WorkSpaces that are based on the bundle, rebuild the WorkSpace\.

**To update a bundle**

1. Connect to a WorkSpace that is based on the bundle and make any changes\. For example, you can apply the latest operating system and application patches and install additional applications\.

   Alternatively, you can create a WorkSpace with the same base software package \(Plus or Standard\) as the image used to create the bundle and make changes\.

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Create Image**\.

1. Type an image name and a description, and then choose **Create Image**\. The WorkSpace is unavailable while the image is being created\.

1. In the navigation pane, choose **Bundles**\.

1. Select the bundle and choose **Actions**, **Update Bundle**\.

1. For **Update WorkSpace Bundle**, select the image that you created and choose **Update Bundle**\.

1. \(Optional\) Rebuild the existing WorkSpaces based on the bundle\. For more information, see [Rebuild a WorkSpace](reset-workspace.md)\.