# Setting up SAML 2\.0 \(preview\)<a name="setting-up-saml"></a>

Enable WorkSpaces client application registration and signing in to WorkSpaces for your users by using their SAML 2\.0 identity provider \(IdP\) credentials and authentication methods by setting up identity federation using SAML 2\.0\. To set up identity federation using SAML 2\.0, use an IAM role and a relay state URL to configure your IdP and enable AWS\. This grants your federated users access to a WorkSpaces directory\. The relay state is the WorkSpaces directory endpoint to which users are forwarded after successfully signing in to AWS\. 

**Topics**
+ [Requirements \(preview\)](#setting-up-saml-requirements)
+ [Prerequisites](#setting-up-saml-prerequisites)
+ [Step 1: Create a SAML identity provider in AWS IAM](#create-saml-identity-provider)
+ [Step 2: Create a SAML 2\.0 federation IAM role](#create-saml-iam-role)
+ [Step 3: Embed an inline policy for the IAM role](#embed-inline-policy)
+ [Step 4: Configure your SAML 2\.0 identity provider](#configure-saml-id-provider)
+ [Step 5: Create assertions for the SAML authentication response](#create-assertions-saml-auth)
+ [Step 6: Configure the relay state of your federation](#configure-relay-state)
+ [Step 7: Enable integration with SAML 2\.0 on your WorkSpaces directory](#enable-integration-saml)

## Requirements \(preview\)<a name="setting-up-saml-requirements"></a>
+ SAML 2\.0 authentication is available in the following Regions:
  + US East \(N\. Virginia\) Region
  + US West \(Oregon\) Region
  + Asia Pacific \(Mumbai\) Region
  + Asia Pacific \(Seoul\) Region
  + Asia Pacific \(Singapore\) Region
  + Asia Pacific \(Sydney\) Region
  + Asia Pacific \(Tokyo\) Region
  + Canada \(Central\) Region
  + Europe \(Frankfurt\) Region
  + Europe \(Ireland\) Region
  + Europe \(London\) Region
+ During the SAML 2\.0 authentication preview, SAML must be configured on a WorkSpaces directory using WorkSpaces API or the AWS CLI\. The WorkSpaces management console can't be used to configure SAML properties\.
+ The user must be able to sign in to WorkSpaces by entering their AD user name without the domain, as `username`\. Directory use cases that require user name formats such as `corp\username`, `corp.example.com\username`, or `username@corp.example.com` aren't supported\. As a result, SAML 2\.0 authentication isn't supported with WorkSpaces that are launched using AWS Managed Microsoft AD trusted domains\.
+ To use SAML 2\.0 authentication with WorkSpaces, the IdP must support IdP\-initiated deep linking for the relay state URL\. Examples of IdPs include ADFS, Azure AD, Duo Single Sign\-On, Okta, PingFederate, and PingOne\. Consult your IdP documentation for more information\.
+ SAML 2\.0 authentication will function with WorkSpaces launched using Simple AD, but this isn't recommended as Simple AD doesn't integrate with SAML 2\.0 IdPs\.
+ SAML 2\.0 authentication is supported on the following WorkSpaces clients\. Open Amazon WorkSpaces [Client Downloads](https://clients.amazonworkspaces.com/) to find the latest versions\. Other client versions won't be able to connect to WorkSpaces enabled for SAML 2\.0 authentication unless fallback is enabled\. For more information, see [ Enable SAML 2\.0 authentication on the WorkSpaces directory:](https://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html#enable-integration-saml)
  + Windows client application version 5\.1\.0\.3029 or later
  + macOS client version 5\.x or later

**Important**  
The **FIPS 140\-2 Validated Mode** setting for WorkSpaces endpoint encryption doesn't apply to relay state endpoints that are used for the SAML 2\.0 authentication preview\. For more information, see [FIPS endpoint encryption](https://docs.aws.amazon.com/workspaces/latest/adminguide/fips-encryption.html)\.

## Prerequisites<a name="setting-up-saml-prerequisites"></a>

Complete the following prerequisites before configuring your SAML 2\.0 identity provider \(IdP\) connection to a WorkSpaces directory\.

1. Configure your IdP to integrate user identities from the Microsoft Active Directory that is used with the WorkSpaces directory\. For a user with a WorkSpace, the **sAMAccountName** and **email** attributes for the Active Directory user and the IdP user must match for the user to sign in to WorkSpaces using the IdP\. For more information about integrating Active Directory with your IdP, consult your IdP documentation\.

1. Configure your IdP to establish a trust relationship with AWS\.
   + See [Integrating third\-party SAML solution providers with AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml_3rd-party.html) for more information on configuring AWS federation\. Relevant examples include IdP integration with AWS IAM to access the AWS management console\.
   + Use your IdP to generate and download a federation metadata document that describes your organization as an IdP\. This signed XML document is used to establish the relying party trust\. Save this file to a location that you can access from the IAM console later\.

1. Create or register a directory for WorkSpaces by using the WorkSpaces management console\. For more information, see [Manage directories for WorkSpaces](https://docs.aws.amazon.com/workspaces/latest/adminguide/manage-workspaces-directory.html)\. SAML 2\.0 authentication for WorkSpaces is supported for the following directory types:
   + AD Connector
   + AWS Managed Microsoft AD

1. Create a WorkSpace for a user who can sign in to the IdP using a supported directory type\. You can create a WorkSpace using the WorkSpaces management console, AWS CLI, or WorkSpaces API\. For more information, see [Launch a virtual desktop using WorkSpaces](https://docs.aws.amazon.com/workspaces/latest/adminguide/launch-workspaces-tutorials.html)\. 

## Step 1: Create a SAML identity provider in AWS IAM<a name="create-saml-identity-provider"></a>

First, create a SAML IdP in AWS IAM\. This IdP defines your organization's IdP\-to\-AWS trust relationship using the metadata document generated by the IdP software in your organization\. For more information, see [ Creating and managing a SAML identity provider \(Amazon Web Services Management Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html#idp-manage-identityprovider-console)\.

## Step 2: Create a SAML 2\.0 federation IAM role<a name="create-saml-iam-role"></a>

Next, create a SAML 2\.0 federation IAM role\. This step establishes a trust relationship between IAM and your organization's IdP, which identifies your IdP as a trusted entity for federation\.

To create an IAM role for SAML IdP

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** > **Create role**\.

1.  For **Role type**, choose **SAML 2\.0 federation**\. 

1.  For **SAML Provider** select the SAML IdP that you created\. 
**Important**  
Don't choose either of the two SAML 2\.0 access methods, **Allow programmatic access only** or **Allow programmatic and Amazon Web Services Management Console access**\.

1. For **Attribute**, choose **SAML:sub\_type**\.

1. For **Value** enter `persistent`\. This value restricts role access to SAML user streaming requests that include a SAML subject type assertion with a value of persistent\. If the SAML:sub\_type is persistent, your IdP sends the same unique value for the NameID element in all SAML requests from a particular user\. For more information about the SAML:sub\_type assertion, see the **Uniquely identifying users in SAML\-based federation** section in [ Using SAML\-based federation for API access to AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html#CreatingSAML-configuring)\.

1.  Review your SAML 2\.0 trust information, confirming the correct trusted entity and condition, and then choose **Next: Permissions**\. 

1. On the **Attach permissions policies** page, choose **Next: Tags**\.

1. \(Optional\) Enter a key and value for each tag that you want to add\. For more information, see [Tagging IAM users and roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html)\.

1. When you're done, choose **Next: Review**\. You'll create and embed an inline policy for this role later\.

1. For **Role name**, enter a name that identifies the purpose of this role\. Because multiple entities might reference the role, you can't edit the role's name once it is created\.

1. \(Optional\) For **Role description**, enter a description for the new role\.

1. Review the role details and choose **Create role**\.

1. Add the sts:TagSession permission to your new IAM role's trust policy\. For more information, see [ Attribute\-based application entitlements using a third\-party SAML 2\.0 identity provider](https://docs.aws.amazon.com/appstream2/latest/developerguide/application-entitlements-saml.html) and [Passing session tags in AWS STS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html)\. In your new IAM role's details, choose the **Trust relationships** tab, and then choose **Edit trust relationship\***\. When Edit Trust Relationship policy editor opens, add the **sts:TagSession\*** permission, as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Federated": "arn:aws:iam::ACCOUNT-ID-WITHOUT-HYPHENS:saml-provider/IDENTITY-PROVIDER"
         },
         "Action": [
           "sts:AssumeRoleWithSAML",
           "sts:TagSession"
         ],
         "Condition": {
           "StringEquals": {
             "SAML:sub_type": "persistent"
           }
         }
       }
     ]
   }
   ```

Replace `IDENTITY-PROVIDER` with the name of the SAML IdP you created in Step 1\. Then choose **Update Trust Policy**\.

## Step 3: Embed an inline policy for the IAM role<a name="embed-inline-policy"></a>

Next, embed an inline IAM policy for the role that you created\. When you embed an inline policy, the permissions in that policy can't be accidentally attached to the wrong principal entity\. The inline policy provides federated users with access to the WorkSpaces directory\.

1. In the details for the IAM role that you created, choose the **Permissions** tab, and then choose **Add inline policy**\. The Create policy wizard will start\.

1. In **Create policy**, choose the **JSON** tab\.

1. Copy and paste the following JSON policy into the JSON window\. Then, modify the resource by entering your AWS Region Code, account ID, and directory ID\. In the following policy, `"Action": "workspaces:Stream"` is the action that provides your WorkSpaces users with permissions to connect to their desktop sessions in the WorkSpaces directory\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "workspaces:Stream",
               "Resource": "arn:aws:workspaces:REGION-CODE:ACCOUNT-ID-WITHOUT-HYPHENS:directory/DIRECTORY-ID",
               "Condition": {
                   "StringEquals": {
                       "workspaces:userId": "${saml:sub}"
                   }
               }
           }
       ]
   }
   ```

   Replace `REGION-CODE` with the AWS Region where your WorkSpaces directory exists\. Replace `DIRECTORY-ID` with the WorkSpaces directory ID, which can be found in the WorkSpaces management console\.

1. When you're done, choose **Review policy**\. The [Policy Validator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_policy-validator.html) will report any syntax errors\.

## Step 4: Configure your SAML 2\.0 identity provider<a name="configure-saml-id-provider"></a>

Next, depending on your SAML 2\.0 IdP, you may need to manually update your IdP to trust AWS as a service provider by uploading the `saml-metadata.xml` file at [https://signin\.aws\.amazon\.com/static/saml\-metadata\.xml](https://signin.aws.amazon.com/static/saml-metadata.xml) to your IdP\. This step updates your IdP’s metadata\. For some IdPs, the update may already be configured\. If this is the case, proceed to the next step\.

If this update isn't already configured in your IdP, review the documentation provided by your IdP for information about how to update the metadata\. Some providers give you the option to type the URL, and the IdP obtains and installs the file for you\. Others require you to download the file from the URL and then provide it as a local file\.

**Important**  
At this time, you can also authorize users in your IdP to access the WorkSpaces application you have configured in your IdP\. Users who are authorized to access the WorkSpaces application for your directory don't automatically have a WorkSpace created for them\. Likewise, users that have a WorkSpace created for them are not automatically authorized to access the WorkSpaces application\. To successfully connect to a WorkSpace using SAML 2\.0 authentication, a user must be authorized by the IdP and must have a WorkSpace created\.

## Step 5: Create assertions for the SAML authentication response<a name="create-assertions-saml-auth"></a>

Next, configure the information that your IdP sends to AWS as SAML attributes in its authentication response\. Depending on your IdP, this is already configured, skip this step and continue to [ Step 6: Configure the relay state of your federation](https://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html#configure-relay-state)\.

If this information isn't already configured in your IdP, provide the following:
+ **SAML Subject NameID** – The unique identifier for the user who is signing in\. The value must match the WorkSpaces user name, and is typically the **sAMAccountName** attribute for the Active Directory user\.
+ **SAML Subject Type** \(with a value set to `persistent`\) – Setting the value to `persistent` ensures that your IdP sends the same unique value for the `NameID` element in all SAML requests from a particular user\. Make sure that your IAM policy includes a condition to only allow SAML requests with a SAML sub\_type set to `persistent`, as described in [ Step 2: Create a SAML 2\.0 federation IAM role](https://docs.aws.amazon.com/appstream2/latest/developerguide/external-identity-providers-setting-up-saml.html#external-identity-providers-grantperms)\.
+ **`Attribute` element with the `Name` attribute set to `https://aws.amazon.com/SAML/Attributes/Role`** – This element contains one or more `AttributeValue` elements that list the IAM role and SAML IdP to which the user is mapped by your IdP\. The role and IdP are specified as a comma\-delimited pair of ARNs\.
+ **`Attribute` element with the `Name` attribute set to `https://aws.amazon.com/SAML/Attributes/RoleSessionName`** – This element contains one `AttributeValue` element that provides an identifier for the AWS temporary credentials that are issued for SSO\. The value in the `AttributeValue` element must be between 2 and 64 characters long, can contain only alphanumeric characters, underscores, and the following characters: **\.**, **,**, **\+**, **=**, **@**, **\-**\. It can't contain spaces\. The value is typically an email address or a user principal name \(UPN\)\. It shouldn't be a value that includes a space, such as a user's display name\.
+ **`Attribute` element with the `Name` attribute set to `https://aws.amazon.com/SAML/Attributes/PrincipalTag:Email`** – This element contains one `AttributeValue` element that provides the email address of the user\. The value must match the WorkSpaces user email address as defined in the WorkSpaces directory\. Tag values may include combinations of letters, numbers, spaces, and **\_**, **:**, **/****\.**, **,**, **\+**, **=**, **@**, **\-** characters\. For more information, see [Rules for tagging in IAM and AWS STS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html#id_tags_rules) in the *IAM User Guide*\. 
+ **`Attribute` element with the `Name` attribute set to https://aws\.amazon\.com/SAML/Attributes/SessionDuration** \(optional\) – This element contains one `AttributeValue` element that specifies the maximum amount of time that a federated streaming session for a user can remain active before reauthentication is required\. The default value is 60 minutes\. For more information, see the [ SAML `SessionDurationAttribute`](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html#saml_role-session-duration)\. 
**Note**  
Although `SessionDuration` is an optional attribute, we recommend that you include it in the SAML response\. If you don't specify this attribute, the session duration is set to a default value of 60 minutes\. WorkSpaces desktop sessions are disconnected after their session duration expires\.

For more information about how to configure these elements, see [ Configuring SAML assertions for the authentication response](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html) in the *IAM User Guide*\. For information about specific configuration requirements for your IdP, see your IdP's documentation\.

## Step 6: Configure the relay state of your federation<a name="configure-relay-state"></a>

Next, use your IdP to configure the relay state of your federation to point to the WorkSpaces directory relay state URL\. After successful authentication by AWS, the user is directed to the WorkSpaces directory endpoint, defined as the relay state in the SAML authentication response\.

The following is the relay state URL format:

```
https://relay-state-region-endpoint/sso-idp?registrationCode=registration-code
```

Construct your relay state URL from your WorkSpaces directory registration code and the relay state endpoint associated with the Region in which your directory is located\. The registration code can be found in the WorkSpaces management console\.

Optionally, if you are using cross\-region redirection for WorkSpaces, you can substitute the registration code with the fully qualified domain name \(FQDN\) associated with directories in your primary and failover Regions\. For more information, see [Cross\-region redirection for Amazon WorkSpaces](https://docs.aws.amazon.com/workspaces/latest/adminguide/cross-region-redirection.html)\. When using cross\-region redirection and SAML 2\.0 authentication, both primary and failover directories need to be enabled for SAML 2\.0 authentication and independently configured with the IdP, using the relay state endpoint associated with each Region\. This will allow the FQDN to be configured correctly when users register their WorkSpaces client applications before signing in, and will allow users to authenticate during a failover event\.

The following table lists the relay state endpoints for the Regions where WorkSpaces SAML 2\.0 authentication is available\.


**Regions where WorkSpaces SAML 2\.0 authentication is available**  

| Region | Relay state endpoint | 
| --- | --- | 
| US East \(N\. Virginia\) Region | workspaces\.euc\-sso\.us\-east\-1\.aws\.amazon\.com | 
| US West \(Oregon\) Region | workspaces\.euc\-sso\.us\-west\-2\.aws\.amazon\.com | 
| Asia Pacific \(Mumbai\) Region | workspaces\.euc\-sso\.ap\-south\-1\.aws\.amazon\.com | 
| Asia Pacific \(Seoul\) Region | workspaces\.euc\-sso\.ap\-northeast\-2\.aws\.amazon\.com | 
| Asia Pacific \(Singapore\) Region | workspaces\.euc\-sso\.ap\-southeast\-1\.aws\.amazon\.com | 
| Asia Pacific \(Sydney\) Region | workspaces\.euc\-sso\.ap\-southeast\-2\.aws\.amazon\.com | 
| Asia Pacific \(Tokyo\) Region | workspaces\.euc\-sso\.ap\-northeast\-1\.aws\.amazon\.com | 
| Canada \(Central\) Region | workspaces\.euc\-sso\.ca\-central\-1\.aws\.amazon\.com | 
| Europe \(Frankfurt\) Region | workspaces\.euc\-sso\.eu\-central\-1\.aws\.amazon\.com | 
| Europe \(Ireland\) Region | workspaces\.euc\-sso\.eu\-west\-1\.aws\.amazon\.com | 
| Europe \(London\) Region | workspaces\.euc\-sso\.eu\-west\-2\.aws\.amazon\.com | 

## Step 7: Enable integration with SAML 2\.0 on your WorkSpaces directory<a name="enable-integration-saml"></a>

Finally, enable SAML 2\.0 authentication on the WorkSpaces directory by performing the following steps\. Once this step is completed, you can use the IdP\-initiated and client application\-initiated flows to register WorkSpaces client applications and sign in to WorkSpaces\.

**Note**  
During the SAML 2\.0 authentication preview, SAML must be enabled on the WorkSpaces directory using the AWS CLI or WorkSpaces API\. For more information, see the [AWS CLI command reference for WorkSpaces](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/workspaces/index.html#cli-aws-workspaces), and the [Amazon WorkSpaces API reference](https://docs.aws.amazon.com/workspaces/latest/api/welcome.html)\. The following examples reference the AWS CLI\.

1. Construct the following “saml\.json” file:

   ```
   {
       "ResourceId": "DIRECTORY-ID",
       "SamlProperties": {
           "Status": "DISABLED | ENABLED | ENABLED_WITH_DIRECTORY_LOGIN_FALLBACK",
           "UserAccessUrl": "USER-ACCESS-URL",
           "RelayStateParameterName": "RELAY-STATE-PARAMETER-NAME"
       }
    }
   ```

   Replace `DIRECTORY-ID` with the WorkSpaces directory ID, which can be found in the WorkSpaces management console\.

   For `Status`, select one of the following valid options: `DISABLED`, `ENABLED`, or `ENABLED_WITH_DIRECTORY_LOGIN_FALLBACK`\.
   + `DISABLED` is the default status for WorkSpaces directories and doesn't enable SAML 2\.0 authentication\.
   + `ENABLED` status enables SAML 2\.0 authentication\. Users attempting to connect to WorkSpaces from a client application that doesn't support SAML 2\.0 authentication will not be able to connect\.
   + `ENABLED_WITH_DIRECTORY_LOGIN_FALLBACK` status enables SAML 2\.0 authentication with supported client applications, but doesn't prevent clients that don't support SAML 2\.0 authentication from connecting as if SAML 2\.0 authentication was disabled\.

   For the **UserAccessUrl** and **RelayStateParameterName**, replace `USER-ACCESS-URL` and `RELAY-STATE-PARAMETER-NAME` with values that are applicable to your IdP and the application you have configured in Step 1\. The default value for `RelayStateParameterName` is “RelayState“ if you omit this parameter\. The following table lists user access URL and relay state parameter names that are unique to various identity providers for applications\. Use this table to determine the correct values\. Typically, the user access URL is the URL a user would navigate to in their web browser in order to federate and directly access the application, without any SAML 2\.0 service provider \(SP\) bindings\.  
**Domains and IP addresses to add to your allow list**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/setting-up-saml.html)

1. Adjust the “saml\.json” path as needed and run the following command to modify your WorkSpaces directory to enable SAML authentication:

   ```
   aws workspaces modify-saml-properties --region REGION-CODE --cli-input-json file://saml.json
   ```

   Replace `REGION-CODE` with the AWS Region where your WorkSpaces directory exists\.