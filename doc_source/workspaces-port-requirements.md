# IP Address and Port Requirements for Amazon WorkSpaces<a name="workspaces-port-requirements"></a>

To connect to your WorkSpaces, the network that your Amazon WorkSpaces clients are connected to must have certain ports open to the IP address ranges for the various AWS services \(grouped in subsets\)\. These address ranges vary by AWS Region\. These same ports must also be open on any firewall running on the client\. For more information about the AWS IP address ranges for different Regions, see [AWS IP Address Ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *Amazon Web Services General Reference*\.

For an architecture diagram, see [ WorkSpaces Architecture](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces.html#architecture)\. For additional architecture diagrams, see the ["Best Practices for Deploying Amazon WorkSpaces"](http://aws.amazon.com/workspaces/resources/) whitepaper\.

## Ports for Client Applications<a name="client-application-ports"></a>

The Amazon WorkSpaces client application requires outbound access on the following ports:

Port 443 \(TCP\)  
This port is used for client application updates, registration, and authentication\. The desktop client applications support the use of a proxy server for port 443 \(HTTPS\) traffic\. To enable the use of a proxy server, open the client application, choose **Advanced Settings**, select **Use Proxy Server**, specify the address and port of the proxy server, and choose **Save**\.  
This port must be open to the following IP address ranges:  
+ The `AMAZON` subset in the `GLOBAL` Region\.
+ The `AMAZON` subset in the Region that the WorkSpace is in\.
+ The `AMAZON` subset in the `us-east-1` Region\.
+ The `AMAZON` subset in the `us-west-2` Region\.
+ The `S3` subset in the `us-west-2` Region\.

Port 4172 \(UDP and TCP\)  
This port is used for streaming the WorkSpace desktop and health checks\. The desktop client applications do not support the use of a proxy server for port 4172 traffic; they require a direct connection to port 4172\. This port must be open to the PCoIP Gateway IP address ranges and health check servers in the Region that the WorkSpace is in\. For more information, see [PCoIP Health Check Servers](#health_check) and [PCoIP Gateway Servers](#gateway_IP)\.

**Note**  
If your firewall uses stateful filtering, ephemeral \(also known as dynamic\) ports are automatically opened to allow return communication\. If your firewall uses stateless filtering, you must open ephemeral ports explicitly to allow return communication\. The required ephemeral port range that you must open will vary depending on your configuration\.

## Ports for Web Access<a name="web-access-ports"></a>

Amazon WorkSpaces Web Access requires inbound and outbound access for the following ports:

Port 53 \(UDP\)  
This port is used to access DNS servers\. It must be open to your DNS server IP addresses so that the client can resolve public domain names\. This port requirement is optional if you are not using DNS servers for domain name resolution\.

Port 80 \(UDP and TCP\)  
This port is used for initial connections to `https://clients.amazonworkspaces.com`, which then switch to HTTPS\. It must be open to all IP address ranges in the `EC2` subset in the Region that the WorkSpace is in\. 

Port 443 \(UDP and TCP\)  
This port is used for registration and authentication using HTTPS\. It must be open to all IP address ranges in the `EC2` subset in the Region that the WorkSpace is in\.

Typically, the web browser randomly selects a source port in the high range to use for streaming traffic\. Amazon WorkSpaces Web Access does not have control over the port that the browser selects\. You must ensure that return traffic to this port is allowed\.

Amazon WorkSpaces Web Access prefers UDP over TCP for desktop streams, but falls back to TCP if UDP is not available\. If all UDP ports are blocked except 53, 80, and 443, Web Access will work on Chrome and Firefox using TCP connections\.

## Domains and IP Addresses to Add to Your Allow List<a name="whitelisted_ports"></a>

For the Amazon WorkSpaces client application to be able to access the Amazon WorkSpaces service, you must add the following domains and IP addresses to the allow list on the network from which the client is trying to access the service\.


**Domains and IP addresses to add to your allow list**  

| Category | Domain or IP address | 
| --- | --- | 
| CAPTCHA | https://opfcaptcha\-prod\.s3\.amazonaws\.com/ | 
| Client Auto\-update |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| Connectivity Check |  https://connectivity\.amazonworkspaces\.com/  | 
| Device Metrics \(for 1\.0\+ and 2\.0\+ WorkSpaces client applications\) | https://device\-metrics\-us\-2\.amazon\.com/ | 
| Client Metrics \(for 3\.0\+ WorkSpaces client applications\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| Dynamic Messaging Service \(for 3\.0\+ WorkSpaces client applications\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| Directory Settings |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| Forrester Log Service  | https://fls\-na\.amazon\.com/ | 
| PCoIP Health Check \(DRP\) Servers | [PCoIP Health Check Servers](#health_check) | 
| PCoIP Session Gateway \(PSG\) Servers | [PCoIP Gateway Servers](#gateway_IP) | 
| Registration Dependency \(for Web Access and Teradici PCoIP Zero Clients\) | https://s3\.amazonaws\.com | 
| Session Broker \(PCM\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| User Login Pages | https://<directory id>\.awsapps\.com/ \(where <directory id> is the customer's domain\) | 
| Web Access TURN Servers |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| WS Broker |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 
| WorkSpaces API Endpoints |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/workspaces/latest/adminguide/workspaces-port-requirements.html)  | 

## PCoIP Health Check Servers<a name="health_check"></a>

The Amazon WorkSpaces client applications perform PCoIP health checks over port 4172\. These checks validate whether TCP or UDP traffic streams from the Amazon WorkSpaces servers to the client applications\. For these checks to finish successfully, your firewall policies must allow outbound traffic to the IP addresses of the following Regional PCoIP health check servers\.


| Region | Health check hostname | IP addresses | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | drp\-iad\.amazonworkspaces\.com |  3\.209\.215\.252 3\.212\.50\.30 3\.225\.55\.35 3\.226\.24\.234 34\.200\.29\.95 52\.200\.219\.150  | 
| US West \(Oregon\) | drp\-pdx\.amazonworkspaces\.com |  34\.217\.248\.177 52\.34\.160\.80 54\.68\.150\.54 54\.185\.4\.125 54\.188\.171\.18 54\.244\.158\.140  | 
| Asia Pacific \(Seoul\) | drp\-icn\.amazonworkspaces\.com |  13\.124\.44\.166 13\.124\.203\.105 52\.78\.44\.253 52\.79\.54\.102  | 
| Asia Pacific \(Singapore\) | drp\-sin\.amazonworkspaces\.com |  3\.0\.212\.144 18\.138\.99\.116 18\.140\.252\.123 52\.74\.175\.118  | 
| Asia Pacific \(Sydney\) | drp\-syd\.amazonworkspaces\.com |  3\.24\.11\.127 13\.237\.232\.125  | 
| Asia Pacific \(Tokyo\) | drp\-nrt\.amazonworkspaces\.com |  18\.178\.102\.247 54\.64\.174\.128  | 
| Canada \(Central\) | drp\-yul\.amazonworkspaces\.com |  52\.60\.69\.16 52\.60\.80\.237 52\.60\.173\.117 52\.60\.201\.0  | 
| Europe \(Frankfurt\) | drp\-fra\.amazonworkspaces\.com |  52\.59\.191\.224 52\.59\.191\.225 52\.59\.191\.226 52\.59\.191\.227  | 
| Europe \(Ireland\) | drp\-dub\.amazonworkspaces\.com |  18\.200\.177\.86 52\.48\.86\.38 54\.76\.137\.224  | 
| Europe \(London\) | drp\-lhr\.amazonworkspaces\.com |  35\.176\.62\.54 35\.177\.255\.44 52\.56\.46\.102 52\.56\.111\.36  | 
| South America \(São Paulo\) | drp\-gru\.amazonworkspaces\.com |  18\.231\.0\.105 52\.67\.55\.29 54\.233\.156\.245 54\.233\.216\.234  | 
| AWS GovCloud \(US\-West\) | drp\-pdt\.amazonworkspaces\.com |  52\.61\.60\.65 52\.61\.65\.14 52\.61\.88\.170 52\.61\.137\.87 52\.61\.155\.110 52\.222\.20\.88  | 

## PCoIP Gateway Servers<a name="gateway_IP"></a>

Amazon WorkSpaces uses PCoIP to stream the desktop session to clients over port 4172\. For its PCoIP gateway servers, Amazon WorkSpaces uses a small range of Amazon EC2 public IPv4 addresses\. This enables you to set more finely grained firewall policies for devices that access Amazon WorkSpaces\. Note that the Amazon WorkSpaces clients do not support IPv6 addresses as a connectivity option at this time\.

**Note**  
We are regularly updating our IP address ranges in the [ AWS IP Address Ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) `ip-ranges.json` file\. To ingest the most up\-to\-date IP address ranges for Amazon WorkSpaces, look for entries in the `ip-ranges.json` file where `service: "WORKSPACES_GATEWAYS"`\.


| Region | Public IP Address Range | 
| --- | --- | 
| US East \(N\. Virginia\) |  3\.217\.228\.0 \- 3\.217\.231\.255 3\.235\.112\.0 \- 3\.235\.119\.255 52\.23\.61\.0 \- 52\.23\.62\.255  | 
| US West \(Oregon\) |  44\.234\.54\.0 \- 44\.234\.55\.255 54\.244\.46\.0 \- 54\.244\.47\.255  | 
| Asia Pacific \(Seoul\) |  3\.34\.37\.0 \- 3\.34\.37\.255 3\.34\.38\.0 \- 3\.34\.39\.255 13\.124\.247\.0 \- 13\.124\.247\.255  | 
| Asia Pacific \(Singapore\) |  18\.141\.152\.0 \- 18\.141\.152\.255 18\.141\.154\.0 \- 18\.141\.155\.255 52\.76\.127\.0 \- 52\.76\.127\.255  | 
| Asia Pacific \(Sydney\) |  3\.25\.43\.0 \- 3\.25\.43\.255 3\.25\.44\.0 \- 3\.25\.45\.255 54\.153\.254\.0 \- 54\.153\.254\.255  | 
| Asia Pacific \(Tokyo\) |  18\.180\.178\.0 \- 18\.180\.178\.255 18\.180\.180\.0 \- 18\.180\.181\.255 54\.250\.251\.0 \- 54\.250\.251\.255  | 
| Canada \(Central\) |  15\.223\.100\.0 \- 15\.223\.100\.255 15\.223\.102\.0 \- 15\.223\.103\.255 35\.183\.255\.0 \- 35\.183\.255\.255  | 
| Europe \(Frankfurt\) |  18\.156\.52\.0 \- 18\.156\.52\.255 18\.156\.54\.0 \- 18\.156\.55\.255 52\.59\.127\.0 \- 52\.59\.127\.255  | 
| Europe \(Ireland\) |  3\.249\.28\.0 \- 3\.249\.29\.255 52\.19\.124\.0 \- 52\.19\.125\.255  | 
| Europe \(London\) |  18\.132\.21\.0 \- 18\.132\.21\.255 18\.132\.22\.0 \- 18\.132\.23\.255 35\.176\.32\.0 \- 35\.176\.32\.255  | 
| South America \(São Paulo\) |  18\.230\.103\.0 \- 18\.230\.103\.255 18\.230\.104\.0 \- 18\.230\.105\.255 54\.233\.204\.0 \- 54\.233\.204\.255  | 
| AWS GovCloud \(US\-West\) |  52\.61\.193\.0 \- 52\.61\.193\.255  | 

## Network Interfaces<a name="network-interfaces"></a>

Each WorkSpace has the following network interfaces:
+ The primary network interface \(eth1\) provides connectivity to the resources within your VPC and on the internet, and is used to join the WorkSpace to the directory\.
+ The management network interface \(eth0\) is connected to a secure Amazon WorkSpaces management network\. It is used for interactive streaming of the WorkSpace desktop to Amazon WorkSpaces clients, and to allow Amazon WorkSpaces to manage the WorkSpace\.

Amazon WorkSpaces selects the IP address for the management network interface from various address ranges, depending on the Region that the WorkSpaces are created in\. When a directory is registered, Amazon WorkSpaces tests the VPC CIDR and the route tables in your VPC to determine if these address ranges create a conflict\. If a conflict is found in all available address ranges in the Region, an error message is displayed and the directory is not registered\. If you change the route tables in your VPC after the directory is registered, you might cause a conflict\.

**Warning**  
Do not modify or delete any of the network interfaces that are attached to a WorkSpace\. Doing so might cause the WorkSpace to become unreachable or lose internet access\. For example, if you have [enabled automatic assignment of Elastic IP addresses](update-directory-details.md#automatic-assignment) at the directory level, an [ Elastic IP address](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-eips.html) \(from the Amazon\-provided pool\) is assigned to your WorkSpace when it is launched\. However, if you associate an Elastic IP address that you own to a WorkSpace, and then you later disassociate that Elastic IP address from the WorkSpace, the WorkSpace loses its public IP address, and it doesn't automatically get a new one from the Amazon\-provided pool\.  
To associate a new public IP address from the Amazon\-provided pool with the WorkSpace, you must [rebuild the WorkSpace](rebuild-workspace.md)\. If you don't want to rebuild the WorkSpace, you must associate another Elastic IP address that you own to the WorkSpace\.

### Management Interface IP Ranges<a name="management-ip-ranges"></a>

The following table lists the IP address ranges used for the management network interface\.


| Region | IP Address Range | 
| --- | --- | 
| US East \(N\. Virginia\) | 172\.31\.0\.0/16, 192\.168\.0\.0/16, 198\.19\.0\.0/16 | 
| US West \(Oregon\) | 172\.31\.0\.0/16, 192\.168\.0\.0/16, and 198\.19\.0\.0/16 | 
| Asia Pacific \(Seoul\) | 198\.19\.0\.0/16 | 
| Asia Pacific \(Singapore\) | 198\.19\.0\.0/16 | 
| Asia Pacific \(Sydney\) | 172\.31\.0\.0/16, 192\.168\.0\.0/16, and 198\.19\.0\.0/16 | 
| Asia Pacific \(Tokyo\) | 198\.19\.0\.0/16 | 
| Canada \(Central\) | 198\.19\.0\.0/16 | 
| Europe \(Frankfurt\) | 198\.19\.0\.0/16 | 
| Europe \(Ireland\) | 172\.31\.0\.0/16, 192\.168\.0\.0/16, and 198\.19\.0\.0/16 | 
| Europe \(London\) | 198\.19\.0\.0/16 | 
| South America \(São Paulo\) | 198\.19\.0\.0/16 | 
| AWS GovCloud \(US\-West\) | 198\.19\.0\.0/16 | 

### Management Interface Ports<a name="management_ports"></a>

The following ports must be open on the management network interface of all WorkSpaces:
+ Inbound TCP on port 4172\. This is used for establishment of the streaming connection\.
+ Inbound UDP on port 4172\. This is used for streaming user input\.
+ Inbound TCP on port 4489\. This is for access using the web client\.
+ Inbound TCP on port 8200\. This is used for management and configuration of the WorkSpace\.
+ Outbound TCP on ports 8443 and 9997\. This is used for access using the web client\.
+ Outbound UDP on ports 3478 and 4172\. This is used for access using the web client\.
+ Outbound UDP on ports 50002 and 55002\. This is used for PCoIP streaming\. If your firewall uses stateful filtering, the ephemeral ports 50002 and 55002 are automatically opened to allow return communication\. If your firewall uses stateless filtering, you must open ephemeral ports 49152 \- 65535 to allow return communication\.
+ Outbound TCP on port 80 to IP address 169\.254\.169\.254 for access to the EC2 metadata service\. Any HTTP proxy assigned to your WorkSpaces must also exclude 169\.254\.169\.254\.
+ Outbound TCP on port 1688 to IP addresses 169\.254\.169\.250 and 169\.254\.169\.251 to allow access to Microsoft KMS for Windows and Office activation\.

Under normal circumstances, the Amazon WorkSpaces service configures these ports for your WorkSpaces\. If any security or firewall software is installed on a WorkSpace that blocks any of these ports, the WorkSpace may not function correctly or may be unreachable\.

### Primary Interface Ports<a name="primary_ports"></a>

No matter which type of directory you have, the following ports must be open on the primary network interface of all WorkSpaces:
+ For internet connectivity, the following ports must be open outbound to all destinations and inbound from the WorkSpaces VPC\. You need to add these manually to the security group for your WorkSpaces if you want them to have internet access\.
  + TCP 80 \(HTTP\)
  + TCP 443 \(HTTPS\)
+ To communicate with the directory controllers, the following ports must be open between your WorkSpaces VPC and your directory controllers\. For a Simple AD directory, the security group created by AWS Directory Service will have these ports configured correctly\. For an AD Connector directory, you might need to adjust the default security group for the VPC to open these ports\.
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