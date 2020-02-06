# Infrastructure Security in Amazon WorkSpaces<a name="infrastructure-security"></a>

As a managed service, Amazon WorkSpaces is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access Amazon WorkSpaces through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

## Network Isolation<a name="network-isolation"></a>

A virtual private cloud \(VPC\) is a virtual network in your own logically isolated area in the AWS Cloud\. You can deploy your WorkSpaces in a private subnet in your VPC\. For more information, see [Configure a VPC for Amazon WorkSpaces](amazon-workspaces-vpc.md)\.

To allow traffic only from specific address ranges \(for example, from your corporate network\), update the security group for your VPC or use an [IP access control group](amazon-workspaces-ip-access-control-groups.md)\.

You can restrict WorkSpace access to trusted devices with valid certificates\. For more information, see [Restrict WorkSpaces Access to Trusted Devices](trusted-devices.md)\.

## Isolation on Physical Hosts<a name="physical-isolation"></a>

Different WorkSpaces on the same physical host are isolated from each other through the hypervisor\. It is as though they are on separate physical hosts\. When a WorkSpace is deleted, the memory allocated to it is scrubbed \(set to zero\) by the hypervisor before it is allocated to a new WorkSpace\.

## Authorization of Corporate Users<a name="authorization"></a>

With Amazon WorkSpaces, directories are managed through the AWS Directory Service\. You can create a standalone, managed directory for users\. Or you can integrate with your existing Active Directory environment so that your users can use their current credentials to obtain seamless access to corporate resources\. For more information, see [Manage Directories for Amazon WorkSpaces](manage-workspaces-directory.md)\.

To further control access to your WorkSpaces, use multi\-factor authentication\. For more information, see [How to Enable Multi\-Factor Authentication for AWS Services](http://aws.amazon.com/blogs/security/how-to-enable-multi-factor-authentication-for-amazon-workspaces-and-amazon-quicksight-by-using-microsoft-ad-and-on-premises-credentials/)\.

## Make Amazon WorkSpaces API Requests Through a VPC Interface Endpoint<a name="interface-vpc-endpoint"></a>

You can connect directly to Amazon WorkSpaces API endpoints through an [interface endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in your virtual private cloud \(VPC\) instead of connecting over the internet\. When you use a VPC interface endpoint, communication between your VPC and the Amazon WorkSpaces API endpoint is conducted entirely and securely within the AWS network\. 

**Note**  
This feature can be used only for connecting to WorkSpaces API endpoints\. To connect to WorkSpaces using the WorkSpaces clients, internet connectivity is required, as described in [IP Address and Port Requirements for Amazon WorkSpaces](workspaces-port-requirements.md)\.

The Amazon WorkSpaces API endpoints support [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) \(Amazon VPC\) interface endpoints that are powered by [AWS PrivateLink](https://aws.amazon.com/privatelink/)\. Each VPC endpoint is represented by one or more [network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) \(also known as elastic network interfaces, or ENIs\) with private IP addresses in your VPC subnets\.

The VPC interface endpoint connects your VPC directly to the Amazon WorkSpaces API endpoint without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. The instances in your VPC don't need public IP addresses to communicate with the Amazon WorkSpaces API endpoint\.

You can create an interface endpoint to connect to Amazon WorkSpaces with either the AWS console or AWS Command Line Interface \(AWS CLI\) commands\. For instructions, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html#create-interface-endpoint)\.

*After you have created a VPC endpoint*, you can use the following example CLI commands that use the `endpoint-url` parameter to specify interface endpoints to the Amazon WorkSpaces API endpoint:

```
aws workspaces copy-workspace-image --endpoint-url VPC_Endpoint_ID.workspaces.Region.vpce.amazonaws.com

aws workspaces delete-workspace-image --endpoint-url VPC_Endpoint_ID.api.workspaces.Region.vpce.amazonaws.com

aws workspaces describe-workspace-bundles --endpoint-url VPC_Endpoint_ID.workspaces.Region.vpce.amazonaws.com  \
   --endpoint-name Endpoint_Name \
   --body "Endpoint_Body" \
   --content-type "Content_Type" \
       Output_File
```

If you enable private DNS hostnames for your VPC endpoint, you don't need to specify the endpoint URL\. The Amazon WorkSpaces API DNS hostname that the CLI and Amazon WorkSpaces SDK use by default \(https://api\.workspaces\.*Region*\.amazonaws\.com\) resolves to your VPC endpoint\.

The Amazon WorkSpaces API endpoint supports VPC endpoints in all AWS Regions where both [Amazon VPC](https://docs.aws.amazon.com/general/latest/gr/rande.html#vpc_region) and [Amazon WorkSpaces](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services) are available\. Amazon WorkSpaces supports making calls to all of its [public APIs](https://docs.aws.amazon.com/workspaces/latest/api/welcome.html) inside your VPC\.

To learn more about AWS PrivateLink, see the [AWS PrivateLink documentation](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink)\. For the price of VPC endpoints, see [VPC Pricing](https://aws.amazon.com/vpc/pricing/)\. To learn more about VPC and endpoints, see [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)\.

To see a list of Amazon WorkSpaces API endpoints by Region, see [WorkSpaces API Endpoints](workspaces-port-requirements.md#workspaces_api_endpoints)\.

**Note**  
Amazon WorkSpaces API endpoints with AWS PrivateLink are not supported for Federal Information Processing Standard \(FIPS\) Amazon WorkSpaces API endpoints\.

## Create a VPC Endpoint Policy for Amazon WorkSpaces<a name="api-private-link-policy"></a>

You can create a policy for Amazon VPC endpoints for Amazon WorkSpaces to specify the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

**Note**  
VPC endpoint policies aren't supported for Federal Information Processing Standard \(FIPS\) Amazon WorkSpaces endpoints\.

The following example VPC endpoint policy specifies that all users who have access to the VPC interface endpoint are allowed to invoke the Amazon WorkSpaces hosted endpoint named `ws-f9abcdefg`\.

```
{
     "Statement": [
         {
             "Action": "workspaces:*",
             "Effect": "Allow",
             "Resource": "arn:aws:workspaces:us-west-2:1234567891011:workspace/ws-f9abcdefg",
             "Principal": "*"
         }
     ]
}
```

In this example, the following actions are denied:
+ Invoking Amazon WorkSpaces hosted endpoints other than `ws-f9abcdefg`\.
+ Performing an action on any resource besides the one specified \(WorkSpace ID: `ws-f9abcdefg`\)\.

**Note**  
In this example, users can still take other Amazon WorkSpaces API actions from outside the VPC\. To restrict API calls to those from within the VPC, see [Identity and Access Management for Amazon WorkSpaces](workspaces-access-control.md) for information about using identity\-based policies to control access to Amazon WorkSpaces API endpoints\.

## Connect Your Private Network to Your VPC<a name="notebook-private-link-vpn"></a>

To call the Amazon WorkSpaces API through your VPC, you have to connect from an instance that is inside the VPC, or connect your private network to your VPC by using an Amazon Virtual Private Network \(VPN\) or AWS Direct Connect\. For information about Amazon VPN, see [VPN Connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html) in the *Amazon Virtual Private Cloud User Guide*\. For information about AWS Direct Connect, see [Creating a Connection](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-connection.html) in the *AWS Direct Connect User Guide*\.