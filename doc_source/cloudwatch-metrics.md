# Monitor your WorkSpaces using CloudWatch metrics<a name="cloudwatch-metrics"></a>

WorkSpaces and Amazon CloudWatch are integrated, so you can gather and analyze performance metrics\. You can monitor these metrics using the CloudWatch console, the CloudWatch command line interface, or programmatically using the CloudWatch API\. CloudWatch also allows you to set alarms when you reach a specified threshold for a metric\.

For more information about using CloudWatch and alarms, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/)\.

**Prerequisites**  
To get CloudWatch metrics, enable access on port 443 on the `AMAZON` subset in the `us-east-1` Region\. For more information, see [IP address and port requirements for WorkSpaces](workspaces-port-requirements.md)\.

**Topics**
+ [WorkSpaces metrics](#wsp-metrics)
+ [Dimensions for WorkSpaces metrics](#wsp-metric-dimensions)
+ [Monitoring example](#monitoring_example)

## WorkSpaces metrics<a name="wsp-metrics"></a>

The `AWS/WorkSpaces` namespace includes the following metrics\.


| Metric | Description | Dimensions | Statistics | Units | 
| --- | --- | --- | --- | --- | 
| `Available`1 |  The number of WorkSpaces that returned a healthy status\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `Unhealthy`1 |  The number of WorkSpaces that returned an unhealthy status\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `ConnectionAttempt`2,5 |  The number of connection attempts\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `ConnectionSuccess`2,5 |  The number of successful connections\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `ConnectionFailure`2,5 |  The number of failed connections\.  |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `SessionLaunchTime`2 | The amount of time it takes to initiate a WorkSpaces session\. |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Second \(time\) | 
| `InSessionLatency`2 | The round trip time between the WorkSpaces client and the WorkSpace\. |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Millisecond \(time\) | 
| `SessionDisconnect`2 | The number of connections that were closed, including user\-initiated and failed connections\. |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `UserConnected`3 | The number of WorkSpaces that have a user connected\. |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `Stopped` | The number of WorkSpaces that are stopped\. |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `Maintenance`4 | The number of WorkSpaces that are under maintenance\. |  `DirectoryId` `WorkspaceId`  | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `TrustedDeviceValidationAttempt`6 | The number of device authentication signature validation attempts\. | `DirectoryId` | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `TrustedDeviceValidationSuccess`6 | The number of successful device authentication signature validations\. | `DirectoryId` | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `TrustedDeviceValidationFailure`6 | The number of failed device authentication signature validations\.  | `DirectoryId` | Average, Sum, Maximum, Minimum, Data Samples | Count | 
| `TrustedDeviceCertificateDaysBeforeExpiration` | Days left before the root certificate associated with the directory is expired\. | `CertificateId` | Average, Sum, Maximum, Minimum, Data Samples | Count | 

1 WorkSpaces periodically sends status requests to a WorkSpace\. A WorkSpace is marked `Available` when it responds to these requests, and `Unhealthy` when it fails to respond to these requests\. These metrics are available at a per\-WorkSpace level of granularity, and also aggregated for all WorkSpaces in an organization\. 

2 WorkSpaces records metrics on connections made to each WorkSpace\. These metrics are emitted after a user has successfully authenticated via the WorkSpaces client and the client then initiates a session\. The metrics are available at a per\-WorkSpace level of granularity, and also aggregated for all WorkSpaces in a directory\.

3 WorkSpaces periodically sends connection status requests to a WorkSpace\. Users are reported as connected when they are actively using their sessions\. This metric is available at a per\-WorkSpace level of granularity, and is also aggregated for all WorkSpaces in an organization\.

4 This metric applies to WorkSpaces that are configured with an AutoStop running mode\. If you have maintenance enabled for your WorkSpaces, this metric captures the number of WorkSpaces that are currently under maintenance\. This metric is available at a per\-WorkSpace level of granularity, which describes when a WorkSpace went into maintenance and when it was removed\.

5 This metric is currently emitted only for PCoIP WorkSpaces\.

6 If the trusted devices feature is enabled for the directory, Amazon WorkSpaces uses certificate\-based authentication to determine whether a device is trusted\. When users attempt to access their WorkSpaces, these metrics are emitted to indicate successful or failed trusted device authentication\. These metrics are available at a per\-directory level of granularity, and only for the Amazon WorkSpaces Windows and macOS client applications\. 

## Dimensions for WorkSpaces metrics<a name="wsp-metric-dimensions"></a>

To filter the metric data, use the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `DirectoryId` | Filters the metric data to the WorkSpaces in the specified directory\. The form of the directory ID is `d-XXXXXXXXXX`\. | 
| `WorkspaceId` | Filters the metric data to the specified WorkSpace\. The form of the WorkSpace ID is `ws-XXXXXXXXXX`\. | 
| `CertificateId` | Filters the metric data to the specified root certificate associated with the directory\. The form of the certificate ID is `wsc-XXXXXXXXX`\. | 

## Monitoring example<a name="monitoring_example"></a>

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