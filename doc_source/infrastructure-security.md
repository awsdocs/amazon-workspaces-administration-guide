# Infrastructure Security in Amazon WorkSpaces<a name="infrastructure-security"></a>

As a managed service, Amazon WorkSpaces is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access Amazon WorkSpaces through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

## Network Isolation<a name="network-isolation"></a>

A virtual private cloud \(VPC\) is a virtual network in your own logically isolated area in the AWS cloud\. You can deploy your WorkSpaces in a private subnet in your VPC\. For more information, see [Configure a VPC for Amazon WorkSpaces](amazon-workspaces-vpc.md)\.

To allow traffic only from specific address ranges \(for example, from your corporate network\), update the security group for your VPC or use an [IP access control group](amazon-workspaces-ip-access-control-groups.md)\.

You can restrict WorkSpace access to trusted devices with valid certificates\. For more information, see [Restrict WorkSpaces Access to Trusted Devices](trusted-devices.md)\.

## Isolation on Physical Hosts<a name="physical-isolation"></a>

Different WorkSpaces on the same physical host are isolated from each other through the hypervisor\. It is as though they are on separate physical hosts\. When a WorkSpace is deleted, the memory allocated to it is scrubbed \(set to zero\) by the hypervisor before it is allocated to a new WorkSpace\.

## Authorization of Corporate Users<a name="authorization"></a>

With Amazon WorkSpaces, directories are managed through the AWS Directory Service\. You can create a standalone, managed directory for users, or integrate with your existing Active Directory environment so that your users can use their current credentials to obtain seamless access to corporate resources\. For more information, see [Manage Directories for Amazon WorkSpaces](manage-workspaces-directory.md)\.

To further control access to your WorkSpaces, use multi\-factor authentication\. For more information, see [How to Enable Multi\-Factor Authentication for AWS Services](http://aws.amazon.com/blogs/security/how-to-enable-multi-factor-authentication-for-amazon-workspaces-and-amazon-quicksight-by-using-microsoft-ad-and-on-premises-credentials/)\.