# Create multiple WorkSpaces for a user<a name="create-multiple-workspaces-for-user"></a>

By default, you can create only one WorkSpace per user per directory\. However, if needed, you can create more than one WorkSpace for a user, depending on your directory setup\. 
+ If you have only one directory for your WorkSpaces, create multiple usernames for the user\. For example, a user named Mary Major can have mmajor1, mmajor2, and so on as usernames\. Each username is associated with a different WorkSpace in the same directory, but the WorkSpaces have the same registration code, as long as the WorkSpaces are all created in the same directory in the same AWS Region\.
+ If you have multiple directories for your WorkSpaces, create the WorkSpaces for the user in separate directories\. You can use the same username in the directories, or you can use different usernames in the directories\. The WorkSpaces will have different registration codes\.

**Tip**  
So that you can easily locate all the WorkSpaces that you've created for a user, use the same base username for each WorkSpace\.  
For example, if you have a user named Mary Major with the Active Directory username mmajor, create WorkSpaces for her with usernames such as mmajor, mmajor1, mmajor2, mmajor3, or other variants, such as mmajor\_windows or mmajor\_linux\. As long as all the WorkSpaces have the same starting base username \(mmajor\), you can sort on the username in your WorkSpaces console to group all of the WorkSpaces for that user together\.

**Important**  
A user can have both a PCoIP and a WSP WorkSpace as long as the two WorkSpaces are located in separate directories\. The same user cannot have a PCoIP and a WSP WorkSpace in the same directory\. 
If you are setting up multiple WorkSpaces for use with cross\-Region redirection, you must set up the WorkSpaces in different directories in different AWS Regions, and you must use the same usernames in each directory\. For more information about cross\-Region redirection, see [Cross\-region redirection for Amazon WorkSpaces](cross-region-redirection.md)\. 

To switch between the WorkSpaces, the user logs in with the username and registration code associated with a particular Workspace\. If the user is using a 3\.0\+ version of the WorkSpaces client applications for Windows, macOS, or Linux, the user can assign different names to the WorkSpaces by going to **Settings**, **Manage Login Information** in the client application\.