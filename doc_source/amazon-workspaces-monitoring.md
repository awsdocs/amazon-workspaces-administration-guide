# Monitor your WorkSpaces<a name="amazon-workspaces-monitoring"></a>

You can use the following features to monitor your WorkSpaces\.

**CloudWatch metrics**  
Amazon WorkSpaces publishes data points to Amazon CloudWatch about your WorkSpaces\. CloudWatch enables you to retrieve statistics about those data points as an ordered set of time\-series data, known as *metrics*\. You can use these metrics to verify that your WorkSpaces are performing as expected\. For more information, see [Monitor your WorkSpaces using CloudWatch metrics](cloudwatch-metrics.md)\.

**CloudWatch Events**  
Amazon WorkSpaces can submit events to Amazon CloudWatch Events when users log in to your WorkSpace\. This enables you to respond when the event occurs\. For more information, see [Monitor your WorkSpaces using CloudWatch Events](cloudwatch-events.md)\.

**CloudTrail logs**  
AWS CloudTrail provides a record of actions taken by a user, role, or an AWS service in WorkSpaces\. Using the information collected by CloudTrail, you can determine the request that was made to WorkSpaces, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, see [Logging WorkSpaces API Calls by Using CloudTrail](https://docs.aws.amazon.com/workspaces/latest/api/cloudtrail_logging.html)\. AWS CloudTrail logs successful and unsuccessful sign\-in events for smart card users\. For more information, see [Understanding AWS sign\-in events for smart card users](signin-events.md)\.