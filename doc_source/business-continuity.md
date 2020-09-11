# Business Continuity for Amazon WorkSpaces<a name="business-continuity"></a>

Amazon WorkSpaces is built on the AWS global infrastructure, which is organized into AWS Regions and Availability Zones\. These Regions and Availability Zones provide resiliency in terms of both physical isolation and data redundancy\. For more information, see [Resilience in Amazon WorkSpaces](disaster-recovery-resiliency.md)\.

Amazon WorkSpaces also provides cross\-Region redirection, a feature that works with your Domain Name System \(DNS\) routing policies to redirect your WorkSpaces users to alternative WorkSpaces when their primary WorkSpaces aren't available\. For example, by using DNS failover routing policies, you can connect your users to WorkSpaces in your specified failover Region when they can't access their WorkSpaces in the primary Region\.

You can use cross\-Region redirection to achieve regional resiliency and high availability\. You can also use it for other purposes, such as traffic distribution or providing alternative WorkSpaces during maintenance periods\. If you use Amazon Route 53 for your DNS configuration, you can take advantage of health checks that monitor Amazon CloudWatch alarms\.

**Topics**
+ [Cross\-Region Redirection for Amazon WorkSpaces](cross-region-redirection.md)