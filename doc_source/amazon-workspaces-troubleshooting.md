# Troubleshooting Amazon WorkSpaces Issues<a name="amazon-workspaces-troubleshooting"></a>

The following information can help you troubleshoot issues with your WorkSpaces\.

**Topics**
+ [Launching WorkSpaces in my connected directory often fails](#provision_fail)
+ [Launching WorkSpaces fails with an internal error](#launch-failure-ipv6)
+ [Can't connect to a WorkSpace with an interactive logon banner](#logon_banner)
+ [No WorkSpaces in my directory can connect to the Internet](#no_internet)
+ [I receive a "DNS unavailable" error when I try to connect to my on\-premises directory](#dns_unavailable)
+ [I receive a "Connectivity issues detected" error when I try to connect to my on\-premises directory](#connectivity_issues_detected)
+ [I receive an "SRV record" error when I try to connect to my on\-premises directory](#srv_record_not_found)
+ [One of my WorkSpaces has a state of "Unhealthy"](#unhealthy)
+ [The state of my apps was not saved when my WorkSpace was stopped](#save-state)

## Launching WorkSpaces in my connected directory often fails<a name="provision_fail"></a>

Verify that the two DNS servers or domain controllers in your on\-premises directory are accessible from each of the subnets that you specified when you connected to your directory\. You can verify this connectivity by launching an EC2 instance in each subnet and joining the instance to your directory, using the IP addresses of the two DNS servers\.

## Launching WorkSpaces fails with an internal error<a name="launch-failure-ipv6"></a>

Check whether your subnets are configured to automatically assign IPv6 addresses to instances launched in the subnet\. To check this setting, open the Amazon VPC console, select your subnet, and choose **Subnet Actions**, **Modify auto\-assign IP settings**\. If this setting is enabled, you cannot launch WorkSpaces using the Performance or Graphics bundles\. Instead, disable this setting and specify IPv6 addresses manually when you launch your instances\.

## Can't connect to a WorkSpace with an interactive logon banner<a name="logon_banner"></a>

Implementing an interactive logon message to display a logon banner prevents users from being able to access their WorkSpaces\. The interactive logon message Group Policy setting is not currently supported by Amazon WorkSpaces\.

## No WorkSpaces in my directory can connect to the Internet<a name="no_internet"></a>

WorkSpaces cannot communicate with the Internet by default\. You must explicitly provide Internet access\. For more information, see [Provide Internet Access from Your WorkSpace](amazon-workspaces-internet-access.md)\.

## I receive a "DNS unavailable" error when I try to connect to my on\-premises directory<a name="dns_unavailable"></a>

You receive an error message similar to the following when connecting to your on\-premises directory:

```
DNS unavailable (TCP port 53) for IP: dns-ip-address
```

AD Connector must be able to communicate with your on\-premises DNS servers via TCP and UDP over port 53\. Verify that your security groups and on\-premises firewalls allow TCP and UDP communication over this port\.

## I receive a "Connectivity issues detected" error when I try to connect to my on\-premises directory<a name="connectivity_issues_detected"></a>

You receive an error message similar to the following when connecting to your on\-premises directory:

```
Connectivity issues detected: LDAP unavailable (TCP port 389) for IP: ip-address
Kerberos/authentication unavailable (TCP port 88) for IP: ip-address
Please ensure that the listed ports are available and retry the operation.
```

AD Connector must be able to communicate with your on\-premises domain controllers via TCP and UDP over the following ports\. Verify that your security groups and on\-premises firewalls allow TCP and UDP communication over these ports\.
+ 88 \(Kerberos\)
+ 389 \(LDAP\)

## I receive an "SRV record" error when I try to connect to my on\-premises directory<a name="srv_record_not_found"></a>

You receive an error message similar to one or more of the following when connecting to your on\-premises directory:

```
SRV record for LDAP does not exist for IP: dns-ip-address

SRV record for Kerberos does not exist for IP: dns-ip-address
```

AD Connector needs to obtain the `_ldap._tcp.dns-domain-name` and `_kerberos._tcp.dns-domain-name` SRV records when connecting to your directory\. You will get this error if the service cannot obtain these records from the DNS servers that you specified when connecting to your directory\. Make sure that your DNS servers contains these SRV records\. For more information, see [SRV Resource Records](http://technet.microsoft.com/en-us/library/cc961719.aspx) on Microsoft TechNet\.

## One of my WorkSpaces has a state of "Unhealthy"<a name="unhealthy"></a>

The Amazon WorkSpaces service periodically sends status requests to a WorkSpace\. A WorkSpace is marked `Unhealthy` when it fails to respond to these requests\. Common causes for this problem are:
+ An application on the WorkSpace is blocking network ports which prevents the WorkSpace from responding to the status request\.
+ High CPU utilization is preventing the WorkSpace from responding to the status request in a timely manner\.
+ The computer name of the WorkSpace has been changed\. This prevents a secure channel from being established between Amazon WorkSpaces and the WorkSpace\.

You can attempt to correct the situation using the following methods:
+ Reboot the WorkSpace from the Amazon WorkSpaces console\.
+ Connect to the unhealthy WorkSpace using the following procedure, which should be used only for troubleshooting purposes:

  1. Connect to an operational WorkSpace in the same directory as the unhealthy WorkSpace\.

  1. From the operational WorkSpace, use Remote Desktop Protocol \(RDP\) to connect to the unhealthy WorkSpace using the IP address of the unhealthy WorkSpace\. Depending on the extent of the problem, you might not be able to connect to the unhealthy WorkSpace\.

  1. On the unhealthy WorkSpace, confirm that the minimum port requirements are met\.
+ Rebuild the WorkSpace from the Amazon WorkSpaces console\. Because rebuilding a WorkSpace can potentially cause a loss of data, this option should only be used if all other attempts to correct the problem have been unsuccessful\.

## The state of my apps was not saved when my WorkSpace was stopped<a name="save-state"></a>

To save the state of your apps, you must have enough free space on the root volume of your WorkSpace to store the total memory offered on the WorkSpace bundle\. For example, a Standard Workspace has 4 GB of memory, so you must have 4 GB of free space on the root volume of the WorkSpace\.

Also, if the Amazon WorkSpaces service could not communicate with the WorkSpace when a stop request is issued, it will force shut down the operating system and the state of the apps will not be saved\.