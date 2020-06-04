# Delete a WorkSpace<a name="delete-workspaces"></a>

When you are finished with a WorkSpace, you can delete it\. You can also delete related resources\.

**Warning**  
This is a permanent action and cannot be undone\. The WorkSpace user's data does not persist and is destroyed\. For help with backing up user data, contact AWS Support\.

**To delete a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select your WorkSpace and choose **Actions**, **Remove WorkSpaces**\.

1. When prompted for confirmation, choose **Remove WorkSpaces**\. The status of the WorkSpace is set to `TERMINATING`\. When the termination is complete, the status is set to `TERMINATED`\.

1. \(Optional\) To delete any custom bundles and images that you are finished with, see [Delete a Custom WorkSpaces Bundle or Image](delete_bundle.md)\.

1. \(Optional\) After you delete all WorkSpaces in a directory, you can delete the directory\. For more information, see [Delete the Directory for Your WorkSpaces](delete-workspaces-directory.md)\.

1. \(Optional\) After you delete all resources in the virtual private cloud \(VPC\) for your directory, you can delete the VPC and release the Elastic IP address used for the NAT gateway\. For more information, see [ Deleting your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#VPC_Deleting) and [ Working with Elastic IP addresses](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html#WorkWithEIPs) in the *Amazon VPC User Guide*\.