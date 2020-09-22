# Tag WorkSpaces Resources<a name="tag-workspaces-resources"></a>

You can organize and manage the resources for your WorkSpaces by assigning your own metadata to each resource in the form of *tags*\. You specify a *key* and a *value* for each tag\. A key can be a general category, such as "project," "owner," or "environment," with specific associated values\. Using tags is a simple yet powerful way to manage AWS resources and to organize data, including billing data\.

When you add tags to an existing resource, those tags don't appear in your cost allocation report until the first day of the following month\. For example, if you add tags to an existing WorkSpace on July 15, the tags won't appear in your cost allocation report until August 1\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing and Cost Management User Guide*\.

**Note**  
To view your WorkSpaces resource tags in the Cost Explorer, you must activate the tags that you have applied to your WorkSpaces resources by following the instructions in [Activating User\-Defined Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html) in the *AWS Billing and Cost Management User Guide*\.  
Although tags appear 24 hours after activation, it can take 4 to 5 days for values associated with those tags to appear in the Cost Explorer\. Additionally, to appear and provide cost data in Cost Explorer, WorkSpaces resources that have been tagged must incur charges during that time\. Cost Explorer only shows cost data from the time when the tags were activated and onward\. No historical data is available at this time\.

**Resources That You Can Tag**
+ You can add tags to the following resources when you create them—WorkSpaces, imported images, and IP access control groups\.
+ You can add tags to existing resources of the following types—WorkSpaces, registered directories, custom bundles, images, and IP access control groups\.

**Tag Restrictions**
+ Maximum number of tags per resource—50
+ Maximum key length—127 Unicode characters
+ Maximum value length—255 Unicode characters
+ Tag keys and values are case\-sensitive\. Allowed characters are letters, spaces, and numbers representable in UTF\-8, plus the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\.
+ Do not use the "aws:" or "aws:workspaces:" prefixes in your tag names or values because they are reserved for AWS use\. You can't edit or delete tag names or values with these prefixes\.

**To update the tags for an existing resource using the console**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose one of the following resource types: **Directories**, **WorkSpaces**, **Bundles**, **Images**, or **IP Access Controls**\.

1. Select the resource and choose **Actions**, **Manage Tags**\.

1. Do one or more of the following:
   + To update a tag, edit the values of **Key** and **Value**\.
   + To add a tag, choose **Add Tag** and then edit the values of **Key** and **Value**\.
   + To delete a tag, choose the delete icon \(X\) next to the tag\.

1. When you are finished updating tags, choose **Save**\.

**To update the tags for an existing resource using the AWS CLI**  
Use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/workspaces/create-tags.html) and [delete\-tags](https://docs.aws.amazon.com/cli/latest/reference/workspaces/delete-tags.html) commands\.