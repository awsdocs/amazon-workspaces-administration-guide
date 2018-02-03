# What Is Amazon WorkSpaces?<a name="amazon-workspaces"></a>

Amazon WorkSpaces enables you to provision virtual, cloud\-based Microsoft Windows desktops for your users, known as *WorkSpaces*\. Amazon WorkSpaces eliminates the need to procure and deploy hardware or install complex software\. You can quickly add or remove users as your needs change\. Users can access their virtual desktops from multiple devices or web browsers\.

For more information, see [Amazon WorkSpaces](https://aws.amazon.com/workspaces/)\.

## Features<a name="features"></a>

+ Select from a range of hardware configurations, software configurations, and AWS regions\. For more information, see [Amazon WorkSpaces Bundles](https://aws.amazon.com/workspaces/details/#Amazon_WorkSpaces_Bundles)\.

+ Connect to your WorkSpace and pick up from right where you left off\. Amazon WorkSpaces provides a persistent desktop experience\.

+ Amazon WorkSpaces provides the flexibility of either monthly or hourly billing for WorkSpaces\. For more information, see [Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

+ Deploy and manage applications for your WorkSpaces using Amazon WorkSpaces Application Manager \(Amazon WAM\)\.

+ Bring your own licenses and applications, or purchase them from the AWS Marketplace for Desktop Apps\.

+ Create a standalone managed directory for your users, or connect your WorkSpaces to your on\-premises directory so that your users can use their existing credentials to obtain seamless access to corporate resources\.

+ Use the same tools to manage WorkSpaces that you use to manage on\-premises desktops\.

+ Use multi\-factor authentication \(MFA\) for additional security\.

+ Use AWS Key Management Service \(AWS KMS\) to encrypt data at rest, disk I/O, and volume snapshots\.

## Architecture<a name="architecture"></a>

Each WorkSpace is associated with the virtual private cloud \(VPC\), and a directory to store and manage information for your WorkSpaces and users\. Directories are managed through the AWS Directory Service, which offers the following options: Simple AD, AD Connector, or AWS Directory Service for Microsoft Active Directory \(Enterprise Edition\), also known as Microsoft AD\. For more information, see the [AWS Directory Service Administration Guide](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Amazon WorkSpaces uses a directory, either AWS Directory Service or Microsoft AD, to authenticate users\. Users access their WorkSpaces using a client application from a supported device or a web browser and log in using their directory credentials\. The login information is sent to an authentication gateway, which forwards the traffic to the directory for the WorkSpace\. After the user is authenticated, streaming traffic is initiated through the streaming gateway\.

Client applications use HTTPS over port 443 for all authentication and session\-related information\. Client applications uses port 4172 for pixel streaming to the WorkSpace and for network health checks\. For more information, see [Ports for Client Applications](workspaces-port-requirements.md#client-application-ports)\.

Each WorkSpace has two elastic network interfaces \(ENI\) associated with it: an ENI for management and streaming \(eth0\) and a primary ENI \(eth1\)\. The primary ENI has an IP address provided by your VPC, from the same subnets used by the directory\. This ensures that traffic from your WorkSpace can easily reach the directory\. Access to resources in the VPC is controlled by the security groups assigned to the primary ENI\. For more information, see [Network Interfaces](workspaces-port-requirements.md#network-interfaces)\.

The following diagram shows the architecture of Amazon WorkSpaces\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/images/architectural_diagram.png)

## Accessing Your WorkSpace<a name="devices"></a>

You can connect to your WorkSpaces using the client application for a supported device or using a supported web browser on a supported operating system\.

There are client applications for the following devices:

+ Windows computers

+ Mac computers

+ Chromebooks

+ iPads

+ Android tablets

+ Fire tablets

+ Zero client devices

The following web browsers are supported on Windows, macOS, and Linux:

+ Chrome 53 and later

+ Firefox 49 and later

For more information, see [Amazon WorkSpaces Clients](http://docs.aws.amazon.com/workspaces/latest/userguide/amazon-workspaces-clients.html) in the *Amazon WorkSpaces User Guide*\.

## Pricing<a name="pricing"></a>

After you sign up for AWS, you can get started with Amazon WorkSpaces for free using the Amazon WorkSpaces free tier offer\. For more information, see [Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

With Amazon WorkSpaces, you pay only for what you use\. You are charged based on the bundle and the number of WorkSpaces that you launch\. The pricing for Amazon WorkSpaces includes the use of Simple AD and AD Connector but not the use of Microsoft AD\.

Amazon WorkSpaces provides monthly or hourly billing for WorkSpaces\. With monthly billing, you pay a fixed fee for unlimited usage, which is best for users who use their WorkSpaces full time\. With hourly billing, you pay a small fixed monthly fee per WorkSpace, plus a low hourly rate for each hour the WorkSpace is running\. For more information, see [Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

## How to Get Started<a name="how-to-start"></a>

To create a WorkSpace, try one of the following tutorials:

+ [Get Started with Amazon WorkSpaces Quick Setup](getting-started.md)

+ [Launch a WorkSpace Using Microsoft AD](launch-workspace-microsoft-ad.md)

+ [Launch a WorkSpace Using Simple AD](launch-workspace-simple-ad.md)

+ [Launch a WorkSpace Using AD Connector](launch-workspace-ad-connector.md)

+ [Launch a WorkSpace Using a Trusted Domain](launch-workspace-trusted-domain.md)