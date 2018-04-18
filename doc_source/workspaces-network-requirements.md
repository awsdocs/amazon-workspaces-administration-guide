# Amazon WorkSpaces Client Network Requirements<a name="workspaces-network-requirements"></a>

Your Amazon WorkSpaces users access their WorkSpaces using the client application for a supported device or a web browser\. To provide your users with a good experience with their WorkSpaces, verify that their client devices meet the following network requirements:
+ The client device must have a broadband Internet connection\.
+ The network that the client device is connected to, and any firewall on the client device, must have certain ports open to the IP address ranges for various AWS services\. For more information, see [Port Requirements for Amazon WorkSpaces](workspaces-port-requirements.md)\.
+ The round trip time \(RTT\) from the client's network to the region that the WorkSpaces are in should be less than 100ms\. If the RTT is between 100ms and 250ms, the user can access the WorkSpace but performance is degraded\.
+ If users will access their WorkSpaces through a virtual private network \(VPN\), the connection must support a maximum transmission unit \(MTU\) of at least 1200 bytes\.
+ The clients require HTTPS access to Amazon WorkSpaces resources hosted by the service and Amazon Simple Storage Service \(Amazon S3\)\. The client do not support proxy redirection at the application level\. HTTPS access is required so that users can successfully complete registration and access their WorkSpaces\.
+ To allow access from PCoIP zero client devices, you must launch and configure an EC2 instance with PCoIP Connection Manager for Amazon WorkSpaces\. For more information, see *Deploying the PCoIP Connection Manager for Amazon WorkSpaces* in the [PCoIP Connection Manager User Guide](http://www.teradici.com/web-help/Connecting_ZC_AWS_HTML5/TER1408002_Connecting_ZC_AWS.htm)\.

You can verify that a client device meets the networking requirements as follows\.

**To verify client networking requirements**

1. Open the Amazon WorkSpaces client\. If this is the first time you have opened the client, you are prompted to type the registration code that you received in the invitation email\.

1. Choose **Network** in the lower right corner of the client application\. The client application tests the network connection, ports, and round trip time and reports the results of these tests\.

1. Choose **Dismiss** to return to the sign in page\.