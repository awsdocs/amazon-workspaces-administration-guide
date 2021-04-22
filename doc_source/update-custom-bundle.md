# Update a Custom WorkSpaces Bundle<a name="update-custom-bundle"></a>

You can update an existing custom WorkSpaces bundle by modifying a WorkSpace that is based on the bundle, creating an image from the WorkSpace, and updating the bundle with the new image\. You can then launch new WorkSpaces using the updated bundle\.

**Important**  
Existing WorkSpaces aren't automatically updated when you update the bundle that they're based on\. To update existing WorkSpaces that are based on a bundle that you've updated, you must either rebuild the WorkSpaces or delete and recreate them\.

**To update a bundle using the console**

1. Connect to a WorkSpace that is based on the bundle and make the changes that you want\. For example, you can apply the latest operating system and application patches and install additional applications\.

   Alternatively, you can create a new WorkSpace with the same base software package \(Plus or Standard\) as the image used to create the bundle, and make changes\.

1. If you are still connected to the WorkSpace, disconnect by choosing **Amazon WorkSpaces** and **Disconnect** in the WorkSpaces client application\.

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Create Image**\. If the status of the WorkSpace is `STOPPED`, you must start it first \(choose **Actions**, **Start WorkSpaces**\) before you can choose **Actions**, **Create Image**\.

1. Enter an image name and a description, and then choose **Create Image**\. The WorkSpace is unavailable while the image is being created\. For detailed information about the image creation process, see [Create a Custom WorkSpaces Image and Bundle](create-custom-bundle.md)\.

1. In the navigation pane, choose **Bundles**\.

1. Choose the bundle to open its details page, and then under **Source image**, choose **Edit**\.

1. On the **Update source image** page, select the image that you created and choose **Update bundle**\.

1. As needed, update any existing WorkSpaces that are based on the bundle by rebuilding the WorkSpaces or deleting and recreating them\. For more information, see [Rebuild a WorkSpace](rebuild-workspace.md)\.

**To update a bundle programmatically**  
To update a bundle programmatically, use the UpdateWorkspaceBundle API action\. For more information, see [ UpdateWorkspaceBundle](https://docs.aws.amazon.com/workspaces/latest/api/API_UpdateWorkspaceBundle.html) in the *Amazon WorkSpaces API Reference*\.