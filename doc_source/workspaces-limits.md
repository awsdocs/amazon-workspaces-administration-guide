# Amazon WorkSpaces quotas<a name="workspaces-limits"></a>

Amazon WorkSpaces provides different resources that you can use in your account in a given Region, including WorkSpaces, images, bundles, directories, connection aliases, and IP control groups\. When you create your Amazon Web Services account, we set default quotas \(also referred to as limits\) on the number of resources that you can create\.

The following are the default quotas for WorkSpaces for your AWS account\. You can use the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home/) to view the default quota and applied quota, or to [request quota increases](https://console.aws.amazon.com/servicequotas/home/services/workspaces/quotas) for adjustable quotas\. 

In some Regions, where Service Quotas is not available, you must submit a support case to request limit increase\. For more information, see [Viewing service quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/gs-request-quota.html) and [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.


| Resource | Default | Description | Adjustable | 
| --- | --- | --- | --- | 
| WorkSpaces | 1 | The maximum number of WorkSpaces in this account in the current Region\. | Yes | 
| Graphics WorkSpaces | 0 | The maximum number of Graphics WorkSpaces in this account in the current Region\. | Yes | 
| Graphics\.g4dn WorkSpaces | 0 | The maximum number of Graphics\.g4dn WorkSpaces in this account in the current Region\.  | Yes | 
| GraphicsPro WorkSpaces | 0 | The maximum number of GraphicsPro WorkSpaces in this account in the current Region\. | Yes | 
| GraphicsPro\.g4dn WorkSpaces | 0 | The maximum number of GraphicsPro\.g4dn WorkSpaces in this account in the current Region\.  | Yes | 
| Bundles | 50 | The maximum number of bundles in this account in the current Region\. This quota applies only to custom bundles, not to public bundles\. | No | 
| Connection aliases | 20 | The maximum number of connection aliases in this account in the current Region\. | No | 
| Directories | 50 | The maximum number of directories that can be registered for use with Amazon WorkSpaces in this account in the current Region\. | No | 
| Images | 40 | The maximum number of images in this account in the current Region\. | Yes | 
| IP access control groups | 100 | The maximum number of IP access control groups in this account in the current Region\. | No | 
| IP access control groups per directory | 25 | The maximum number of IP access control groups per directory in this account in the current Region\. | No | 
| Rules per IP access control group | 10 | The maximum number of rules per IP access control group in this account in the current Region\. | No | 

**API throttling**  
The allowed rate is two calls per second\. For more information, see [Throttling exceptions](amazon-workspaces-troubleshooting.md#throttled-api-calls)\.