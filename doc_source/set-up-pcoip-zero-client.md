# Set up PCoIP zero clients for WorkSpaces<a name="set-up-pcoip-zero-client"></a>

PCoIP zero clients are compatible only with WorkSpaces bundles that are using the PCoIP protocol\.

If your zero client device has firmware version 6\.0\.0 or later, your users can connect to their WorkSpaces directly\. When your users are connecting directly to their WorkSpaces using a zero client device, we recommend using multi\-factor authentication \(MFA\) with your WorkSpaces directory\. For more information about using MFA with your directory, see the following documentation:
+ **AWS Managed Microsoft AD** — [ Enable multi\-factor authentication for AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_mfa.html) in the *AWS Directory Service Administration Guide*
+ **AD Connector** — [ Enable multi\-factor authentication for AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_mfa.html) in the *AWS Directory Service Administration Guide* and [Multi\-factor authentication \(AD Connector\)](update-directory-details.md#connect-mfa)
+ **Trusted domains** — [ Enable multi\-factor authentication for AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_mfa.html) in the *AWS Directory Service Administration Guide*
+ **Simple AD** — Multi\-factor authentication is not available for Simple AD\.

As of April 13, 2021, PCoIP Connection Manager is no longer supported for use with zero client device firmware versions between 4\.6\.0 and 6\.0\.0\. If your zero client firmware is not version 6\.0\.0 or later, you can get the latest firmware through a Desktop Access subscription at [https://www\.teradici\.com/desktop\-access](https://www.teradici.com/desktop-access)\.

**Important**  
In the Teradici PCoIP Administrative Web Interface \(AWI\) or the Teradici PCoIP Management Console \(MC\), make sure you enable Network Time Protocol \(NTP\)\. For the NTP host DNS name, use **pool\.ntp\.org**, and set the NTP host port to **123**\. If NTP isn't enabled, your PCoIP zero client users might receive certificate failure errors, such as "The supplied certificate is invalid due to timestamp\."
Starting with version 20\.10\.4 of the PCoIP agent, Amazon WorkSpaces disables USB redirection by default through the Windows registry\. This registry setting affects the behavior of USB peripherals when your users are using PCoIP zero client devices to connect to their WorkSpaces\. For more information, see [USB printers and other USB peripherals aren't working for PCoIP zero clients](amazon-workspaces-troubleshooting.md#pcoip_zero_client_usb)\.

For information about setting up and connecting with a PCoIP zero client device, see [PCoIP Zero Client](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-pcoip-zero-client.html) in the *Amazon WorkSpaces User Guide*\. For a list of approved PCoIP zero client devices, see [PCoIP Zero Clients](https://www.teradici.com/resource-center/product-service-finder/pcoip-zero-clients) on the Teradici website\.