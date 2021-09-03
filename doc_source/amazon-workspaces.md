# What is Amazon WorkSpaces?<a name="amazon-workspaces"></a>

Amazon WorkSpaces enables you to provision virtual, cloud\-based Microsoft Windows or Amazon Linux desktops for your users, known as *WorkSpaces*\. WorkSpaces eliminates the need to procure and deploy hardware or install complex software\. You can quickly add or remove users as your needs change\. Users can access their virtual desktops from multiple devices or web browsers\.

For more information, see [Amazon WorkSpaces](https://aws.amazon.com/workspaces/)\.

## Features<a name="features"></a>
+ Choose your operating system \(Windows or Amazon Linux\) and select from a range of hardware configurations, software configurations, and AWS Regions\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles) and [Create a custom WorkSpaces image and bundle](create-custom-bundle.md)\.
+ Choose your protocol: PCoIP or WorkSpaces Streaming Protocol \(WSP\)\. For more information, see [Protocols for Amazon WorkSpaces](amazon-workspaces-protocols.md)\.
+ Connect to your WorkSpace and pick up from right where you left off\. WorkSpaces provides a persistent desktop experience\.
+ WorkSpaces provides the flexibility of either monthly or hourly billing for WorkSpaces\. For more information, see [WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.
+ Deploy and manage applications for your Windows WorkSpaces by using Amazon WorkSpaces Application Manager \(Amazon WAM\)\.
+ For Windows desktops, you can bring your own licenses and applications, or purchase them from the AWS Marketplace for Desktop Apps\.
+ Create a standalone managed directory for your users, or connect your WorkSpaces to your on\-premises directory so that your users can use their existing credentials to obtain seamless access to corporate resources\. For more information, see [Manage directories for WorkSpaces](manage-workspaces-directory.md)\.
+ Use the same tools to manage WorkSpaces that you use to manage on\-premises desktops\.
+ Use multi\-factor authentication \(MFA\) for additional security\.
+ Use AWS Key Management Service \(AWS KMS\) to encrypt data at rest, disk I/O, and volume snapshots\.
+ Control the IP addresses from which users are allowed to access their WorkSpaces\.

## Architecture<a name="architecture"></a>

For both Windows and Amazon Linux WorkSpaces, each WorkSpace is associated with a virtual private cloud \(VPC\), and a directory to store and manage information for your WorkSpaces and users\. For more information, see [Configure a VPC for WorkSpaces](amazon-workspaces-vpc.md)\. Directories are managed through the AWS Directory Service, which offers the following options: Simple AD, AD Connector, or AWS Directory Service for Microsoft Active Directory, also known as AWS Managed Microsoft AD\. For more information, see the [AWS Directory Service Administration Guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

WorkSpaces uses your Simple AD, AD Connector, or AWS Managed Microsoft AD directory to authenticate users\. Users access their WorkSpaces by using a client application from a supported device or, for Windows WorkSpaces, a web browser, and they log in by using their directory credentials\. The login information is sent to an authentication gateway, which forwards the traffic to the directory for the WorkSpace\. After the user is authenticated, streaming traffic is initiated through the streaming gateway\.

Client applications use HTTPS over port 443 for all authentication and session\-related information\. Client applications use port 4172 \(PCoIP\) and port 4195 \(WSP\) for pixel streaming to the WorkSpace and ports 4172 and 4195 for network health checks\. For more information, see [Ports for client applications](workspaces-port-requirements.md#client-application-ports)\.

Each WorkSpace has two elastic network interfaces associated with it: a network interface for management and streaming \(eth0\) and a primary network interface \(eth1\)\. The primary network interface has an IP address provided by your VPC, from the same subnets used by the directory\. This ensures that traffic from your WorkSpace can easily reach the directory\. Access to resources in the VPC is controlled by the security groups assigned to the primary network interface\. For more information, see [Network interfaces](workspaces-port-requirements.md#network-interfaces)\.

The following diagram shows the architecture of WorkSpaces\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/architectural-diagram-new-2.png)

For additional architecture diagrams, see the [ *Best Practices for Deploying Amazon WorkSpaces*](https://d1.awsstatic.com/whitepapers/Best-Practices-for-Deploying-Amazon-WorkSpaces.pdf) whitepaper\.

## Access your WorkSpace<a name="devices"></a>

You can connect to your WorkSpaces by using the client application for a supported device or, for Windows WorkSpaces, by using a supported web browser on a supported operating system\.

**Note**  
You cannot use a web browser to connect to Amazon Linux WorkSpaces\.

There are client applications for the following devices:
+ Windows computers
+ macOS computers
+ Ubuntu Linux 18\.04 computers
+ Chromebooks
+ iPads
+ Android devices
+ Fire tablets
+ Zero client devices \(Teradici zero client devices are supported only with PCoIP\.\)

On Windows, macOS, and Linux PCs, you can use the following web browsers to connect to Windows WorkSpaces:
+ Chrome 53 and later \(Windows and macOS only\)
+ Firefox 49 and later

For more information, see [WorkSpaces Clients](https://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide*\.

## Pricing<a name="pricing"></a>

After you sign up for AWS, you can get started with WorkSpaces for free using the WorkSpaces free tier offer\. For more information, see [WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

With WorkSpaces, you pay only for what you use\. You are charged based on the bundle and the number of WorkSpaces that you launch\. The pricing for WorkSpaces includes the use of Simple AD and AD Connector but not the use of AWS Managed Microsoft AD\.

WorkSpaces provides monthly or hourly billing for WorkSpaces\. With monthly billing, you pay a fixed fee for unlimited usage, which is best for users who use their WorkSpaces full time\. With hourly billing, you pay a small fixed monthly fee per WorkSpace, plus a low hourly rate for each hour the WorkSpace is running\. For more information, see [WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

For information about supported regions, see [WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

## How to get started<a name="how-to-start"></a>

To create a WorkSpace, try one of the following tutorials:
+ [Get started with WorkSpaces Quick Setup](getting-started.md)
+ [Launch a WorkSpace using AWS Managed Microsoft AD](launch-workspace-microsoft-ad.md)
+ [Launch a WorkSpace using Simple AD](launch-workspace-simple-ad.md)
+ [Launch a WorkSpace using AD Connector](launch-workspace-ad-connector.md)
+ [Launch a WorkSpace using a trusted domain](launch-workspace-trusted-domain.md)

You might also want to explore these resources to learn more about Amazon WorkSpaces: 
+ [ Implementation guide: Provision Desktops in the Cloud](http://aws.amazon.com/getting-started/hands-on/provision-cloud-desktops/)
+ [ Amazon WorkSpaces resources](http://aws.amazon.com/workspaces/resources/) — whitepapers, blog posts, webinars, re:Invent sessions, and more
+ [Amazon WorkSpaces FAQs](http://aws.amazon.com/workspaces/faqs/)