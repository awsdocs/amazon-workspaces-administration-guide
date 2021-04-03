# Protocols for Amazon WorkSpaces<a name="amazon-workspaces-protocols"></a>

Amazon WorkSpaces supports two protocols: PCoIP and WorkSpaces Streaming Protocol \(WSP\)\. The protocol that you choose depends on several factors, such as the type of devices your users will be accessing their WorkSpaces from, which operating system is on your WorkSpaces, what network conditions your users will be facing, and whether your users require bidirectional video support\.

**When to Use PCoIP**
+ If you want to use the iPad, Android, or Linux clients\.
+ If you use Teradici zero client devices\.
+ If you need to use GPU\-based bundles \(Graphics or GraphicsPro\)\.
+ If you need to use a Linux bundle for non\-smart card use cases\.
+ If you need to use WorkSpaces in the China \(Ningxia\) Region\.

**When to Use WSP**
+ If you need higher loss/latency tolerance to support your end user network conditions\. For example, you have users who are accessing their WorkSpaces across global distances or using unreliable networks\.
+ If you need your users to authenticate with smart cards or to use smart cards in\-session\.
+ If you need webcam support capabilities in\-session\.

**Note**  
A directory can have a mix of PCoIP and WSP WorkSpaces in it\.
A user can have both a PCoIP and a WSP WorkSpace as long as the two WorkSpaces are located in separate directories\. The same user cannot have a PCoIP and a WSP WorkSpace in the same directory\. For more information about creating multiple WorkSpaces for a user, see [Create Multiple WorkSpaces for a User](create-multiple-workspaces-for-user.md)\.
You can migrate a WorkSpace between the two protocols by using the WorkSpaces migration feature, which requires a rebuild of the WorkSpace\. For more information, see [Migrate a WorkSpace](migrate-workspaces.md)\.