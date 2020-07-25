# Set Up PCoIP Zero Client for WorkSpaces<a name="set-up-pcoip-zero-client"></a>

If your zero client device has firmware version 6\.0\.0 or later, your users can connect to their WorkSpaces directly\. Otherwise, if the firmware is between 4\.6\.0 and 6\.0\.0, you must set up Teradici PCoIP Connection Manager for Amazon WorkSpaces and provide your users with server URIs to connect to their WorkSpaces through Teradici PCoIP Connection Manager for Amazon WorkSpaces\.

To set up PCoIP Connection Manager for Amazon WorkSpaces on an EC2 instance, go to [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B00O9E9JZ0) and find an Amazon Machine Image \(AMI\) that you can use to launch an instance with PCoIP Connection Manager\. For more information, see *Deploying the PCoIP Connection Manager for Amazon WorkSpaces* in the [PCoIP Connection Manager User Guide](https://www.teradici.com/web-help/Connecting_ZC_AWS_HTML5/TER1408002_Connecting_ZC_AWS.htm)\.

**Note**  
In the Teradici PCoIP Administrative Web Interface \(AWI\) or the Teradici PCoIP Management Console \(MC\), make sure you enable Network Time Protocol \(NTP\)\. For the NTP host DNS name, use **pool\.ntp\.org**, and set the NTP host port to **123**\. If NTP isn't enabled, your PCoIP zero client users might receive certificate failure errors, such as "The supplied certificate is invalid due to timestamp\."

For information about setting up and connecting with a PCoIP zero client device, see [PCoIP Zero Client](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-pcoip-zero-client.html) in the *Amazon WorkSpaces User Guide*\. For a list of approved PCoIP zero client devices, see [PCoIP Zero Clients](https://www.teradici.com/product-service-finder/pcoip-zero-clients) on the Teradici website\.

Thin clients aren't supported for use with Amazon WorkSpaces\.