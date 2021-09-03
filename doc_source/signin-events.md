# Understanding AWS sign\-in events for smart card users<a name="signin-events"></a>

AWS CloudTrail logs successful and unsuccessful sign\-in events for smart card users\. This includes sign\-in events that are captured each time a user is prompted to solve a specific credential challenge or factor, as well as the status of that particular credential verification request\. A user is signed in only after completing all required credential challenges, which results in a `UserAuthentication` event being logged\.

The following table captures each of the sign\-in CloudTrail event names and their purposes\.


| Event name | Event purpose | 
| --- | --- | 
| `CredentialChallenge` |  Notifies that AWS sign\-in has requested that the user solve a specific credential challenge and specifies the `CredentialType` that is required \(for example, SMARTCARD\)\.  | 
| `CredentialVerification` |  Notifies that the user has attempted to solve a specific `CredentialChallenge` request, and specifies whether that credential has succeeded or failed\.  | 
| `UserAuthentication` |  Notifies that all authentication requirements that the user was challenged with have been successfully completed and that the user was successfully signed in\. When users fail to successfully complete the required credential challenges, no `UserAuthentication` event is logged\.  | 

The following table captures additional useful event data fields contained within specific sign\-in CloudTrail events\.


| Event name | Event purpose | Sign\-in event applicability | Example values | 
| --- | --- | --- | --- | 
| `AuthWorkflowID` |  Correlates all events emitted across an entire sign\-in sequence\. For each user sign\-in, multiple events can be emitted by AWS sign\-in\.  |  `CredentialChallenge`, `CredentialVerification`, `UserAuthentication`  |  "AuthWorkflowID": "9de74b32\-8362\-4a01\-a524\-de21df59fd83"  | 
| `CredentialType` |  Notifies that the user has attempted to solve a specific `CredentialChallenge` request and specifies whether that credential has succeeded or failed\.  |  `CredentialChallenge`, `CredentialVerification`, `UserAuthentication`  |  CredentialType": "SMARTCARD" \(possible values today: SMARTCARD\)  | 
| `LoginTo` |  Notifies that all authentication requirements that the user was challenged with have been successfully completed and that the user was successfully signed in\. When users fail to successfully complete the required credential challenges, no `UserAuthentication` event is logged\.  |  `UserAuthentication`  |  "LoginTo": "https://skylight\.local“  | 

## Example events for AWS sign\-in scenarios<a name="example-event-signin"></a>

The following examples show the expected sequence of CloudTrail events for different sign\-in scenarios\.

**Topics**
+ [Successful sign\-in when authenticating with smart card](#successful-signin)
+ [Failed sign\-in when authenticating with only a smart card](#failed-signin)

### Successful sign\-in when authenticating with smart card<a name="successful-signin"></a>

The following sequence of events captures an example of a successful smart card sign\-in\.

**CredentialChallenge**  

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "Unknown",
        "principalId": "509318101470",
        "arn": "",
        "accountId": "509318101470",
        "accessKeyId": ""
    },
    "eventTime": "2021-07-30T17:23:29Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "CredentialChallenge", 
    "awsRegion": "us-east-1", 
    "sourceIPAddress": "AWS Internal", 
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.164 Safari/537.36",
    "requestParameters": null,
    "responseElements": null,
    "additionalEventData": {
        "AuthWorkflowID": "6602f256-3b76-4977-96dc-306a7283269e",
        "CredentialType": "SMARTCARD"
    },
    "requestID": "65551a6d-654a-4be8-90b5-bbfef7187d3a",
    "eventID": "fb603838-f119-4304-9fdc-c0f947a82116",
    "readOnly": false,
    "eventType": "AwsServiceEvent",
    "managementEvent": true,
    "eventCategory": "Management", 
    "recipientAccountId": "509318101470", 
    "serviceEventDetails": {
        CredentialChallenge": "Success"
    }
}
```

**Successful CredentialVerification**  

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "Unknown",
        "principalId": "509318101470",
        "arn": "",
        "accountId": "509318101470",
        "accessKeyId": ""
    },
    "eventTime": "2021-07-30T17:23:39Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "CredentialVerification",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "AWS Internal",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.164 Safari/537.36",
    "requestParameters": null,
    "responseElements": null,
    "additionalEventData": {
        "AuthWorkflowID": "6602f256-3b76-4977-96dc-306a7283269e",
        "CredentialType": "SMARTCARD"
    },
    "requestID": "81869203-1404-4bf2-a1a4-3d30aa08d8d5",
    "eventID": "84c0a2ff-413f-4d0f-9108-f72c90a41b6c",
    "readOnly": false,
    "eventType": "AwsServiceEvent",
    "managementEvent": true,
    "eventCategory": "Management",
    "recipientAccountId": "509318101470",
    "serviceEventDetails": {
        CredentialVerification": "Success"
    }
}
```

**Successful UserAuthentication**  

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "Unknown",
        "principalId": "509318101470",
        "arn": "",
        "accountId": "509318101470",
        "accessKeyId": ""
    },
    "eventTime": "2021-07-30T17:23:39Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "UserAuthentication", 
    "awsRegion": "us-east-1", 
    "sourceIPAddress": "AWS Internal", 
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.164 Safari/537.36",
    "requestParameters": null,
    "responseElements": null,
    "additionalEventData": {
        "AuthWorkflowID": "6602f256-3b76-4977-96dc-306a7283269e", 
        "LoginTo": "https://skylight.local", 
        "CredentialType": "SMARTCARD"
    },
    "requestID": "81869203-1404-4bf2-a1a4-3d30aa08d8d5", 
    "eventID": "acc0dba8-8e8b-414b-a52d-6b7cd51d38f6", 
    "readOnly": false,
    "eventType": "AwsServiceEvent", 
    "managementEvent": true,
    "eventCategory": "Management", 
    "recipientAccountId": "509318101470", 
    "serviceEventDetails": {
        UserAuthentication": "Success"
    }
}
```

### Failed sign\-in when authenticating with only a smart card<a name="failed-signin"></a>

The following sequence of events captures an example of failed smart card sign\-in\.

**CredentialChallenge**  

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "Unknown",
        "principalId": "509318101470",
        "arn": "",
        "accountId": "509318101470",
        "accessKeyId": ""
    },
    "eventTime": "2021-07-30T17:23:06Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "CredentialChallenge", 
    "awaRegion": "us-east-1", 
    "sourceIPAddress": "AWS Internal", 
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.164 Safari/537.36",
    "requestParameters": null,
    "responseElements": null,
    "additionalEventData": {
        "AuthWorkflowID": "73dfd26b-f812-4bd2-82e9-0b2abb358cdb",
        "CredentialType": "SMARTCARD"
    },
    "requestID": "73eb499d-91a8-4c18-9c5d-281fd45ab50a",
    "eventID": "f30a50ec-71cf-415a-a5ab-e287edc800da",
    "readOnly": false,
    "eventType": "AwsServiceEvent",
    "managementEvent": true,
    "eventCategory": "Management", 
    "recipientAccountId": "509318101470", 
    "serviceEventDetails": {
        CredentialChallenge": "Success"
    }
}
```

**Failed CredentialVerification**  

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "Unknown",
        "principalId": "509318101470",
        "arn": "",
        "accountId": "509318101470",
        "accessKeyId": ""
    },
    "eventTime": "2021-07-30T17:23:13Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "CredentialVerification",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "AWS Internal",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.164 Safari/537.36",
    "requestParameters": null,
    "responseElements": null,
    "additionalEventData": {
        "AuthWorkflowID": "73dfd26b-f812-4bd2-82e9-0b2abb358cdb",
        "CredentialType": "SMARTCARD"
    },
    "requestID": "051ca316-0b0d-4d38-940b-5fe5794fda03",
    "eventID": "4e6fbfc7-0479-48da-b7dc-e875155a8177",
    "readOnly": false,
    "eventType": "AwsServiceEvent",
    "managementEvent": true,
    "eventCategory": "Management", 
    "recipientAccountId": "509318101470", 
    "serviceEventDetails": {
        CredentialVerification": "Failure"
    }
}
```