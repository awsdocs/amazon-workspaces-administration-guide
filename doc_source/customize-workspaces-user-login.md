# Customize How Users Log in to their WorkSpaces<a name="customize-workspaces-user-login"></a>

Customize your users' access to WorkSpaces by using uniform resource identifiers \(URIs\) to provide a simplified login experience that integrates with existing workflows in your organization\. For example, automatically generate login URIs that registers your users by using their WorkSpaces registration code\. As a result: 
+ Users can bypass the manual registration process\.
+ Their user names are automatically entered on their WorkSpaces client login page\.
+ Their user names and multifactor authentication \(MFA\) code are automatically entered on their client login page, if MFA is used in your organization\.

You can configure URI access to WorkSpaces for client applications on the following supported devices: 
+ Windows computers
+ macOS computers
+ iPads
+ Android tablets

To use URIs to access their WorkSpaces, users must first install the client application for their device by opening [http://clients\.amazonworkspaces\.com/](http://clients.amazonworkspaces.com/) and following the directions\.

URI access is supported on the Firefox and Chrome browsers on Windows and macOS computers, and on Windows computers, the Internet Explorer and Microsoft Edge browsers\. For more information about WorkSpaces clients, see [Amazon WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide*\.

**Note**  
On Android devices, URI access works only with the Firefox browser, not with the Google Chrome browser\.

To configure URI access to WorkSpaces, use any of the URI formats described in the following table\.

**Note**  
If the data component of your URI includes any of the following reserved characters, we recommend that you use percent\-encoding in the data component to avoid ambiguity:   
`@ : / ? & =`  
For example, if you have user names that include any of these characters, you should percent\-encode those user names in your URI\. For more information, see [Uniform Resource Identifier \(URI\): Generic Syntax](https://www.rfc-editor.org/rfc/rfc3986.txt)\.


| Supported Syntax | Description | 
| --- | --- | 
| workspaces:// | Opens the WorkSpaces client application\. | 
| workspaces://@registrationcode | Registers a user by using their WorkSpaces registration code\. Also displays the client login page\. | 
| workspaces://username@registrationcode | Registers a user by using their WorkSpaces registration code\. Also automatically enters the user name in the username field on the client login page\. | 
| workspaces://username@registrationcode?MFACode=mfa | Registers a user by using their WorkSpaces registration code\. Also automatically enters the user name in the username field and the multifactor authentication \(MFA\) code in the MFA code field on the client login page\. | 
| workspaces://@registrationcode?MFACode=mfa | Registers a user by using their WorkSpaces registration code\. Also automatically enters the multifactor authentication \(MFA\) code in the MFA code field on the client login page\. | 

**Note**  
If users open a URI link when they are already connected to a WorkSpace from a Windows client, a new WorkSpaces session opens and their original WorkSpaces session remains open\. If users open a URI link when they are connected to a WorkSpace from a macOS, iPad, or Android client, no new session opens; only their original WorkSpaces session remains open\.