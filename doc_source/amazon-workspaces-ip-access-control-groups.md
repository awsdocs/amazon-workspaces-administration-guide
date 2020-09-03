# IP Access Control Groups for Your WorkSpaces<a name="amazon-workspaces-ip-access-control-groups"></a>

An *IP access control group* acts as a virtual firewall that controls the IP addresses from which users are allowed to access their WorkSpaces\. You can associate each IP access control group with one or more directories\. You can create up to 100 IP access control groups per Region per AWS account\. However, you can only associate up to 25 IP access control groups with a single directory\.

There is a default IP access control group associated with each directory\. The default group allows all traffic\. If you associate an IP access control group with a directory, the default IP access control group is disassociated\.

To specify the public IP addresses and ranges of IP addresses for your trusted networks, add rules to your IP access control groups\. If your users access their WorkSpaces through a NAT gateway or VPN, you must create rules that allow traffic from the public IP addresses for the NAT gateway or VPN\.

**Note**  
IP access control groups do not allow the use of dynamic IP addresses for NATs\. If you're using a NAT, configure it to use a static IP address instead of a dynamic IP address\. Make sure the NAT routes all the UDP traffic through the same static IP address for the duration of the WorkSpaces session\.

You can use this feature with Web Access and the client applications for macOS, iPad, Windows, Chromebook, and Android\. To use this feature with a PCoIP zero client, you cannot use PCoIP Connection Manager\.

## Create an IP Access Control Group<a name="create-ip-access-control-group"></a>

You can create an IP access control group as follows\. Each IP access control group can contain up to 10 rules\.

**To create an IP access control group**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **IP Access Controls**\.

1. Choose **Create IP Group**\.

1. In the **Create IP Group** dialog box, enter a name and description for the group and choose **Create**\.

1. Select the group and choose **Edit**\.

1. For each IP address, choose **Add Rule**\. For **Source**, enter the IP address or IP address range\. For **Description**, enter a description\. When you are done adding rules, choose **Save**\.

## Associate an IP Access Control Group with a Directory<a name="associate-ip-access-control-group"></a>

You can associate an IP access control group with a directory to ensure that WorkSpaces are accessed only from trusted networks\.

If you associate an IP access control group that has no rules with a directory, this blocks all access to all WorkSpaces\.

**To associate an IP access control group with a directory**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Update Details**\.

1. Expand **IP Access Control Groups** and select one or more IP access control groups\.

1. Choose **Update and Exit**\.

## Copy an IP Access Control Group<a name="copy-ip-access-control-group"></a>

You can use an existing IP access control group as a base for creating a new IP access control group\.

**To create an IP access control group from an existing one**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **IP Access Controls**\.

1. Select the group and choose **Actions**, **Copy to New**\.

1. In the **Copy IP Group** dialog box, enter a name and description for the new group and choose **Copy Group**\.

1. \(Optional\) To modify the rules copied from the original group, select the new group and choose **Edit**\. Add, update, or remove rules as needed\. Choose **Save**\.

## Delete an IP Access Control Group<a name="delete-ip-access-control-group"></a>

You can delete a rule from an IP access control group at any time\. If you remove a rule that was used to allow a connection to a WorkSpace, the user is disconnected from the WorkSpace\.

Before you can delete an IP access control group, you must disassociate it from any directories\.

**To delete an IP access control group**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. For each directory that is associated with the IP access control group, select the directory and choose **Actions**, **Update Details**\. Expand **IP Access Control Groups**, clear the check box for the IP access control group, and choose **Update and Exit**\.

1. In the navigation pane, choose **IP Access Controls**\.

1. Select the group and choose **Actions**, **Delete IP Group**\.