# Bring Your Own Windows Desktop Images<a name="byol-windows-images"></a>

If your licensing agreement with Microsoft allows it, you can use your Windows 7 or Windows 10 Enterprise or Professional desktop images for your WorkSpaces\. To do this, you must bring your own Microsoft Windows License \(BYOL\) and provide a Windows 7 or Windows 10 image that meets the following requirements\. To stay compliant with Microsoft licensing terms, you must run your Amazon WorkSpaces on hardware that is dedicated to you on the AWS cloud\. By bringing your own license, you can save money and provide a consistent experience for your users\. For more information, see [Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

**Important**  
If you plan to create an image from a computer running the Windows 10 operating system, do so from one that hasn't been upgraded\. Image creation from Windows 10 computers that have been upgraded isn't supported because running Microsoft Sysprep on an upgraded operating system isn't recommended\.

## Requirements<a name="windows_images_prerequisties"></a>

Before you begin, do the following:
+ Review the prerequisites and limitations for importing Windows operating systems from a VM\. For more information, see [Importing a VM as an Image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html)\.
+ Verify that your Microsoft licensing agreement allows Windows to be run in a virtual hosted environment\.
+ Verify that your Windows operating system is 64\-bit and activated against your key management servers\.
+ Verify that your Windows operating system has "English \(United States\)" as the primary language\.
+ Verify that your base image contains no additional software beyond what comes with Windows 7 or Windows 10\. You can add additional software, like an anti\-virus solution, and create a custom image later on\.
+ Run Sysprep to prepare the image\.
+ Create the following account with local administrator access before you share the image: WorkSpaces\_BYOL\. The password for this account will be requested at a later time\.
+ Verify that the image that you are importing is on a single volume that is smaller than 80 GB, and the format of the image is OVA\.
+ Amazon WorkSpaces uses a management interface\. It's connected to a secure Amazon WorkSpaces management network used for interactive streaming\. This allows Amazon WorkSpaces to manage your WorkSpaces\. For more information, see [Network Interfaces](workspaces-port-requirements.md#network-interfaces)\. Confirm that Amazon WorkSpaces can use a /16 range for the management interface in at least one of the following IP ranges:
  + 10\.0\.0\.0/8
  + 100\.64\.0\.0/10
  + 172\.16\.0\.0/12
  + 192\.168\.0\.0/16
  + 198\.18\.0\.0/15
+ You must use a minimum of 200 Amazon WorkSpaces in order to run your Amazon WorkSpaces on dedicated hardware\. Running your Amazon WorkSpaces on dedicated hardware is necessary to comply with Microsoft licensing requirements\. 

## Getting Started<a name="windows_images_get_started"></a>

To get started, contact your AWS account manager or [Sales representative](https://aws.amazon.com/contact-us/aws-sales/), or create a [Technical Support case](https://console.aws.amazon.com/support/home?region=us-east-1#/case/create) for Amazon WorkSpaces\. Your contact will verify whether you have enough dedicated capacity allocated to your account and guide you through BYOL setup\.