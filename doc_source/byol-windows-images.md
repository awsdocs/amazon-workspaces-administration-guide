# Bring Your Own Windows Desktop Licenses<a name="byol-windows-images"></a>

If your licensing agreement with Microsoft allows it, you can use your Windows 10 Enterprise or Windows 10 Pro desktop licenses for your WorkSpaces\. To do this, you must Bring Your Own License \(BYOL\) and provide a Windows 10 license that meets the following requirements\. To stay compliant with Microsoft licensing terms, AWS runs your BYOL WorkSpaces on hardware that is dedicated to you in the AWS Cloud\. By bringing your own license, you can provide a consistent experience for your users\. For more information, see [Amazon WorkSpaces Pricing](https://aws.amazon.com/workspaces/pricing/)\.

**Important**  
Image creation is not supported on Windows 10 systems that have been upgraded from one version of Windows 10 to a newer version of Windows 10 \(a Windows feature/version upgrade\)\. However, Windows cumulative or security updates are supported by the WorkSpaces image\-creation process\. 

To get started, open the Amazon WorkSpaces console and choose **Account Settings** to enable your account for BYOL\.

**Topics**
+ [Requirements](#windows_images_prerequisites)
+ [Windows Versions That Are Supported for BYOL](#windows_images_supported_versions)
+ [Step 1: Enable BYOL for Your Account by Using the Amazon WorkSpaces Console](#windows_images_enable_byol)
+ [Step 2: Run the BYOL Checker PowerShell Script on a Windows VM](#windows_images_run_byol_checker_script)
+ [Step 3: Export the VM from Your Virtualization Environment](#windows_images_create_image_byol)
+ [Step 4: Import the VM as an Image into EC2](#windows_images_import_image_ec2_byol)
+ [Step 5: Create a BYOL Image by Using the Amazon WorkSpaces Console](#windows_images_create_byol_image_console)
+ [Step 6: Create a Custom Bundle from the BYOL Image](#windows_images_create_custom_bundle_byol)
+ [Step 7: Register a Directory for Dedicated WorkSpaces](#windows_images_dedicate_directory_for_byol)
+ [Step 8: Launch Your BYOL WorkSpaces](#windows_images_launch_byol_workspaces)

## Requirements<a name="windows_images_prerequisites"></a>

Before you begin, verify the following:
+ Your Microsoft licensing agreement allows Windows to be run in a virtual hosted environment\.
+ If you will be using non\-GPU\-enabled bundles \(bundles other than Graphics and GraphicsPro\), verify that you will use a minimum of 200 Amazon WorkSpaces\. These 200 WorkSpaces can be any mix of AlwaysOn and AutoStop WorkSpaces\. Using a minimum of 200 WorkSpaces is a requirement for running your Amazon WorkSpaces on dedicated hardware\. Running your Amazon WorkSpaces on dedicated hardware is necessary to comply with Microsoft licensing requirements\. The dedicated hardware is provisioned on the AWS side, so your VPC can stay on default tenancy\.

  If you plan to use GPU\-enabled \(Graphics and GraphicsPro\) bundles, verify that you will run a minimum of 4 AlwaysOn or 20 AutoStop GPU\-enabled WorkSpaces in a Region per month on dedicated hardware\.
+ Amazon WorkSpaces can use a management interface in the /16 IP address range\. The management interface is connected to a secure Amazon WorkSpaces management network used for interactive streaming\. This allows Amazon WorkSpaces to manage your WorkSpaces\. For more information, see [Network Interfaces](workspaces-port-requirements.md#network-interfaces)\. You must reserve a /16 netmask from at least one of the following IP address ranges for this purpose:
  + 10\.0\.0\.0/8
  + 100\.64\.0\.0/10
  + 172\.16\.0\.0/12
  + 192\.168\.0\.0/16
  + 198\.18\.0\.0/15
**Note**  
As you adopt the WorkSpaces service, the available management interface IP address ranges frequently change\. To determine which ranges are currently available, run the [ list\-available\-management\-cidr\-ranges](https://docs.aws.amazon.com/cli/latest/reference/workspaces/list-available-management-cidr-ranges.html) CLI command\.
+ You have a virtual machine \(VM\) that runs a supported 64\-bit version of Windows\. For a list of supported versions, see the next section in this topic, [Windows Versions That Are Supported for BYOL](#windows_images_supported_versions)\. The VM must also meet these requirements: 
  + The Windows operating system must be activated against your key management servers\.
  + The Windows operating system must have **English \(United States\)** as the primary language\.
  + No software beyond what is included with Windows can be installed on the VM\. You can add additional software, such as an antivirus solution, when you later create a custom image\.
  + Do not customize the default user profile \(`C:\Users\Default`\) or make other customizations before creating an image\. All customizations should be made after image creation\. We recommend making any customizations to the user profile through Group Policy Objects \(GPOs\) and applying them after image creation\. This is because customizations done through GPOs can be easily modified or rolled back and are less prone to error than customizations made to the default user profile\.
  + You must create a **WorkSpaces\_BYOL** account with local administrator access before you share the image\. The password for this account might be required later, so make note of it\.
  + The VM must be on a single volume with a maximum size of 70 GB and at least 10 GB of free space\.
  + Your VM must run Windows PowerShell version 4 or later\.
+ Make sure that you have installed the latest Microsoft Windows patches before running the BYOL Checker PowerShell script in [Step 2](#windows_images_run_byol_checker_script) later in this topic\.

## Windows Versions That Are Supported for BYOL<a name="windows_images_supported_versions"></a>

Your VM must run one of the following Windows versions:
+ Windows 10 Version 1803 \(April 2018 Update\) 
+ Windows 10 Version 1809 \(October 2018 Update\) 
+ Windows 10 Version 1903 \(May 2019 Update\)
+ Windows 10 Version 1909 \(November 2019 Update\)

All supported OS versions support all of the compute types available in the AWS Region where you're using WorkSpaces\. Versions of Windows that are no longer supported by Microsoft are not guaranteed to work and are not supported by AWS Support\.

## Step 1: Enable BYOL for Your Account by Using the Amazon WorkSpaces Console<a name="windows_images_enable_byol"></a>

To enable BYOL for your account, you must specify a management network interface\. This interface is connected to a secure Amazon WorkSpaces management network\. It is used for interactive streaming of the WorkSpace desktop to Amazon WorkSpaces clients, and to allow Amazon WorkSpaces to manage the WorkSpace\.

**Note**  
The steps in this procedure for enabling BYOL for your account need to be performed only once per account, per Region\.

**To enable BYOL for your account by using the Amazon WorkSpaces console**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Account Settings**\. If your account is not currently eligible for BYOL, a message provides guidance for next steps\.

1. Under **Bring Your Own License \(BYOL\)**, in the **Management network interface IP address range** area, choose an IP address range, and then choose **Display available CIDR blocks**\.

   Amazon WorkSpaces searches for and displays available IP address ranges as IPv4 CIDR blocks, within the range that you specify\. If you require a specific IP address range, you can edit the search range\.
**Important**  
After you specify an IP address range, you cannot modify it\. Make sure to specify an IP address range that doesn't conflict with the ranges used by your internal network\. If you have any question about which range to specify, contact your AWS account manager or sales representative, or contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/) before proceeding\.

1. Choose the CIDR block that you want from the list of results, and then choose **Enable BYOL**\.

   This process may take several hours\. While Amazon WorkSpaces is enabling your account for BYOL, proceed to the next step\. 

## Step 2: Run the BYOL Checker PowerShell Script on a Windows VM<a name="windows_images_run_byol_checker_script"></a>

After you enable BYOL for your account, you must confirm that your VM meets the requirements for BYOL\. To do so, perform these steps to download and run the Amazon WorkSpaces BYOL Checker PowerShell script\. The script performs a series of tests on the VM that you plan to use to create your image\. 

**Important**  
The VM must pass all tests before you can use it for BYOL\.

**To download the BYOL Checker script**

Before you download and run the BYOL Checker script, verify that the latest Windows security updates are installed on your VM\. While this script runs, it disables the Windows Update service\. 

1. Download the BYOL Checker script \.zip file from [https://tools\.amazonworkspaces\.com/BYOLChecker\.zip](https://tools.amazonworkspaces.com/BYOLChecker.zip) to your `Downloads` folder\.

1. In your `Downloads` folder, create a `BYOL` folder\.

1. Extract the files from `BYOLChecker.zip` and copy them to the `Downloads\BYOL` folder\.

1. Delete the `Downloads\BYOLChecker.zip` folder so that only the extracted files remain\.

Perform these steps to run the BYOL Checker script\.

**To run the BYOL Checker script**

1. From the Windows desktop, open Windows PowerShell\. Choose the Windows **Start** button, right\-click **Windows PowerShell**, and choose **Run as administrator**\. If you are prompted by User Account Control to choose whether you want PowerShell to make changes to your device, choose **Yes**\. 

1. At the PowerShell command prompt, change to the directory where the BYOL Checker script is located\. For example, if the script is located in the `Downloads\BYOL` directory, enter the following command and press **Enter**:

   `cd C:\Users\username\Downloads\BYOL`

1. Enter the following command to update the PowerShell execution policy on the computer\. Doing so allows the BYOL Checker script to run: 

   `Set-ExecutionPolicy Unrestricted`

1. When prompted to confirm whether to change the PowerShell execution policy, enter A to specify Yes to All\.

1. Enter the following command to run the BYOL Checker script:

   `.\BYOLChecker.ps1`

1. If a security notification appears, press the **R** key to Run Once\.

1. <a name="step_begin_tests"></a>In the **Amazon WorkSpaces Image Validation** dialog box, choose **Begin Tests**\.

1. <a name="step_resolve_issues"></a>After each test is completed, you can view the status of the test\. For any test with a status of **FAILED**, choose **Info** to display information about how to resolve the issue that caused the failure\. If any tests display a status of **WARNING**, choose the **Fix All Warnings** button\.

1. If applicable, resolve any issues that cause test failures and warnings, and repeat [Step 7](#step_begin_tests) and [Step 8](#step_resolve_issues) until the VM passes all tests\. All failures and warnings must be resolved before you export the VM\.

1. The BYOL script checker generates two log files, `BYOLPrevalidationlogYYYY-MM-DD_HHmmss.txt` and `ImageInfo.text`\. These files are located in the directory that contains the BYOL Checker script files\.
**Tip**  
Do not delete these files\. If an issue occurs, they might be helpful in troubleshooting\.

1. After your VM passes all tests, you get a **Validation Successful** message\. Review the VM locale settings displayed in the tool\. To update the locale settings, follow [these instructions](https://docs.microsoft.com/previous-versions/windows/hardware/previsioning-framework/dn965674(v=vs.85)) in the Microsoft documentation and run the BYOL Checker script again\.

1. <a name="step_create_VM_snapshot"></a>Shut down the VM and create a snapshot of it\.

1. Choose **Run Sysprep**\. If Sysprep is successful, your VM that you exported after [Step 12](#step_create_VM_snapshot) can be imported into EC2\. Otherwise, review the Sysprep logs, roll back to the snapshot taken at [Step 12](#step_create_VM_snapshot), resolve the reported issues, take a new snapshot, and run the BYOL Checker script again\.

   The most common reason that Sysprep fails is that the Modern AppX Packages are not uninstalled for all users\. Use the Remove\-AppxPackage PowerShell cmdlet to remove the AppX Packages\.

1. After you have successfully created your image, you can remove the **WorkSpaces\_BYOL** account\.

## Step 3: Export the VM from Your Virtualization Environment<a name="windows_images_create_image_byol"></a>

To create an image for BYOL, you must first export the VM from your virtualization environment\. The VM must be on a single volume with a maximum size of 70 GB and at least 10 GB of free space\. For more information, see the documentation for your virtualization environment and [Export Your VM from its Virtualization Environment](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html#export-vm-image) in the *VM Import/Export User Guide*\.

## Step 4: Import the VM as an Image into EC2<a name="windows_images_import_image_ec2_byol"></a>

After you export your VM, review the requirements for importing Windows operating systems from a VM\. Take action as needed\. For more information, see [VM Import/Export Requirements](https://docs.aws.amazon.com/vm-import/latest/userguide/vmie_prereqs.html)\.

**Note**  
Importing a VM with an encrypted disk is not supported\.

Import your VM into Amazon EC2 as an Amazon Machine Image \(AMI\)\. Use one of the following methods:
+ Use the import\-image command with the AWS Command Line Interface \(AWS CLI\)\. For more information, see [import\-image](https://docs.aws.amazon.com/cli/latest/reference/ec2/import-image.html) in the *AWS CLI Command Reference*\.
+ Use the ImportImage API operation\. For more information, see [ImportImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ImportImage.html) in the *Amazon EC2 API Reference*\.

For more information, see [Importing a VM as an Image](https://docs.aws.amazon.com/vm-import/latest/userguide/vmimport-image-import.html) in the *VM Import/Export User Guide*\.

## Step 5: Create a BYOL Image by Using the Amazon WorkSpaces Console<a name="windows_images_create_byol_image_console"></a>

Perform these steps to create an Amazon WorkSpaces BYOL image\. 

**Note**  
To perform this procedure, verify that you have permissions to:  
Call Amazon WorkSpaces **ImportWorkspaceImage**\.
Call EC2 **DescribeImages** on the EC2 image that you want to use to create the BYOL image\.
Call EC2 **ModifyImageAttribute** on the EC2 image that you want to use to create the BYOL image\.
For more information, see [Changing Permissions for an IAM User ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.  
To create a Graphics or GraphicsPro bundle from your image, contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/) to get your account added to the allow list\. After your account is on the allow list, you can use the AWS CLI import\-workspace\-image command to ingest the Graphics or GraphicsPro image\. For more information, see [import\-workspace\-image](https://docs.aws.amazon.com/cli/latest/reference/workspaces/import-workspace-image.html) in the *AWS CLI Command Reference*\.

**To create an image from the Windows VM**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Images**\.

1. Choose **Actions**, **Create BYOL Image**\. 

1. In the **Create BYOL Image** dialog box, do the following: 
   + For **AMI ID**, click the **EC2 Console** link, and choose the EC2 image that you imported as described in the previous section \(Step 4: Import the VM as an Image into EC2\)\. The image name must begin with `ami-` and be followed by the identifier for the AMI \(for example, `ami-5731123e`\)\.
   + For **BYOL image name**, enter a unique name for the image\.
   + For **Image description**, enter a description to help you quickly identify the image\.
   + For **Ingestion process**, choose the appropriate bundle type \(either **Regular**, **Graphics**, or **GraphicsPro**\)\. For non\-GPU\-enabled bundles \(bundles other than Graphics or GraphicsPro\), choose **Regular**\.

1. Choose **Create**\.

   While your image is being created, the image status in the image registry of the console appears as **Pending**\. If the image validation does not succeed, the console displays an error code\. When the image creation is complete, the status changes to** Available**\.

## Step 6: Create a Custom Bundle from the BYOL Image<a name="windows_images_create_custom_bundle_byol"></a>

After your BYOL image is created, you can use the image to create a custom bundle\. For information, see [Create a Custom WorkSpaces Image and Bundle](create-custom-bundle.md)\.

## Step 7: Register a Directory for Dedicated WorkSpaces<a name="windows_images_dedicate_directory_for_byol"></a>

To use BYOL images for WorkSpaces, you must register a directory for this purpose\. To do so, perform these steps\. 

**To register a directory for dedicated WorkSpaces**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Register**\.

1. In the **Register directory** dialog box, for **Enable Dedicated WorkSpaces**, choose **Yes**\.

1. Choose **Register**\.

If you have already registered AWS Directory Service for Microsoft Active Directory or an AD Connector directory for WorkSpaces that does not run on dedicated hardware, you can set up a new Microsoft Active Directory or AD Connector directory for this purpose\. You can also deregister the directory and then reregister it as a directory for dedicated WorkSpaces\. To do so, perform these steps\.

**Note**  
You can only perform this procedure if no WorkSpaces are associated with the directory\.

**To deregister a directory and reregister it for dedicated WorkSpaces**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. Terminate existing WorkSpaces\.

1. In the navigation pane, choose **Directories**\.

1. Select the directory and choose **Actions**, **Deregister**\.

1. When prompted for confirmation, choose **Deregister**\.

1. Select the directory again and choose **Actions**, **Register**\.

1. In the **Register directory** dialog box, for **Enable Dedicated WorkSpaces**, choose **Yes**\.

1. Choose **Register**\.

## Step 8: Launch Your BYOL WorkSpaces<a name="windows_images_launch_byol_workspaces"></a>

After you register a directory for dedicated WorkSpaces, you can launch your BYOL WorkSpaces in this directory\. For information about how to launch WorkSpaces, see [Launch a Virtual Desktop Using Amazon WorkSpaces](launch-workspaces-tutorials.md)\.