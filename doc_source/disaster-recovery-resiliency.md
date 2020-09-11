# Resilience in Amazon WorkSpaces<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. Regions provide multiple physically separated and isolated Availability Zones, which are connected through low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\.

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

Amazon WorkSpaces also provides cross\-Region redirection, a feature that works with your Domain Name System \(DNS\) failover routing policies to redirect your WorkSpaces users to alternative WorkSpaces in another AWS Region when their primary WorkSpaces aren't available\. For more information, see [Cross\-Region Redirection for Amazon WorkSpaces](cross-region-redirection.md)\.