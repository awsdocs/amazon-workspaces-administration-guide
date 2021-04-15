# Required Configuration and Service Components for WorkSpaces<a name="required-service-components"></a>

As a WorkSpace administrator, you must understand the following about required configuration and service components\. 

## Required Routing Table Configuration<a name="routing-table-configuration"></a>

We recommend that you not modify the operating system\-level routing table for a WorkSpace\. The WorkSpaces service requires the preconfigured routes in this table to monitor the system state and update system components\. If routing table changes are required for your organization, contact AWS Support or your AWS account team before applying any changes\. 

## Required Service Components<a name="required-service-components"></a>

On Windows WorkSpaces, the service components are installed in the following locations\. Do not delete, change, block, or quarantine these objects\. If you do so, the WorkSpace will not function correctly\.

If antivirus software is installed on the WorkSpace, make sure it does not interfere with the service components installed in the following locations\.

**Important**  
Starting March 29, 2021, we're updating the PCoIP agent from 32\-bit to 64\-bit\. For Windows WorkSpaces that are using the PCoIP protocol, this means that the location of the Teradici files changes from `C:\Program Files (x86)\Teradici` to `C:\Program Files\Teradici`\. These PCoIP agent updates will occur in waves during our regular maintenance windows, which means that some of your WorkSpaces might be using the 32\-bit agent and some might be using the 64\-bit agent during this transition\.  
If you've configured firewall rules, antivirus software exclusions \(on the client side and host side\), Group Policy Object \(GPO\) settings, or settings for Microsoft System Center Configuration Manager \(SCCM\), Microsoft Endpoint Configuration Manager, or similar configuration management tools based on the full path to the 32\-bit agent, you must also add the full path to the 64\-bit agent to those settings\.  
Because your WorkSpaces might not all be upgraded at the same time, do not replace the 32\-bit path with the 64\-bit path, or some of your WorkSpaces might not work\. For example, if you're basing your exclusions or communication filters on `C:\Program Files (x86)\Teradici\PCoIP Agent\bin\pcoip_server_win32.exe`, you must also add `C:\Program Files\Teradici\PCoIP Agent\bin\pcoip_server.exe`\.
+ C:\\Program Files\\Amazon
+ C:\\Program Files\\Teradici
+ C:\\Program Files \(x86\)\\Teradici
+ C:\\ProgramData\\Amazon
+ C:\\ProgramData\\Teradici

On Amazon Linux WorkSpaces, the service components are installed in the following locations\. Do not delete, change, block, or quarantine these objects\. If you do so, the WorkSpace will not function correctly\.
+ /etc/dhcp/dhclient\.conf
+ /etc/os\-release
+ /etc/pam\.d/pcoip
+ /etc/pam\.d/pcoip\-session
+ /etc/profile\.d/system\-restart\-check\.sh
+ /etc/X11/default\-display\-manager
+ /etc/yum/pluginconf\.d/halt\_os\_update\_check\.conf
+ /usr/lib/pcoip\-agent
+ /usr/lib/skylight
+ /usr/lib/systemd/system/pcoip\.service
+ /usr/lib/systemd/system/pcoip\.service\.d/
+ /usr/lib/systemd/system/skylight\-agent\.service
+ /usr/lib/yum\-plugins/halt\_os\_update\_check\.py
+ /var/lib/pcoip\-agent
+ /var/lib/skylight
+ /var/log/pcoip\-agent 
+ /var/log/skylight