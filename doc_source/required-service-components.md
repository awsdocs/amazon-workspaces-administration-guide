# Required Configuration and Service Components for WorkSpaces<a name="required-service-components"></a>

As a WorkSpace administrator, you must understand the following about required configuration and service components\. 

## Required Routing Table Configuration<a name="routing-table-configuration"></a>

We recommend that you not modify the operating system\-level routing table for a WorkSpace\. The WorkSpaces service requires the preconfigured routes in this table to monitor the system state and update system components\. If routing table changes are required for your organization, contact AWS Support or your AWS account team before applying any changes\. 

## Required Service Components<a name="required-service-components"></a>

On Windows WorkSpaces, the service components are installed in the following locations\. Do not delete, change, block, or quarantine these objects\. If you do so, the WorkSpace will not function correctly\.

**Note**  
If antivirus software is installed on the WorkSpace, exclude the following locations\.
+ C:\\Program Files\\Amazon
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