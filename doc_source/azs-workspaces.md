# Availability Zones for Amazon WorkSpaces<a name="azs-workspaces"></a>

When you are creating a virtual private cloud \(VPC\) for use with Amazon WorkSpaces, your VPC's subnets must reside in different Availability Zones in the Region where you're launching WorkSpaces\. Availability Zones are distinct locations that are engineered to be isolated from failures in other Availability Zones\. By launching instances in separate Availability Zones, you can protect your applications from the failure of a single location\. Each subnet must reside entirely within one Availability Zone and cannot span zones\.

An Availability Zone is represented by a Region code followed by a letter identifier; for example, `us-east-1a`\. To ensure that resources are distributed across the Availability Zones for a Region, we independently map Availability Zones to names for each AWS account\. For example, the Availability Zone `us-east-1a` for your AWS account might not be the same location as `us-east-1a` for another AWS account\.

To coordinate Availability Zones across accounts, you must use the *AZ ID*, which is a unique and consistent identifier for an Availability Zone\. For example, `use1-az2` is an AZ ID for the `us-east-1` Region and it has the same location in every AWS account\. 

Viewing AZ IDs enables you to determine the location of resources in one account relative to the resources in another account\. For example, if you share a subnet in the Availability Zone with the AZ ID `use1-az2` with another account, this subnet is available to that account in the Availability Zone whose AZ ID is also `use1-az2`\. The AZ ID for each VPC and subnet is displayed in the Amazon VPC console\.

Amazon WorkSpaces is available in a subset of the Availability Zones for each supported Region\. The following table lists the AZ IDs that you can use for each Region\. To see the mapping of AZ IDs to Availability Zones in your account, see [ AZ IDs for Your Resources](https://docs.aws.amazon.com/ram/latest/userguide/working-with-az-ids.html) in the *AWS RAM User Guide*\.


| Region name | Region code | Supported AZ IDs | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | use1\-az2, use1\-az4, use1\-az6 | 
| US West \(Oregon\) | us\-west\-2 | usw2\-az1, usw2\-az2, usw2\-az3 | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | apne2\-az1, apne2\-az3 | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | apse1\-az1, apse1\-az2 | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | apse2\-az1, apse2\-az3 | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | apne1\-az1, apne1\-az4 | 
| Canada \(Central\) | ca\-central\-1 | cac1\-az1, cac1\-az2 | 
| Europe \(Frankfurt\) | eu\-central\-1 | euc1\-az2, euc1\-az3 | 
| Europe \(Ireland\) | eu\-west\-1 | euw1\-az1, euw1\-az2, euw1\-az3 | 
| Europe \(London\) | eu\-west\-2 | euw2\-az2, euw2\-az3 | 
| South America \(São Paulo\) | sa\-east\-1 | sae1\-az1, sae1\-az3 | 

For more information about Availability Zones and AZ IDs, see [ Regions, Availability Zones, and Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\.