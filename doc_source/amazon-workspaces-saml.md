# WorkSpaces Integration with SAML 2\.0 \(preview\)<a name="amazon-workspaces-saml"></a>



Integrating SAML 2\.0 with your WorkSpaces for desktop session authentication allows your users to use their existing SAML 2\.0 identity provider \(IdP\) credentials and authentication methods through their default web browser\. By using your IdP to authenticate users for WorkSpaces, you can protect WorkSpaces by employing IdP features like multi\-factor authentication and contextual access policies\.

## Authentication workflow<a name="authentication-workflow"></a>

The following sections describe the authentication workflow between WorkSpaces and a SAML 2\.0 identity provider \(IdP\):
+ When the flow is initiated by the IdP\. For example, when a user clicks on an application in the IdP user portal in a web browser\.
+ When the flow is initiated by the WorkSpaces client\. For example, when a user opens the client and signs in\.

In these examples, the user enters `user@example.com`to sign in to the IdP\. The IdP has a SAML 2\.0 service provider application configured for a WorkSpaces directory and the user is authorized for the WorkSpaces SAML 2\.0 application\. The user creates a WorkSpace for the user name, `user`, in a directory that's enabled for SAML 2\.0 authentication\. Additionally, the user installs the [WorkSpaces client application](https://clients.amazonworkspaces.com/) on their device\.

**Identity provider \(IdP\)\-initiated flow**

The IdP\-initiated flow allows users to automatically register the WorkSpaces client application on their devices without having to enter a WorkSpaces registration code\. Users don't sign in to their WorkSpaces using the IdP\-initiated flow\. WorkSpaces authentication must originate from the client application\.

1. Using their web browser, the user signs in to the IdP\.

1. After signing in to the IdP, the user clicks the WorkSpaces application from the IdP user portal\.

1. The user is redirected to this page in the browser, and the WorkSpaces client application is opened automatically\.   
![\[Opening WorkSpaces application redirection page\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/saml-redir.png)

1. The WorkSpaces client is now registered and the user can continue to sign by clicking **Continue to sign in to WorkSpaces**\.

**WorkSpaces client\-initiated flow**

The client\-initiated flow allows users to sign in to their WorkSpaces after signing in to an IdP\.

1. The user launches the WorkSpaces client application \(if it isn't already running\) and clicks **Continue to sign in to WorkSpaces**\.

1. The user is redirected to their default web browser to sign in to the IdP\. If the user is already signed in to the IdP in their browser, they don't need to sign in again and will skip this step\.

1. Once signed in to the IdP, the user is redirected to this page in the browser, and clicks **Log in to WorkSpaces**\.  
![\[Confirm you want to continue logging into WorkSpaces\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/saml-confirm-login.png)

1. The user is redirected to the WorkSpaces client application to complete sign in to their WorkSpace\. The WorkSpaces user name is populated automatically from the IdP SAML 2\.0 assertion\.

1. The user is signed in to their WorkSpace\.