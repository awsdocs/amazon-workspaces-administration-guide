# Tag a WorkSpace<a name="tag-workspaces"></a>

You can organize and manage your WorkSpaces by assigning your own metadata to each WorkSpace in the form of *tags*\. You specify a *key* and a *value* for each tag\. A key can be a general category, such as "project," "owner," or "environment," with specific associated values\. Using tags is a simple yet powerful way to manage AWS resources and organize data, including billing data\.

You can apply tags to a WorkSpace when you launch it or apply them to the WorkSpace later on\. Each tag automatically applies to all WAM applications and WAM related service charges for the WorkSpace\. Tags added to existing WorkSpaces appear in your cost allocation report on the first of the following month for WorkSpaces renewed in that month\. For more information, see [Setting Up Your Monthly Cost Allocation Report](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html)\.

**Tag Restrictions**
+ The maximum number of tags per WorkSpace is 50\.
+ The maximum key length is 127 characters\.
+ The maximum value length is 255 characters\.
+ Tag keys and values are both case\-sensitive\.
+ Tags with a prefix of "aws:" or "aws:workspaces:" cannot be used\. These prefixes are reserved for AWS and WorkSpaces, respectively\. Tags with these prefixes cannot be edited or deleted\.

**To update the tags for an existing WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Select the WorkSpace and choose **Actions**, **Manage Tags**\.

1. Do one or more of the following:

   1. To update a tag, edit the values of **Key** and **Value**\.

   1. To add a tag, choose **Add Tag** and then edit the values of **Key** and **Value**\.

   1. To delete a tag, choose the delete icon \(X\) next to the tag\.

1. When you are finished updating tags, choose **Save**\.