# Provide Internet Access from Your WorkSpace<a name="amazon-workspaces-internet-access"></a>

We recommend that you launch your WorkSpaces in private subnets in your virtual private cloud \(VPC\) and use one of the following options to allow your WorkSpaces to access the internet:

**Options**
+ Configure a NAT gateway in your VPC\. For more information, see [Configure a VPC for Amazon WorkSpaces](amazon-workspaces-vpc.md)\.
+ Configure automatic assignment of public IP addresses\. For more information, see [Configure Automatic IP Addresses](update-directory-details.md#automatic-assignment)\.
+ Manually assign public IP addresses to your WorkSpaces\. For more information, see [Manually Assign IP Addresses](#manual-assignment)\.

With any of these options, you must ensure that the security group for your WorkSpaces allows outbound traffic on ports 80 \(HTTP\) and 443 \(HTTPS\) to all destinations \(`0.0.0.0/0`\)\.

**Amazon WAM**  
If you are using Amazon WorkSpaces Application Manager \(Amazon WAM\) to deploy applications to your WorkSpaces, your WorkSpaces must have access to the internet\.

**Amazon Linux Extras Library**  
If you are using the Amazon Linux repository, your Amazon Linux WorkSpaces must either have internet access or you must configure VPC endpoints to this repository and to the main Amazon Linux repository\. For more information, see the *Example: Enabling Access to the Amazon Linux AMI Repositories* section in [Endpoints for Amazon S3](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html)\. The Amazon Linux AMI repositories are Amazon S3 buckets in each region\. If you want instances in your VPC to access the repositories through an endpoint, create an endpoint policy that enables access to these buckets\. The following policy allows access to the Amazon Linux repositories\.

```
{
  {
"Statement": [
{
"Sid": "AmazonLinux2AMIRepositoryAccess",
"Principal": "",
"Action": [
"s3:GetObject"
],
"Effect": "Allow",
"Resource": [
"arn:aws:s3:::amazonlinux..amazonaws.com/*"
]
}
]
}
}
```

## Manually Assign IP Addresses<a name="manual-assignment"></a>

You can manually assign an Elastic IP address to a WorkSpace\.

**Prerequisites**
+ Your VPC must have an attached internet gateway\. For more information, see [Attaching an Internet Gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon VPC User Guide*\.
+ The route table for the WorkSpaces subnets must have one route for local traffic and another route that sends all other traffic to the internet gateway\.

**To assign an Elastic IP address to a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **WorkSpaces**\.

1. Expand the row for the WorkSpace and note the value of **WorkSpace IP**\. This is the primary private IP address of WorkSpace\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Elastic IPs**\. If you do not have an available Elastic IP address, choose **Allocate new address** and follow the directions\.

1. In the navigation pane, choose **Network Interfaces**\.

1. Select the network interface for your WorkSpace\. Note that the value of **VPC ID** matches the ID of your WorkSpaces VPC and the value of **Primary private IPv4 IP** matches the primary private IP address of the WorkSpace that you noted earlier\.

1. Choose **Actions**, **Associate Address**\.

1. On the **Associate Elastic IP Address** page, choose an Elastic IP address from **Address** and then choose **Associate Address**\.