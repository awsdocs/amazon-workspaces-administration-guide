# Encrypted WorkSpaces<a name="encrypt-workspaces"></a>

Amazon WorkSpaces is integrated with the AWS Key Management Service \(AWS KMS\)\. This enables you to encrypt storage volumes of WorkSpaces using customer master keys \(CMKs\)\. When you launch a WorkSpace, you can encrypt the root volume \(for Microsoft Windows, the C drive; for Linux, /\) and the user volume \(for Windows, the D drive; for Linux, /home\)\. Doing so ensures that the data stored at rest, disk I/O to the volume, and snapshots created from the volumes are all encrypted\.

**Note**  
In addition to encrypting your WorkSpaces, you can also use FIPS endpoint encryption in certain AWS US Regions\. For more information, see [Set Up Amazon WorkSpaces for FedRAMP Authorization or DoD SRG Compliance](fips-encryption.md)\.

**Topics**
+ [Prerequisites](#encryption_prerequisites)
+ [Limits](#encryption_limits)
+ [Overview of Amazon WorkSpaces Encryption Using AWS KMS](#kms-workspaces-overview)
+ [Amazon WorkSpaces Encryption Context](#kms-workspaces-encryption-context)
+ [Giving Amazon WorkSpaces Permission to Use a CMK on Your Behalf](#kms-workspaces-permissions)
+ [Encrypting WorkSpaces](#encrypt_workspace)
+ [Viewing Encrypted WorkSpaces](#maintain_encryption)

## Prerequisites<a name="encryption_prerequisites"></a>

You need an AWS KMS CMK before you can begin the encryption process\. This CMK can be either the [ AWS managed CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-managed-cmk) for Amazon WorkSpaces \(**aws/workspaces**\) or a symmetric [ customer managed CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk)\.
+ **AWS managed CMK** – The first time that you launch an unencrypted WorkSpace from the Amazon WorkSpaces console in a Region, Amazon WorkSpaces automatically creates an AWS managed CMK \(**aws/workspaces**\) in your account\. You can select this AWS managed CMK to encrypt the user and root volumes of your WorkSpace\. For details, see [Overview of Amazon WorkSpaces Encryption Using AWS KMS](#kms-workspaces-overview)\.

  You can view this AWS managed CMK, including its policies and grants, and can track its use in AWS CloudTrail logs, but you cannot use or manage this CMK\. Amazon WorkSpaces creates and manages this CMK\. Only Amazon WorkSpaces can use this CMK, and WorkSpaces can use it only to encrypt WorkSpaces resources in your account\. 

  AWS managed CMKs, including the one that Amazon WorkSpaces supports, are rotated every three years\. For details, see [Rotating Customer Master Keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html) in the *AWS Key Management Service Developer Guide*\.
+ **Customer managed CMK** – Alternatively, you can select a symmetric customer managed CMK that you created using AWS KMS\. You can view, use, and manage this CMK, including setting its policies\. For more information about creating CMKs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. For more information about creating CMKs using the AWS KMS API, see [Working with Keys](https://docs.aws.amazon.com/kms/latest/developerguide/programming-keys.html) in the *AWS Key Management Service Developer Guide*\.

  Customer managed CMKs are not automatically rotated unless you decide to enable automatic key rotation\. For details, see [Rotating Customer Master Keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html) in the *AWS Key Management Service Developer Guide*\.

**Important**  
When you rotate CMKs, you must keep both the original CMK and the new CMK enabled so that AWS KMS can decrypt the WorkSpaces that the original CMK encrypted\. If you don't want to keep the original CMK enabled, you must recreate your WorkSpaces and encrypt them using the new CMK\.

You must meet the following requirements to use an AWS KMS CMK to encrypt your WorkSpaces:
+ **The CMK must be symmetric\.** Amazon WorkSpaces does not support asymmetric CMKs\. For information about distinguishing between symmetric and asymmetric CMKs, see [ Identifying Symmetric and Asymmetric CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/find-symm-asymm.html) in the *AWS Key Management Service Developer Guide*\.
+ **The CMK must be enabled\.** To determine whether a CMK is enabled, see [ Displaying CMK Details](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys-console.html#viewing-console-details) in the *AWS Key Management Service Developer Guide*\.
+ **You must have the correct permissions and policies associated with the CMK\.** For more information, see [Part 2: Giving WorkSpaces Administrators Additional Permissions with an IAM Policy](#kms-permissions-iam-policy)\.

## Limits<a name="encryption_limits"></a>
+ You can't encrypt an existing WorkSpace\. You must encrypt a WorkSpace when you launch it\.
+ Creating a custom image from an encrypted WorkSpace is not supported\.
+ Disabling encryption for an encrypted WorkSpace is not currently supported\.
+ WorkSpaces launched with root volume encryption enabled might take up to an hour to provision\.
+ To reboot or rebuild an encrypted WorkSpace, first make sure that the AWS KMS CMK is enabled; otherwise, the WorkSpace becomes unusable\. To determine whether a CMK is enabled, see [ Displaying CMK Details](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys-console.html#viewing-console-details) in the *AWS Key Management Service Developer Guide*\.

## Overview of Amazon WorkSpaces Encryption Using AWS KMS<a name="kms-workspaces-overview"></a>

When you create WorkSpaces with encrypted volumes, Amazon WorkSpaces uses Amazon Elastic Block Store \(Amazon EBS\) to create and manage those volumes\. Amazon EBS encrypts your volumes with a data key using the industry\-standard AES\-256 algorithm\. Both Amazon EBS and Amazon WorkSpaces use your CMK to work with the encrypted volumes\. For more information about EBS volume encryption, see [Amazon EBS Encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) in the *Amazon EC2 User Guide for Windows Instances*\.

When you launch WorkSpaces with encrypted volumes, the end\-to\-end process works like this:

1. You specify the CMK to use for encryption as well as the user and directory for the WorkSpace\. This action creates a [grant](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) that allows Amazon WorkSpaces to use your CMK only for this WorkSpace—that is, only for the WorkSpace associated with the specified user and directory\.

1. Amazon WorkSpaces creates an encrypted EBS volume for the WorkSpace and specifies the CMK to use as well as the volume's user and directory\. This action creates a grant that allows Amazon EBS to use your CMK only for this WorkSpace and volume—that is, only for the WorkSpace associated with the specified user and directory, and only for the specified volume\.

1. <a name="WSP-EBS-requests-encrypted-volume-data-key"></a>Amazon EBS requests a volume data key that is encrypted under your CMK and specifies the WorkSpace user's Active Directory security identifier \(SID\) and AWS Directory Service directory ID as well as the Amazon EBS volume ID as the [encryption context](#kms-workspaces-encryption-context)\.

1. <a name="WSP-KMS-creates-data-key"></a>AWS KMS creates a new data key, encrypts it under your CMK, and then sends the encrypted data key to Amazon EBS\.

1. <a name="WSP-uses-EBS-to-attach-encrypted-volume"></a>Amazon WorkSpaces uses Amazon EBS to attach the encrypted volume to your WorkSpace\. Amazon EBS sends the encrypted data key to AWS KMS with a [ `Decrypt`](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) request and specifies the WorkSpace user's SID, the directory ID, and the volume ID, which is used as the encryption context\.

1. AWS KMS uses your CMK to decrypt the data key, and then sends the plaintext data key to Amazon EBS\.

1. Amazon EBS uses the plaintext data key to encrypt all data going to and from the encrypted volume\. Amazon EBS keeps the plaintext data key in memory for as long as the volume is attached to the WorkSpace\.

1. Amazon EBS stores the encrypted data key \(received at [Step 4](#WSP-KMS-creates-data-key)\) with the volume metadata for future use in case you reboot or rebuild the WorkSpace\.

1. When you use the AWS Management Console to remove a WorkSpace \(or use the [https://docs.aws.amazon.com/workspaces/latest/devguide/API_TerminateWorkspaces.html](https://docs.aws.amazon.com/workspaces/latest/devguide/API_TerminateWorkspaces.html) action in the Amazon WorkSpaces API\), Amazon WorkSpaces and Amazon EBS retire the grants that allowed them to use your CMK for that WorkSpace\.

## Amazon WorkSpaces Encryption Context<a name="kms-workspaces-encryption-context"></a>

Amazon WorkSpaces doesn't use your CMK directly for cryptographic operations \(such as [https://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html](https://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html), [https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html), [https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html](https://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html), etc\.\), which means Amazon WorkSpaces doesn't send requests to AWS KMS that include an [ encryption context](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#encrypt_context)\. However, when Amazon EBS requests an encrypted data key for the encrypted volumes of your WorkSpaces \([Step 3](#WSP-EBS-requests-encrypted-volume-data-key) in the [Overview of Amazon WorkSpaces Encryption Using AWS KMS](#kms-workspaces-overview)\) and when it requests a plaintext copy of that data key \([Step 5](#WSP-uses-EBS-to-attach-encrypted-volume)\), it includes encryption context in the request\.

 The encryption context provides [additional authenticated data](https://docs.aws.amazon.com/crypto/latest/userguide/cryptography-concepts.html#term-aad) \(AAD\) that AWS KMS uses to ensure data integrity\. The encryption context is also written to your AWS CloudTrail log files, which can help you understand why a given CMK was used\. Amazon EBS uses the following for the encryption context:
+ The security identifier \(SID\) of the Active Directory user that is associated with the WorkSpace
+ The directory ID of the AWS Directory Service directory that is associated with the WorkSpace
+ The Amazon EBS volume ID of the encrypted volume

The following example shows a JSON representation of the encryption context that Amazon EBS uses:

```
{
  "aws:workspaces:sid-directoryid": "[S-1-5-21-277731876-1789304096-451871588-1107]@[d-1234abcd01]",
  "aws:ebs:id": "vol-1234abcd"
}
```

## Giving Amazon WorkSpaces Permission to Use a CMK on Your Behalf<a name="kms-workspaces-permissions"></a>

You can protect your WorkSpace data under the AWS managed CMK for Amazon WorkSpaces \(**aws/workspaces**\) or a customer managed CMK\. If you use a customer managed CMK, you need to give Amazon WorkSpaces permission to use the CMK on behalf of the Amazon WorkSpaces administrators in your account\. The AWS managed CMK for Amazon WorkSpaces has the required permissions by default\.

To prepare your customer managed CMK for use with Amazon WorkSpaces, use the following procedure\.

1. [Add your WorkSpaces administrators to the list of key users in the CMK's key policy](#kms-permissions-key-users)

1. [Give your WorkSpaces administrators additional permissions with an IAM policy](#kms-permissions-iam-policy)

Your WorkSpaces administrators also need permission to use Amazon WorkSpaces\. For more information about these permissions, go to [Identity and Access Management for Amazon WorkSpaces](workspaces-access-control.md)\.

### Part 1: Adding WorkSpaces Administrators to a CMK's Key Users<a name="kms-permissions-key-users"></a>

To give Amazon WorkSpaces administrators the permissions that they require, you can use the AWS Management Console or the AWS KMS API\.

#### To add WorkSpaces administrators as key users for a CMK \(console\)<a name="kms-permissions-users-consoleAPI"></a>

1. Sign in to the AWS Management Console and open the AWS Key Management Service \(AWS KMS\) console at [https://console\.aws\.amazon\.com/kms](https://console.aws.amazon.com/kms)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Customer managed keys**\.

1. Choose the key ID or alias of your preferred customer managed CMK\.

1. Choose the **Key policy** tab\. Under **Key users**, choose **Add**\.

1. In the list of IAM users and roles, select the users and roles that correspond to your WorkSpaces administrators, and then choose **Add**\.

#### To add WorkSpaces administrators as key users for a CMK \(API\)<a name="kms-permissions-users-api"></a>

1. Use the [GetKeyPolicy](https://docs.aws.amazon.com/kms/latest/APIReference/API_GetKeyPolicy.html) operation to get the existing key policy, and then save the policy document to a file\.

1. Open the policy document in your preferred text editor\. Add the IAM users and roles that correspond to your WorkSpaces administrators to the policy statements that [ give permission to key users](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-users)\. Then save the file\.

1. Use the [PutKeyPolicy](https://docs.aws.amazon.com/kms/latest/APIReference/API_PutKeyPolicy.html) operation to apply the key policy to the CMK\.

### Part 2: Giving WorkSpaces Administrators Additional Permissions with an IAM Policy<a name="kms-permissions-iam-policy"></a>

If you select a customer managed CMK to use for encryption, you must establish IAM policies that allow Amazon WorkSpaces to use the CMK on behalf of an IAM user in your account who launches encrypted WorkSpaces\. That user also needs permission to use Amazon WorkSpaces\. For more information about creating and editing IAM user policies, see [ Managing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html) in the *IAM User Guide* and [Identity and Access Management for Amazon WorkSpaces](workspaces-access-control.md)\.

Amazon WorkSpaces encryption requires limited access to the CMK\. The following is a sample key policy that you can use\. This policy separates the principals who can manage the AWS KMS CMK from those who can use it\. Before you use this sample key policy, replace the example account ID and IAM user name with actual values from your account\.

The first statement matches the default AWS KMS key policy\. It gives your account permission to use IAM policies to control access to the CMK\. The second and third statements define which AWS principals can manage and use the key, respectively\. The fourth statement enables AWS services that are integrated with AWS KMS to use the key on behalf of the specified principal\. This statement enables AWS services to create and manage grants\. The statement uses a condition element that limits grants on the CMK to those made by AWS services on behalf of users in your account\.

**Note**  
If your WorkSpaces administrators use the AWS Management Console to create WorkSpaces with encrypted volumes, the administrators need permission to list aliases and list keys \(the `"kms:ListAliases"` and `"kms:ListKeys"` permissions\)\. If your WorkSpaces administrators use only the Amazon WorkSpaces API \(not the console\), you can omit the `"kms:ListAliases"` and `"kms:ListKeys"` permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::123456789012:root"},
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::123456789012:user/Alice"},
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*"
       ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::123456789012:user/Alice"},
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::123456789012:user/Alice"},
      "Action": [
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {"Bool": {"kms:GrantIsForAWSResource": "true"}}
    }
  ]
}
```

The IAM policy for a user or role that is encrypting a WorkSpace must include usage permissions on the customer managed CMK, as well as access to WorkSpaces\. To give an IAM user or role WorkSpaces permissions, you can attach the following sample policy to the IAM user or role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ds:*",
                "ds:DescribeDirectories",
                "workspaces:*",
                "workspaces:DescribeWorkspaceBundles",
                "workspaces:CreateWorkspaces",
                "workspaces:DescribeWorkspaceBundles",
                "workspaces:DescribeWorkspaceDirectories",
                "workspaces:DescribeWorkspaces",
                "workspaces:RebootWorkspaces",
                "workspaces:RebuildWorkspaces"
            ],
            "Resource": "*"
        }
    ]
}
```

The following IAM policy is required by the user for using AWS KMS\. It gives the user read\-only access to the CMK along with the ability to create grants\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:CreateGrant",
                "kms:Describe*",
                "kms:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

If you want to specify the CMK in your policy, use an IAM policy similar to the following\. Replace the example CMK ARN with a valid one\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "kms:CreateGrant",
      "Resource": "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"
    },
    {
      "Effect": "Allow",
      "Action": [
        "kms:ListAliases",
        "kms:ListKeys"
      ],
      "Resource": "*"
    }
  ]
}
```

## Encrypting WorkSpaces<a name="encrypt_workspace"></a>

**To encrypt a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. Choose **Launch WorkSpaces** and complete the first three steps\.

1. For the **WorkSpaces Configuration** step, do the following:

   1. Select the volumes to encrypt: **Root Volume**, **User Volume**, or both volumes\.

   1. For **Encryption Key**, select an AWS KMS CMK, either the AWS managed CMK created by Amazon WorkSpaces or a CMK that you created\. The CMK that you select must be symmetric\. Amazon WorkSpaces does not support asymmetric CMKs\.

   1. Choose **Next Step**\.

1. Choose **Launch WorkSpaces**\.

## Viewing Encrypted WorkSpaces<a name="maintain_encryption"></a>

To see which WorkSpaces and volumes have been encrypted from the Amazon WorkSpaces console, choose **WorkSpaces** from the navigation bar on the left\. The **Volume Encryption** column shows whether each WorkSpace has encryption enabled or disabled\. To see which specific volumes have been encrypted, expand the WorkSpace entry to see the **Encrypted Volumes** field\.