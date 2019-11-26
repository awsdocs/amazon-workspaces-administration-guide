# Monitor Your WorkSpaces Using CloudWatch Metrics<a name="cloudwatch-metrics"></a>

Amazon WorkSpaces and Amazon CloudWatch are integrated, so you can gather and analyze performance metrics\. You can monitor these metrics using the CloudWatch console, the CloudWatch command line interface, or programmatically using the CloudWatch API\. CloudWatch also allows you to set alarms when you reach a specified threshold for a metric\.

For more information about using CloudWatch and alarms, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/)\.

**Prerequisites**  
To get CloudWatch metrics, enable access on port 443 on the `AMAZON` subset in the `us-east-1` Region\. For more information, see [IP Address and Port Requirements for Amazon WorkSpaces](workspaces-port-requirements.md)\.

**Topics**
+ [Amazon WorkSpaces Metrics](#wsp-metrics)
+ [Dimensions for Amazon WorkSpaces Metrics](#wsp-metric-dimensions)
+ [Monitoring Example](#monitoring_example)

## Amazon WorkSpaces Metrics<a name="wsp-metrics"></a>

The `AWS/WorkSpaces` namespace includes the following metrics\.


| Metric | Description | Dimensions | Statistics Available | Units | 
| --- | --- | --- | --- | --- | 
| Available1 |  The number of WorkSpaces that returned a healthy status\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| Unhealthy1 |  The number of WorkSpaces that returned an unhealthy status\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| ConnectionAttempt2 |  The number of connection attempts\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| ConnectionSuccess2 |  The number of successful connections\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| ConnectionFailure2 |  The number of failed connections\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| SessionLaunchTime2 | The amount of time it takes to initiate a WorkSpaces session\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Second \(time\) | 
| InSessionLatency2 | The round trip time between the WorkSpaces client and the WorkSpace\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Millisecond \(time\) | 
| SessionDisconnect2 | The number of connections that were closed, including user\-initiated and failed connections\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| UserConnected3 | The number of WorkSpaces that have a user connected\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| Stopped | The number of WorkSpaces that are stopped\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| Maintenance4 | The number of WorkSpaces that are under maintenance\. |  `DirectoryID` `WorkspaceID`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 

1 Amazon WorkSpaces periodically sends status requests to a WorkSpace\. A WorkSpace is marked `Available` when it responds to these requests, and `Unhealthy` when it fails to respond to these requests\. These metrics are available at a per\-WorkSpace granularity, and also aggregated for all WorkSpaces in an organization\. 

2 Amazon WorkSpaces records metrics on connections made to each WorkSpace\. These metrics are emitted after a user has successfully authenticated via the WorkSpaces client and the client then initiates a session\. The metrics are available at a per\-WorkSpace granularity, and also aggregated for all WorkSpaces in a directory\.

3 Amazon WorkSpaces periodically sends connection status requests to a WorkSpace\. Users are reported as connected when they are actively using their sessions\. This metric is available at a per\-WorkSpace granularity, and is also aggregated for all WorkSpaces in an organization\.

4 This metric applies to WorkSpaces that are configured with an AutoStop running mode\. If you have maintenance enabled for your WorkSpaces, this metric captures the number of WorkSpaces that are currently under maintenance\. This metric is available at a per\-WorkSpace granularity, which describes when a WorkSpace went into maintenance and when it was removed\.

## Dimensions for Amazon WorkSpaces Metrics<a name="wsp-metric-dimensions"></a>

To filter the metric data, use the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| DirectoryId | Filters the metric data to the WorkSpaces in the specified directory\. The form of the directory ID is d\-XXXXXXXXXX\. | 
| WorkspaceId | Filters the metric data to the specified WorkSpace\. The form of the workspace ID is ws\-XXXXXXXXXX\. | 

## Monitoring Example<a name="monitoring_example"></a>

The following example demonstrates how you can use the AWS CLI to respond to a CloudWatch alarm and determine which WorkSpaces in a directory have experienced connection failures\.

**To respond to a CloudWatch alarm**

1. Determine which directory the alarm applies to using the [describe\-alarms](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/describe-alarms.html) command\.

   ```
   aws cloudwatch describe-alarms --state-value "ALARM"
   
   {
     "MetricAlarms": [
       {
         ...
         "Dimensions": [
           {
             "Name": "DirectoryId",
             "Value": "directory_id"
           }
         ],
         ...
       }
     ]
   }
   ```

1. Get the list of WorkSpaces in the specified directory using the [describe\-workspaces](https://docs.aws.amazon.com/cli/latest/reference/workspaces/describe-workspaces.html) command\.

   ```
   aws workspaces describe-workspaces --directory-id directory_id
   
   {
     "Workspaces": [
       {
         ...
         "WorkspaceId": "workspace1_id",
         ...
       },
       {
         ...
         "WorkspaceId": "workspace2_id",
         ...
       },
       {
         ...
         "WorkspaceId": "workspace3_id",
         ...
       }
     ]
   }
   ```

1. Get the CloudWatch metrics for each WorkSpace in the directory using the [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) command\.

   ```
   aws cloudwatch get-metric-statistics \
   --namespace AWS/WorkSpaces \
   --metric-name ConnectionFailure \
   --start-time 2015-04-27T00:00:00Z \
   --end-time 2015-04-28T00:00:00Z \
   --period 3600 \
   --statistics Sum \
   --dimensions "Name=WorkspaceId,Value=workspace_id"
   
   {
     "Datapoints" : [
       {
         "Timestamp": "2015-04-27T00:18:00Z",
         "Sum": 1.0,
         "Unit": "Count"
       },
       {
         "Timestamp": "2014-04-27T01:18:00Z",
         "Sum": 0.0,
         "Unit": "Count"
       }
     ],
     "Label" : "ConnectionFailure"
   }
   ```