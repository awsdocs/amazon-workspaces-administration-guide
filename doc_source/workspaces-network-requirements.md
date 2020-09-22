# Amazon WorkSpaces Client Network Requirements<a name="workspaces-network-requirements"></a>

Your Amazon WorkSpaces users can connect to their WorkSpaces by using the client application for a supported device\. Alternatively, they can use a web browser to connect to WorkSpaces that support this form of access\. For a list of WorkSpaces that support web browser access, see "Which Amazon WorkSpaces bundles support web access?" in [ Client Access, Web Access, and User Experience](https://aws.amazon.com/workspaces/faqs/#Client_Access.2C_Web_Access.2C_and_User_Experience)\.

**Note**  
A web browser cannot be used to connect to Amazon Linux WorkSpaces\.

**Important**  
Beginning October 1, 2020, customers will no longer be able to use the Amazon WorkSpaces Web Access client to connect to Windows 7 custom WorkSpaces or to Windows 7 Bring Your Own License \(BYOL\) WorkSpaces\.

To provide your users with a good experience with their WorkSpaces, verify that their client devices meet the following network requirements:
+ The client device must have a broadband internet connection\. We recommend planning for a minimum of 1 Mbps per simultaneous user watching a 480p video window\. Depending on your user\-quality requirements for video resolution, more bandwidth might be required\.
+ The network that the client device is connected to, and any firewall on the client device, must have certain ports open to the IP address ranges for various AWS services\. For more information, see [IP Address and Port Requirements for Amazon WorkSpaces](workspaces-port-requirements.md)\.
+ For the best performance, the round trip time \(RTT\) from the client's network to the Region that the WorkSpaces are in should be less than 100ms\. If the RTT is between 100ms and 200ms, the user can access the WorkSpace, but performance is affected\. If the RTT is between 200ms and 375ms, the performance is degraded\. If the RTT exceeds 375ms, the WorkSpaces client connection is terminated\.

  To check the RTT to the various AWS Regions from your location, use the [Amazon WorkSpaces Connection Health Check](https://clients.amazonworkspaces.com/Health.html)\.
+ If users will access their WorkSpaces through a virtual private network \(VPN\), the connection must support a maximum transmission unit \(MTU\) of at least 1200 bytes\.
**Note**  
You cannot access WorkSpaces through a VPN connected to your virtual private cloud \(VPC\)\. To access WorkSpaces using a VPN, internet connectivity \(through the VPN's public IP addresses\) is required, as described in [IP Address and Port Requirements for Amazon WorkSpaces](workspaces-port-requirements.md)\.
+ The clients require HTTPS access to Amazon WorkSpaces resources hosted by the service and Amazon Simple Storage Service \(Amazon S3\)\. The clients do not support proxy redirection at the application level\. HTTPS access is required so that users can successfully complete registration and access their WorkSpaces\.
+ To allow access from PCoIP zero client devices, you must launch and configure an EC2 instance with PCoIP Connection Manager for Amazon WorkSpaces\. For more information, see *Deploying the PCoIP Connection Manager for Amazon WorkSpaces* in the [PCoIP Connection Manager User Guide](https://www.teradici.com/web-help/Connecting_ZC_AWS_HTML5/TER1408002_Connecting_ZC_AWS.htm)\. You must also enable Network Time Protocol \(NTP\) in Teradici\. For more information, see [Set Up PCoIP Zero Client for WorkSpaces](set-up-pcoip-zero-client.md)\.
+ For 3\.0\+ clients, if you are using single sign\-on \(SSO\) for Amazon WorkDocs, you must follow the instructions in [ Single Sign\-On](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_single_sign_on.html) in the *AWS Directory Service Administration Guide*\.

You can verify that a client device meets the networking requirements as follows\.

## To verify networking requirements for 3\.0\+ clients<a name="verify-requirements-new-clients"></a>

1. Open your Amazon WorkSpaces client\. If this is the first time you have opened the client, you are prompted to enter the registration code that you received in the invitation email\.

1. Depending on which client you're using, do one of the following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-network-requirements.html)

   The client application tests the network connection, ports, and round\-trip time, and reports the results of these tests\.

1. Close the **Network** dialog box to return to the sign\-in page\.

## To verify networking requirements for 1\.0\+ and 2\.0\+ clients<a name="verify-requirements-legacy-clients"></a>

1. Open your Amazon WorkSpaces client\. If this is the first time you have opened the client, you are prompted to enter the registration code that you received in the invitation email\.

1. Choose **Network** in the lower\-right corner of the client application\. The client application tests the network connection, ports, and round\-trip time, and reports the results of these tests\.

1. Choose **Dismiss** to return to the sign\-in page\.