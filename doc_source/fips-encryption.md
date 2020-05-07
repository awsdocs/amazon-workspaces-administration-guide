# Set Up Amazon WorkSpaces for FedRAMP Authorization or DoD SRG Compliance<a name="fips-encryption"></a>

To comply with the [Federal Risk and Authorization Management Program \(FedRAMP\)](https://aws.amazon.com/compliance/fedramp/) or the [Department of Defense \(DoD\) Cloud Computing Security Requirements Guide \(SRG\)](https://aws.amazon.com/compliance/dod/), you must configure Amazon WorkSpaces to use Federal Information Processing Standards \(FIPS\) endpoint encryption at the directory level\. You must also use a US AWS Region that has FedRAMP authorization or is DoD SRG compliant\.

The level of FedRAMP authorization \(Moderate or High\) or DoD SRG Impact Level \(2, 4, or 5\) depends on the US AWS Region in which Amazon WorkSpaces is being used\. For the levels of FedRAMP authorization and DoD SRG compliance that apply to each Region, see [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)\.

**Requirements**
+ You must create your WorkSpaces in a [US AWS Region that has FedRAMP authorization or is DoD SRG\-compliant](https://aws.amazon.com/compliance/services-in-scope/)\.
+ The WorkSpaces directory must be configured to use **FIPS 140\-2 Validated Mode** for endpoint encryption\.
**Note**  
To use the **FIPS 140\-2 Validated Mode** setting, the WorkSpaces directory must either be new, or all existing WorkSpaces in the directory must be using **FIPS 140\-2 Validated Mode** for endpoint encryption\. Otherwise, you cannot use this setting, and therefore the WorkSpaces that you create will not comply with FedRAMP or DoD security requirements\.
+ Users must access their WorkSpaces from one of the following WorkSpaces client applications:
  + Windows: 2\.4\.3 or later
  + macOS: 2\.4\.3 or later
  + Linux: 3\.0\.0 or later
  + iOS: 2\.4\.1 or later
  + Android: 2\.4\.1 or later
  + Fire Tablet: 2\.4\.1 or later
  + ChromeOS: 2\.4\.1 or later

**To use FIPS endpoint encryption**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Verify that the directory where you want to create FedRAMP\-authorized and DoD SRG\-compliant WorkSpaces does not have any existing WorkSpaces associated with it\. If there are WorkSpaces associated with the directory and the directory is not already enabled to use FIPS 140\-2 Validated Mode, either terminate the WorkSpaces or create a new directory\.

1. Choose the directory that meets the above criteria, and then choose **Actions**, **Update Details**\.

1. On the **Update Directory Details** page, choose the arrow to expand the **Access Control Options** section\.

1. For **Endpoint Encryption**, choose **FIPS 140\-2 Validated Mode** instead of **TLS Encryption Mode \(Standard\)**\.

1. Choose **Update and Exit**\.

1. You can now create WorkSpaces from this directory that are FedRAMP authorized and DoD SRG compliant\. To access these WorkSpaces, users must use one of the WorkSpaces client applications listed earlier in the [Requirements](#fedramp-requirements) section\.