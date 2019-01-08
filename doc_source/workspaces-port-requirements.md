# Port Requirements for Amazon WorkSpaces<a name="workspaces-port-requirements"></a>

To connect to your WorkSpaces, the network that your Amazon WorkSpaces clients are connected to must have certain ports open to the IP address ranges for the various AWS services \(grouped in subsets\)\. These address ranges vary by AWS region\. These same ports must also be open on any firewall running on the client\. For more information about the AWS IP address ranges for different regions, see [AWS IP Address Ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *Amazon Web Services General Reference*\.

## Ports for Client Applications<a name="client-application-ports"></a>

The Amazon WorkSpaces client application requires outbound access on the following ports:

Port 443 \(TCP\)  
This port is used for client application updates, registration, and authentication\. The desktop client applications support the use of a proxy server for port 443 \(HTTPS\) traffic\. To enable the use of a proxy server, open the client application, choose **Advanced Settings**, select **Use Proxy Server**, specify the address and port of the proxy server, and choose **Save**\.  
This port must be open to the following IP address ranges:  
+ The `AMAZON` subset in the `GLOBAL` region\.
+ The `AMAZON` subset in the region that the WorkSpace is in\.
+ The `AMAZON` subset in the `us-east-1` region\.
+ The `AMAZON` subset in the `us-west-2` region\.
+ The `S3` subset in the `us-west-2` region\.

Port 4172 \(UDP and TCP\)  
This port is used for streaming the WorkSpace desktop and health checks\. It must be open to the PCoIP Gateway IP address ranges and health check servers in the region that the WorkSpace is in\. For more information, see [PCoIP Gateway and Health Check Servers](#gateway_IP)\.

## Ports for Web Access<a name="web-access-ports"></a>

Amazon WorkSpaces Web Access requires inbound and outbound access for the following ports:

Port 53 \(UDP\)  
This port is used to access DNS servers\. It must be open to your DNS server IP addresses so that the client can resolve public domain names\. This port requirement is optional if you are not using DNS servers for domain name resolution\.

Port 80 \(UDP and TCP\)  
This port is used for initial connections to `http://clients.amazonworkspaces.com`, which then switch to HTTPS\. It must be open to all IP address ranges in the `EC2` subset in the region that the WorkSpace is in\. 

Port 443 \(UDP and TCP\)  
This port is used for registration and authentication using HTTPS\. It must be open to all IP address ranges in the `EC2` subset in the region that the WorkSpace is in\.

Typically, the web browser randomly selects a source port in the high range to use for streaming traffic\. Amazon WorkSpaces Web Access does not have control over the port the browser selects\. You must ensure that return traffic to this port is allowed\.

Amazon WorkSpaces Web Access prefers UDP over TCP for desktop streams, but falls back to TCP if UDP is not available as follows:
+ Amazon WorkSpaces Web Access will work on Chrome even if all UDP ports are blocked except 53, 80, and 443, using TCP connections\.
+ Amazon WorkSpaces Web Access will not work on Firefox if all UDP ports are blocked except 53, 80, and 443\. Additional UDP ports must be open to enable streaming\.

## Whitelisted Domains and Ports<a name="whitelisted_ports"></a>

For the Amazon WorkSpaces client application to be able to access the Amazon WorkSpaces service, the following domains and ports must be whitelisted on the network from which the client is trying to access the service\.


**Whitelisted domains and ports**  

| Category | Whitelisted | 
| --- | --- | 
| Session Broker \(PCM\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| PCoIP Session Gateway \(PSG\) | [PCoIP Gateway and Health Check Servers](#gateway_IP) | 
| PCoIP Healthcheck \(DRP\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| Device Metrics  | https://device\-metrics\-us\-2\.amazon\.com/ | 
| Forrester Log Service  | https://fls\-na\.amazon\.com/ | 
| Directory Settings |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| CAPTCHA | https://opfcaptcha\-prod\.s3\.amazonaws\.com/ | 
| Client Auto\-update | https://d2td7dqidlhjx7\.cloudfront\.net/ | 
| Registration Dependency | https://s3\.amazonaws\.com | 
| Connectivity Check | https://connectivity\.amazonworkspaces\.com/  | 
| User Login Pages | https://<directory id>\.awsapps\.com/ \(where <directory id> is the customer's domain\) | 
| Web Access TURN Servers |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| WS Broker |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 

## PCoIP Gateway and Health Check Servers<a name="gateway_IP"></a>

Amazon WorkSpaces uses PCoIP to stream the desktop session to clients over port 4172\. Amazon WorkSpaces uses a small range of Amazon EC2 public IP addresses for its PCoIP gateway servers\. This enables you to set more finely grained firewall policies for devices that access Amazon WorkSpaces\.


| Region | Public IP Address Range | 
| --- | --- | 
| US East \(N\. Virginia\) | 52\.23\.61\.0 – 52\.23\.62\.255 | 
| US West \(Oregon\) | 54\.244\.46\.0 – 54\.244\.47\.255 | 
| Canada \(Central\) | 35\.183\.255\.0 \- 35\.183\.255\.255 | 
| EU \(Ireland\) | 52\.19\.124\.0 – 52\.19\.125\.255 | 
| EU \(Frankfurt\) | 52\.59\.127\.0 \- 52\.59\.127\.255 | 
| EU \(London\) | 35\.176\.32\.0 \- 35\.176\.32\.255 | 
| Asia Pacific \(Singapore\) | 52\.76\.127\.0 – 52\.76\.127\.255 | 
| Asia Pacific \(Sydney\) | 54\.153\.254\.0 – 54\.153\.254\.255 | 
| Asia Pacific \(Seoul\) | 13\.124\.247\.0 \- 13\.124\.247\.255 | 
| Asia Pacific \(Tokyo\) | 54\.250\.251\.0 – 54\.250\.251\.255 | 
| South America \(São Paulo\) | 54\.233\.204\.0 \- 54\.233\.204\.255 | 

The Amazon WorkSpaces client application performs PCoIP health checks over port 4172\. This validates whether TCP or UDP traffic streams from the Amazon WorkSpaces servers to the client applications\. To do this successfully, your firewall policies must take into account the following regional PCoIP health check servers\.


| Region | Health check server | 
| --- | --- | 
| US East \(N\. Virginia\) | drp\-iad\.amazonworkspaces\.com | 
| US West \(Oregon\) | drp\-pdx\.amazonworkspaces\.com | 
| Canada \(Central\) | drp\-yul\.amazonworkspaces\.com | 
| EU \(Ireland\) | drp\-dub\.amazonworkspaces\.com | 
| EU \(Frankfurt\) | drp\-fra\.amazonworkspaces\.com | 
| EU \(London\) | drp\-lhr\.amazonworkspaces\.com | 
| Asia Pacific \(Singapore\) | drp\-sin\.amazonworkspaces\.com | 
| Asia Pacific \(Sydney\) | drp\-syd\.amazonworkspaces\.com | 
| Asia Pacific \(Seoul\) | drp\-icn\.amazonworkspaces\.com | 
| Asia Pacific \(Tokyo\) | drp\-nrt\.amazonworkspaces\.com | 
| South America \(São Paulo\) | drp\-gru\.amazonworkspaces\.com | 

## Network Interfaces<a name="network-interfaces"></a>

Each WorkSpace has the following network interfaces:
+ The primary network interface provides connectivity to the resources within your VPC as well as the Internet, and is used to join the WorkSpace to the directory\.
+ The management network interface is connected to a secure Amazon WorkSpaces management network\. It is used for interactive streaming of the WorkSpace desktop to Amazon WorkSpaces clients, and to allow Amazon WorkSpaces to manage the WorkSpace\.

Amazon WorkSpaces selects the IP address for the management network interface from various address ranges, depending on the region the WorkSpaces are created in\. When a directory is registered, Amazon WorkSpaces tests the VPC CIDR and the route tables in your VPC to determine if these address ranges create a conflict\. If a conflict is found in all available address ranges in the region, an error message is displayed and the directory is not registered\. If you change the route tables in your VPC after the directory is registered, you might cause a conflict\.

Do not modify or delete any of the network interfaces attached to a WorkSpace\. Doing so might cause the WorkSpace to become unreachable\.

### Management Interface IP Ranges<a name="management-ip-ranges"></a>

The following table lists the IP address ranges used for the management network interface\.


| Region | IP Address Range | 
| --- | --- | 
| US East \(N\. Virginia\) | 172\.31\.0\.0/16, 192\.168\.0\.0/16, and 198\.19\.0\.0/16 | 
| US West \(Oregon\) | 172\.31\.0\.0/16 and 192\.168\.0\.0/16 | 
| Canada \(Central\) | 198\.19\.0\.0/16 | 
| EU \(Ireland\) | 172\.31\.0\.0/16 and 192\.168\.0\.0/16 | 
| EU \(Frankfurt\) | 198\.19\.0\.0/16 | 
| EU \(London\) | 198\.19\.0\.0/16 | 
| Asia Pacific \(Singapore\) | 198\.19\.0\.0/16 | 
| Asia Pacific \(Sydney\) | 172\.31\.0\.0/16, 192\.168\.0\.0/16, and 198\.19\.0\.0/16 | 
| Asia Pacific \(Seoul\) | 198\.19\.0\.0/16 | 
| Asia Pacific \(Tokyo\) | 198\.19\.0\.0/16 | 
| South America \(São Paulo\) | 198\.19\.0\.0/16 | 

### Management Interface Ports<a name="management_ports"></a>

The following ports must be open on the management network interface of all WorkSpaces:
+ Inbound TCP on port 4172\. This is used for establishment of the streaming connection\.
+ Inbound UDP on port 4172\. This is used for streaming user input\.
+ Inbound TCP on port 4489\. This is for access using the web client\.
+ Inbound TCP on port 8200\. This is used for management and configuration of the WorkSpace\.
+ Outbound TCP on ports 8443 and 9997\. This is used for access using the web client\.
+ Outbound UDP on port 3478 and 4172\. This is used for access using the web client\.
+ Outbound UDP on port 55002\. This is used for PCoIP streaming\. If your firewall uses stateful filtering, the ephemeral port 55002 is automatically opened to allow return communication\. If your firewall uses stateless filtering, you need to open ephemeral ports 49152 \- 65535 to allow return communication\.
+ Outbound TCP on port 80 to IP address 169\.254\.169\.254 for access to the EC2 metadata service\. Any HTTP proxy assigned to your WorkSpaces must also exclude 169\.254\.169\.254\.
+ Outbound TCP on port 1688 to IP addresses 169\.254\.169\.250 and 169\.254\.169\.251 to allow access to Microsoft KMS for Windows and Office activation\.

Under normal circumstances, the Amazon WorkSpaces service configures these ports for your WorkSpaces\. If any security or firewall software is installed on a WorkSpace that blocks any of these ports, the WorkSpace may not function correctly or may be unreachable\.

### Primary Interface Ports<a name="primary_ports"></a>

No matter which type of directory you have, the following ports must be open on the primary network interface of all WorkSpaces:
+ For Internet connectivity, the following ports must be open outbound to all destinations and inbound from the WorkSpaces VPC\. You need to add these manually to the security group for your WorkSpaces if you want them to have Internet access\.
  + TCP 80 \(HTTP\)
  + TCP 443 \(HTTPS\)
+ To communicate with the directory controllers, the following ports must be open between your WorkSpaces VPC and your directory controllers\. For a Simple AD directory, the security group created by AWS Directory Service will have these ports configured correctly\. For an AD Connector directory, you may need to adjust the default security group for the VPC to open these ports\.
  + TCP/UDP 53 \- DNS
  + TCP/UDP 88 \- Kerberos authentication
  + UDP 123 \- NTP
  + TCP 135 \- RPC
  + UDP 137\-138 \- Netlogon
  + TCP 139 \- Netlogon
  + TCP/UDP 389 \- LDAP
  + TCP/UDP 445 \- SMB
  + TCP 1024\-65535 \- Dynamic ports for RPC

  If any security or firewall software is installed on a WorkSpace that blocks any of these ports, the WorkSpace may not function correctly or may be unreachable\.