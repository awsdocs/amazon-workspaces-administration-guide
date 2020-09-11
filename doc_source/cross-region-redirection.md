# Cross\-Region Redirection for Amazon WorkSpaces<a name="cross-region-redirection"></a>

With the cross\-Region redirection feature in Amazon WorkSpaces, you can use a fully qualified domain name \(FQDN\) as the registration code for your WorkSpaces\. Cross\-Region redirection works with your Domain Name System \(DNS\) routing policies to redirect your WorkSpaces users to alternative WorkSpaces when their primary WorkSpaces aren't available\. For example, by using DNS failover routing policies, you can connect your users to WorkSpaces in your specified failover AWS Region when they can't access their WorkSpaces in the primary Region\.

You can use cross\-Region redirection along with your DNS failover routing policies to achieve regional resiliency and high availability\. You can also use this feature for other purposes, such as traffic distribution or providing alternative WorkSpaces during maintenance periods\. If you use Amazon Route 53 for your DNS configuration, you can take advantage of health checks that monitor Amazon CloudWatch alarms\.

To use this feature, you must set up WorkSpaces for your users in two \(or more\) AWS Regions\. You must also create special FQDN\-based registration codes called *connection aliases*\. These connection aliases replace Region\-specific registration codes for your WorkSpaces users\. \(The Region\-specific registration codes remain valid; however, for cross\-Region redirection to work, your users must use the FQDN instead as their registration code\.\)

To create a connection alias, you specify a *connection string*, which is your FQDN, such as `www.example.com` or `desktop.example.com`\. To use this domain for cross\-Region redirection, you must register it with a domain registrar and configure the DNS service for your domain\.

After you've created your connection aliases, you associate them with your WorkSpaces directories in different Regions to create *association pairs*\. Each association pair has a primary Region and one or more failover Regions\. If an outage occurs in the primary Region, your DNS failover routing policies redirect your WorkSpaces users to the WorkSpaces that you've set up for them in the failover Region\.

To designate your primary and failover Regions, you define the Region priority \(either primary or secondary\) when configuring your DNS failover routing policies\.

**Topics**
+ [Prerequisites](#cross-region-redirection-prerequisites)
+ [Limitations](#cross-region-redirection-limitations)
+ [Step 1: Create Your Connection Aliases](#cross-region-redirection-create-connection-aliases)
+ [\(Optional\) Step 2: Share a Connection Alias with Another Account](#cross-region-redirection-share-connection-alias)
+ [Step 3: Associate Your Connection Aliases with Directories in Each Region](#cross-region-redirection-associate-connection-aliases)
+ [Step 4: Configure Your DNS Service and Set Up Your DNS Routing Policies](#cross-region-redirection-configure-DNS-routing)
+ [Step 5: Send the Connection String to Your WorkSpaces Users](#cross-region-redirection-send-connection-string-to-users)
+ [What Happens During Cross\-Region Redirection](#cross-region-redirection-what-happens)
+ [Disassociate a Connection Alias from a Directory](#cross-region-redirection-disassociate-connection-alias)
+ [Unshare a Connection Alias](#cross-region-redirection-unshare-connection-alias)
+ [Delete a Connection Alias](#cross-region-redirection-delete-connection-alias)
+ [Security Considerations if You Stop Using Cross\-Region Redirection](#cross-region-redirection-security-considerations)

## Prerequisites<a name="cross-region-redirection-prerequisites"></a>
+ You must own and register the domain that you want to use as the FQDN in your connection aliases\. If you're not already using another domain registrar, you can use Amazon Route 53 to register your domain\. For more information, see [ Registering domain names using Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar.html) in the *Amazon Route 53 Developer Guide*\.
**Important**  
You must have all necessary rights to use any domain name that you use in conjunction with Amazon WorkSpaces\. You agree that the domain name does not violate or infringe on the legal rights of any third party or otherwise violate applicable law\.

  The total length of your domain name can't exceed 255 characters\. For more information about domain names, see [ DNS domain name format](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/DomainNameFormat.html) in the *Amazon Route 53 Developer Guide*\.

  Cross\-Region redirection works with both public domain names and domain names in private DNS zones\. If you're using a private DNS zone, you must provide a virtual private network \(VPN\) connection to the virtual private cloud \(VPC\) that contains your WorkSpaces\. If your WorkSpaces users attempt to use a private FQDN from the public internet, the WorkSpaces client applications return the following error message:

  `"We're unable to register the WorkSpace because of a DNS server issue. Contact your administrator for help."`
+ You must set up your DNS service and configure the necessary DNS routing policies\. Cross\-Region redirection works in conjunction with your DNS routing policies to redirect your WorkSpaces users as needed\.
+ In each primary and failover Region where you want to set up cross\-Region redirection, create WorkSpaces for your users\. Make sure that you use the same usernames in each WorkSpaces directory in each Region\. To keep your Active Directory user data in sync, we recommend using AD Connector to point to the same Active Directory in each Region where you've set up WorkSpaces for your users\. For more information about creating WorkSpaces, see [Launch WorkSpaces](launch-workspaces-tutorials.md)\.

  When you've finished setting up cross\-Region redirection, you must make sure your WorkSpaces users are using the FQDN\-based registration code instead of the Region\-based registration code \(for example, `WSpdx+ABC12D`\) for their primary Region\. To do this, you must send them an email with the FQDN connection string by using the procedure in [Step 5: Send the Connection String to Your WorkSpaces Users](#cross-region-redirection-send-connection-string-to-users)\.
**Note**  
If you create your users in the WorkSpaces console instead of creating them in Active Directory, WorkSpaces automatically sends an invitation email to your users with a Region\-based registration code whenever you launch a new WorkSpace\. This means that when you set up WorkSpaces for your users in the failover Region, your users will also automatically receive emails for these failover WorkSpaces\. You will need to instruct your users to ignore emails with Region\-based registration codes\.

## Limitations<a name="cross-region-redirection-limitations"></a>
+ Cross\-Region redirection doesn't automatically check whether connections to the primary Region have failed and then fails your WorkSpaces over to another Region\. In other words, automatic failover doesn't occur\.

  To implement an automatic failover scenario, you must use some other mechanism in conjunction with cross\-Region redirection\. For example, you can use an Amazon Route 53 failover DNS routing policy paired with a Route 53 health check that monitors a CloudWatch alarm in the primary Region\. If the CloudWatch alarm in the primary Region is triggered, your DNS failover routing policy then redirects your WorkSpaces users to the WorkSpaces that you've set up for them in the failover Region\.
+ When you're using cross\-Region redirection, user data isn't persisted between WorkSpaces in different Regions\. To ensure that users can access their files from different Regions, we recommend that you set up Amazon WorkDocs Drive for your WorkSpaces users\.
+ Cross\-Region redirection is supported only on version 3\.0\.9 or later of the Linux, macOS, and Windows WorkSpaces client applications\.
+ Cross\-Region redirection is available in all [ AWS Regions where Amazon WorkSpaces is available](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/), except for the AWS GovCloud \(US\-West\) Region and the China \(Ningxia\) Region\. 

## Step 1: Create Your Connection Aliases<a name="cross-region-redirection-create-connection-aliases"></a>

Using the same AWS account, create connection aliases in each primary and failover Region where you want to set up cross\-Region redirection\.

**To create a connection alias**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. <a name="step_select_region_create_alias"></a>In the upper\-right corner of the console, select the primary AWS Region for your WorkSpaces\. 

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection**, choose **Create connection alias**\.

1. For **Connection string**, enter an FQDN, such as `www.example.com` or `desktop.example.com`\. A connection string can be a maximum of 255 characters\. It can include only letters \(A\-Z and a\-z\), numbers \(0\-9\), and the following characters: \.\-
**Important**  
After you create a connection string, it is always associated with your AWS account\. You cannot recreate the same connection string with a different account, even if you delete all instances of it from the original account\. The connection string is globally reserved for your account\.

1. \(Optional\) Under **Tags**, specify any tags that you want to associate with your connection alias\.

1. Choose **Create connection alias**\.

1. Repeat these steps, but in [Step 2](#step_select_region_create_alias), be sure to select the failover Region for your WorkSpaces\. If you have more than one failover Region, repeat these steps for each failover Region\. Be sure to use the same AWS account to create the connection alias in each failover Region\.

## \(Optional\) Step 2: Share a Connection Alias with Another Account<a name="cross-region-redirection-share-connection-alias"></a>

You can share a connection alias with one other AWS account in the same AWS Region\. Sharing a connection alias with another account gives that account permission to associate or disassociate that alias with a directory owned by that account in the same Region only\. Only the account that owns a connection alias can delete the alias\.

**Note**  
A connection alias can be associated with only one directory per AWS Region\. If you share a connection alias with another AWS account, only one account \(your account or the shared account\) can associate the alias with a directory in that Region\.

**To share a connection alias with another AWS account**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the upper\-right corner of the console, select the AWS Region where you want to share the connection alias with another AWS account\.

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection associations**, select the connection string, and then choose **Actions**, **Share/unshare connection alias**\.

   You can also share an alias from the details page for your connection alias\. To do so, under **Shared account**, choose **Share connection alias**\.

1. On the **Share/unshare connection alias** page, under **Share with an account**, enter the AWS account ID that you want to share your connection alias with in this AWS Region\.

1. Choose **Share**\.

## Step 3: Associate Your Connection Aliases with Directories in Each Region<a name="cross-region-redirection-associate-connection-aliases"></a>

Associating the same connection alias with a WorkSpaces directory in two or more Regions creates an association pair between the directories\. Each association pair has a primary Region and one or more failover Regions\. 

For example, if your primary Region is the US West \(Oregon\) Region, you can pair your WorkSpaces directory in the US West \(Oregon\) Region with a WorkSpaces directory in the US East \(N\. Virginia\) Region\. If an outage occurs in the primary Region, cross\-Region redirection works in conjunction with your DNS failover routing policies and any health checks that you've put in place on the US West \(Oregon\) Region to redirect your users to the WorkSpaces you've set up for them in the US East \(N\. Virginia\) Region\. For more information about the cross\-Region redirection experience, see [What Happens During Cross\-Region Redirection](#cross-region-redirection-what-happens)\.

**Note**  
If your WorkSpaces users are located a significant distance from the failover Region \(for example, thousands of miles away\), their WorkSpaces experience might be less responsive than usual\. To check the round\-trip time \(RTT\) to the various AWS Regions from your location, use the [Amazon WorkSpaces Connection Health Check](https://clients.amazonworkspaces.com/Health.html)\.

**To associate a connection alias with a directory**

You can associate a connection alias with only one directory per AWS Region\. If you have shared a connection alias with another AWS account, only one account \(your account or the shared account\) can associate the alias with a directory in that Region\. 

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. <a name="step_select_region_associate_alias"></a>In the upper\-right corner of the console, select the primary AWS Region for your WorkSpaces\. 

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection associations**, select the connection string, and then choose **Actions**, **Associate/disassociate**\.

   You can also associate a connection alias with a directory from the details page for your connection alias\. To do so, under **Associated directory**, choose **Associate directory**\.

1. On the **Associate/disassociate** page, Under **Associate to a directory**, select the directory that you want to associate your connection alias with in this AWS Region\.

1. Choose **Associate**\.

1. Repeat these steps, but in [Step 2](#step_select_region_associate_alias), be sure to select the failover Region for your WorkSpaces\. If you have more than one failover Region, repeat these steps for each failover Region\. Be sure to associate the same connection alias with a directory in each failover Region\.

## Step 4: Configure Your DNS Service and Set Up Your DNS Routing Policies<a name="cross-region-redirection-configure-DNS-routing"></a>

After you've created your connection aliases and your connection alias association pairs, you can then configure the DNS service for the domain that you've used in your connection strings\. You can use any DNS service provider for this purpose\. If you don't already have a preferred DNS service provider, you can use Amazon Route 53\. For more information, see [ Configuring Amazon Route 53 as your DNS service](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html) in the *Amazon Route 53 Developer Guide*\.

After you've configured the DNS service for your domain, you must set up the DNS routing policies that you want to use for cross\-Region redirection\. For example, you can use Amazon Route 53 health checks to determine whether your users can connect to their WorkSpaces in a particular Region\. If your users can't connect, you can use a DNS failover policy to route your DNS traffic from one Region to another\.

For more information about choosing your DNS routing policy, see [Choosing a routing policy](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html) in the *Amazon Route 53 Developer Guide*\. For more information about Amazon Route 53 health checks, see [ How Amazon Route 53 checks the health of your resources](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome-health-checks.html) in the *Amazon Route 53 Developer Guide*\.

When you're setting up your DNS routing policies, you will need the *connection identifier* for the association between the connection alias and the WorkSpaces directory in the primary Region\. You will also need the connection identifier for the association between the connection alias and the WorkSpaces directory in your failover Region or Regions\.

**Note**  
The connection identifier is **not** the same as the connection alias ID\. The connection alias ID starts with `wsca-`\.

**To find the connection identifier for a connection alias association**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. <a name="step_select_region_connection_id"></a>In the upper\-right corner of the console, select the primary AWS Region for your WorkSpaces\.

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection associations**, select the connection string text \(the FQDN\) to view the connection alias details page\.

1. On the details page for your connection alias, under **Associated directory**, make note of the value that's displayed for **Connection identifier**\.

1. Repeat these steps, but in [Step 2](#step_select_region_connection_id), be sure to select the failover Region for your WorkSpaces\. If you have more than one failover Region, repeat these steps to find the connection identifier for each failover Region\.

**Example: To set up a DNS failover routing policy using Route 53**

The following example sets up a public hosted zone for your domain\. However, you can set up a public or a private hosted zone\. For more information about setting up a hosted zone, see [ Working with hosted zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-working-with.html) in the *Amazon Route 53 Developer Guide*\.

This example also uses a failover routing policy\. You can use other routing policy types for your cross\-Region redirection strategy\. For more information about choosing your DNS routing policy, see [Choosing a routing policy](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html) in the *Amazon Route 53 Developer Guide*\.

When you're setting up a failover routing policy in Route 53, a health check is required for the primary Region\. For more information about creating a health check in Route 53, see [ Creating Amazon Route 53 health checks and configuring DNS failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html) and [ Creating, updating, and deleting health checks](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/health-checks-creating-deleting.html) in the *Amazon Route 53 Developer Guide*\.

If you want to use an Amazon CloudWatch alarm with your Route 53 health check, you'll also need to set up a CloudWatch alarm to monitor the resources in your primary Region\. For more information about CloudWatch, see [ What Is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) in the *Amazon CloudWatch User Guide*\. For more information about how Route 53 uses CloudWatch alarms in its health checks, see [ How Route 53 determines the status of health checks that monitor CloudWatch alarms](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-determining-health-of-endpoints.html#dns-failover-determining-health-of-endpoints-cloudwatch) and [ Monitoring a CloudWatch alarm](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/health-checks-creating-values.html#health-checks-creating-values-cloudwatch) in the *Amazon Route 53 Developer Guide*\.

To set up a DNS failover routing policy in Route 53, you first need to create a hosted zone for your domain\.

1. Open the Route 53 console at [https://console\.aws\.amazon\.com/route53/](https://console.aws.amazon.com/route53/)\.

1. In the navigation pane, choose **Hosted zones**, and then choose **Create hosted zone**\.

1. On the **Created hosted zone** page, enter your domain name \(such as `example.com`\) under **Domain name**\.

1. Under **Type**, choose **Public hosted zone**\.

1. Choose **Create hosted zone**\.

Then create a health check for your primary Region\.

1. Open the Route 53 console at [https://console\.aws\.amazon\.com/route53/](https://console.aws.amazon.com/route53/)\.

1. In the navigation pane, choose **Health checks**, and then choose **Create health check**\.

1. On the **Configure health check** page, enter a name for your health check\.

1. For **What to monitor**, select either **Endpoint**, **Status of other health checks \(calculated health check\)**, or **State of CloudWatch alarm**\.

1. Depending on what you've selected in the prior step, configure your health check, and then choose **Next**\.

1. On the **Get notified when health check fails** page, for **Create alarm**, choose **Yes** or **No**\.

1. Choose **Create health check**\.

After you've created your health check, you can create the DNS failover records\.

1. Open the Route 53 console at [https://console\.aws\.amazon\.com/route53/](https://console.aws.amazon.com/route53/)\.

1. In the navigation pane, choose **Hosted zones**\.

1. On the **Hosted zones** page, select your domain name\.

1. On the details page for your domain name, choose **Create record**\.

1. On the **Choose routing policy** page, select **Failover**, and then choose **Next**\.

1. On the **Configure records** page, under **Basic configuration**, for **Record name**, enter your subdomain name\. For example, if your FQDN is `desktop.example.com`, enter **desktop**\.
**Note**  
If you want to use the root domain, leave **Record name** blank\. However, we recommend using a subdomain, such as `desktop` or `workspaces`, unless you've set up the domain solely for use with your WorkSpaces\.

1. For **Record type**, select **TXT – Used to verify email senders and for application\-specific values**\.

1. Leave the **TTL seconds** settings at the default\. 

1. Under **Failover records to add to *your\_domain\_name***, choose **Define failover record**\.

Now you need to set up the failover records for your primary and failover Regions\.

**Example: To set up the failover record for your primary Region**

1. In the **Define failover record** dialog box, for **Value/route traffic to**, select **IP address or another value depending on the record type**\. 

1. A box opens for you to enter your sample text entries\. Enter the connection identifier for the connection alias association for your primary Region\.

1. For **Failover record type**, choose **Primary**\.

1. For **Health check**, select a health check that you've created for your primary Region\. 

1. For **Record ID**, enter a description to identify this record\. 

1. Choose **Define failover record**\. Your new failover record appears under **Failover records to add to *your\_domain\_name***\.

**Example: To set up the failover record for your failover Region**

1. Under **Failover records to add to *your\_domain\_name***, choose **Define failover record**\.

1. In the **Define failover record** dialog box, for **Value/route traffic to**, select **IP address or another value depending on the record type**\. 

1. A box opens for you to enter your sample text entries\. Enter the connection identifier for the connection alias association for your primary Region\.

1. For **Failover record type**, choose **Secondary**\.

1. \(Optional\) For **Health check**, enter a health check that you've created for your failover Region\. 

1. For **Record ID**, enter a description to identify this record\. 

1. Choose **Define failover record**\. Your new failover record appears under **Failover records to add to *your\_domain\_name***\.

If the health check that you've set up for your primary Region fails, your DNS failover routing policy redirects your WorkSpaces users to your failover Region\. Route 53 continues to monitor the health check for your primary Region, and when the health check for your primary Region no longer fails, Route 53 automatically redirects your WorkSpaces users back to their WorkSpaces in the primary Region\.

For more information about creating DNS records, see [ Creating records by using the Amazon Route 53 console](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-creating.html) in the *Amazon Route 53 Developer Guide*\. For more information about configuring DNS TXT records, see [TXT record type](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html#TXTFormat) in the *Amazon Route 53 Developer Guide*\.

## Step 5: Send the Connection String to Your WorkSpaces Users<a name="cross-region-redirection-send-connection-string-to-users"></a>

To make sure your users' WorkSpaces will be redirected as needed during an outage, you must send the connection string \(FQDN\) to your users\. If you've already issued Region\-based registration codes \(for example, `WSpdx+ABC12D`\) to your WorkSpaces users, those codes remain valid\. However, for cross\-Region redirection to work, your WorkSpaces users must use the connection string as their registration code when registering their WorkSpaces in the WorkSpaces client application\. 

**Important**  
If you create your users in the WorkSpaces console instead of creating them in Active Directory, WorkSpaces automatically sends an invitation email to your users with a Region\-based registration code \(for example, `WSpdx+ABC12D`\) whenever you launch a new WorkSpace\. Even if you've already set up cross\-Region redirection, the invitation email that's automatically sent for new WorkSpaces contains this Region\-based registration code instead of your connection string\.  
To make sure your WorkSpaces users are using the connection string instead of the Region\-based registration code, you must send them another email with the connection string by using the procedure below\. 

**To send the connection string to your WorkSpaces users**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the upper\-right corner of the console, select the primary AWS Region for your WorkSpaces\.

1. In the navigation pane, choose **WorkSpaces**\.

1. On the **WorkSpaces** page, use the search box to search for a user that you want to send an invitation to, and then select the corresponding WorkSpace from the search results\. You can select only one WorkSpace at a time\. 

1. Choose **Actions**, **Invite User**\.

1. On the **Invite Users to Their WorkSpaces** page, you will see an email template to send to your users\. 

1. \(Optional\) If there is more than one connection alias associated with your WorkSpaces directory, select the connection string that you want your users to use from the **Connection alias string** list\. The email template updates to display the string that you've chosen\.

1. Copy the email template text and paste it into an email to the users using your own email application\. In your email application, you can modify the text as needed\. When the invitation email is ready, send it to your users\.

## What Happens During Cross\-Region Redirection<a name="cross-region-redirection-what-happens"></a>

In the event of an outage, your WorkSpaces users are disconnected from their WorkSpaces in the primary Region\. When they attempt to reconnect, they receive the following error message:

`We can't connect to your WorkSpace. Check your network connection, and then try again.`

Your users are then prompted to log in again\. If they're using the FQDN as their registration code, when they log in again, your DNS failover routing policies redirect them to the WorkSpaces that you've set up for them in the failover Region\.

**Note**  
In some cases, users might be unable to reconnect when they log in again\. If this behavior occurs, they must close and restart the WorkSpaces client application, and then try to log in again\.

## Disassociate a Connection Alias from a Directory<a name="cross-region-redirection-disassociate-connection-alias"></a>

Only the account that owns a directory can disassociate a connection alias from the directory\. 

If you've shared a connection alias with another account and that account has associated the connection alias with a directory owned by that account, that account must be used to disassociate the connection alias from the directory\.

**To disassociate a connection alias from a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the upper\-right corner of the console, select the AWS Region that contains the connection alias that you want to disassociate\.

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection associations**, select the connection string, and then choose **Actions**, **Associate/disassociate**\.

   You can also dissociate a connection alias from the connection alias details page\. To do so, under **Associated directory**, choose **Disassociate**\. 

1. On the **Associate/disassociate** page, choose **Disassociate**\.

1. In the dialog box that asks you to confirm the disassociation, choose **Disassociate**\.

## Unshare a Connection Alias<a name="cross-region-redirection-unshare-connection-alias"></a>

Only the owner of a connection alias can unshare the alias\. If you unshare a connection alias with an account, that account can no longer associate the connection alias with a directory\.

**To unshare a connection alias**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the upper\-right corner of the console, select the AWS Region that contains the connection alias that you want to unshare\.

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection associations**, select the connection string, and then choose **Actions**, **Share/unshare connection alias**\.

   You can also unshare a connection alias from the connection alias details page\. To do so, under **Shared account**, choose **Unshare**\.

1. On the **Share/unshare connection alias** page, choose **Unshare**\.

1. In the dialog box that asks you to confirm unsharing the connection alias, choose **Unshare**\.

## Delete a Connection Alias<a name="cross-region-redirection-delete-connection-alias"></a>

You can delete a connection alias only if it is owned by your account and if it isn't associated with a directory\. 

If you've shared a connection alias with another account and that account has associated the connection alias with a directory owned by that account, that account must first disassociate the connection alias from the directory before you can delete the connection alias\. 

**Important**  
After you create a connection string, it is always associated to your AWS account\. You cannot recreate the same connection string with a different account, even if you delete all instances of it from the original account\. The connection string is globally reserved for your account\.

**Warning**  
**If you will no longer be using an FQDN as the registration code for your WorkSpaces users, you must take certain precautions to prevent potential security issues\.** For more information, see [Security Considerations if You Stop Using Cross\-Region Redirection](#cross-region-redirection-security-considerations)\.

**To delete a connection alias**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the upper\-right corner of the console, select the AWS Region that contains the connection alias that you want to delete\.

1. In the navigation pane, choose **Account Settings**\.

1. Under **Cross\-Region redirection associations**, select the connection string, and then choose **Delete**\.

   You can also delete a connection alias from the connection alias details page\. To do so, choose **Delete** in the upper\-right corner of the page\.
**Note**  
If the **Delete** button is disabled, make sure that you are the owner of the alias, and make sure that the alias isn't associated with a directory\.

1. In the dialog box that asks you to confirm deletion, choose **Delete**\.

## Security Considerations if You Stop Using Cross\-Region Redirection<a name="cross-region-redirection-security-considerations"></a>

If you will no longer be using an FQDN as the registration code for your WorkSpaces users, you must take the following precautions to prevent potential security issues:
+ Be sure to issue your WorkSpaces users the Region\-specific registration code \(for example, `WSpdx+ABC12D`\) for their WorkSpaces directory and instruct them to stop using the FQDN as their registration code\.
+ **If you still own this domain**, be sure to update your DNS TXT record to remove this domain so that it cannot be exploited in a phishing attack\. If you remove this domain from your DNS TXT record and your WorkSpaces users attempt to use the FQDN as their registration code, their connection attempts will fail harmlessly\. 
+ **If you no longer own this domain**, your WorkSpaces users **must** use their Region\-specific registration code\. If they continue trying to use the FQDN as their registration code, their connection attempts could be redirected to a malicious site\.