# Monitor Your WorkSpaces Using CloudWatch Events<a name="cloudwatch-events"></a>

You can use events from Amazon CloudWatch Events to view, search, download, archive, analyze, and respond to successful logins to your WorkSpaces\. For example, you can use events for the following purposes:
+ Store or archive WorkSpaces login events as logs for future reference, analyze the logs to look for patterns, and take action based on those patterns\.
+ Use the WAN IP address to determine where users are logged in from, and then use policies to allow users access only to files or data from WorkSpaces that meet the access criteria found in the CloudWatch Event type of `WorkSpaces Access`\.
+ Analyze login data, which is available in near real\-time, and perform automated actions by using AWS Lambda\. 
+ Use policy controls to block access to files and applications from unauthorized IP addresses\.

For more information about events, see the [Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)\.

## WorkSpaces Events<a name="workspaces-event-types.title"></a>

Amazon WorkSpaces client applications send WorkSpaces Access events to CloudWatch Events when a user successfully logs in to a WorkSpace\. All Amazon WorkSpaces clients send these events\.

Events are represented as JSON objects\. The following is example data for a `WorkSpaces Access` event\.

```
{
    "version": "0",
    "id": "64ca0eda-9751-dc55-c41a-1bd50b4fc9b7",
    "detail-type": "WorkSpaces Access",
    "source": "aws.workspaces",
    "account": "123456789012",
    "time": "2018-07-01T17:53:06Z",
    "region": "us-east-2",
    "resources": [],
    "detail": {
        "clientIpAddress": "192.0.2.3",
        "actionType": "successfulLogin",
        "workspacesClientProductName": "WorkSpaces Desktop client",
        "loginTime": "2018-07-01T17:52:51.595Z",
        "clientPlatform": "Windows",
        "directoryId": "d-123456789",
        "workspaceId": "ws-xyskdga"
    }
}
```Event\-Specific Fields

`clientIpAddress`  
The WAN IP address of the client application\. For PCoIP zero clients, this is the IP address of the Teradici auth client\.

`actionType`  
This value is always `successfulLogin`\.

`workspacesClientProductName`  
+ `WorkSpaces Desktop client` — Windows, macOS, and Linux clients
+ `Amazon WorkSpaces Mobile client` — iOS client
+ `WorkSpaces Mobile client` — Android clients
+ `WorkSpaces Chrome client` — Chromebook client
+ `WorkSpaces Web client` — Web Access client
+ `Teradici PCoIP Zero Client, Teradici PCoIP Desktop Client, or Dell Wyse PCoIP Client ` — Zero Client

`loginTime`  
The time at which the user logged in to the WorkSpace\.

`clientPlatform`  
+ `Android`
+ `Chrome`
+ `iOS`
+ `Linux`
+ `OSX`
+ `Windows`
+ `Teradici PCoIP Zero Client and Tera2`
+ `Web`

`directoryId`  
The identifier of the directory for the WorkSpace\.

`workspaceId`  
The identifier of the WorkSpace\.

## Create a Rule to Handle WorkSpaces Events<a name="create-event-rule.title"></a>

Use the following procedure to create a CloudWatch Events rule to handle the WorkSpaces events\.

**To create a rule to handle WorkSpaces events**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**\.

1. Choose **Create rule**\.

1. For **Event Source**, do the following:

   1. Choose **Event Pattern** and **Build event pattern to match events by service** \(the default\)\.

   1. For **Service Name**, choose **WorkSpaces**\.

   1. For **Event Type**, choose **WorkSpaces Access**\.

1. For **Targets**, choose **Add target**, and then choose the service that is to act when a WorkSpaces event is detected\. Provide any information required by this service\.

1. Choose **Configure details**\. For **Rule definition**, enter a name and description\.

1. Choose **Create rule**\.