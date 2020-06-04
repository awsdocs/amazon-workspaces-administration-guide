# Encrypted WorkSpaces<a name="encrypt-workspaces"></a>

Amazon WorkSpaces is integrated with the AWS Key Management Service \(AWS KMS\)\. This enables you to encrypt storage volumes of WorkSpaces using customer master keys \(CMKs\)\. When you launch a WorkSpace, you can encrypt the root volume \(for Microsoft Windows, the C drive; for Linux, /\) and the user volume \(for Windows, the D drive; for Linux, /home\)\. Doing so ensures that the data stored at rest, disk I/O to the volume, and snapshots created from the volumes are all encrypted\.

## Prerequisites<a name="encryption_prerequisites"></a>

You need an AWS KMS CMK before you can begin the encryption process\.

The first time that you launch an unencrypted WorkSpace from the Amazon WorkSpaces console in a Region, Amazon WorkSpaces automatically creates an AWS managed CMK \(**aws/workspaces**\) in your account\. You can select this AWS managed CMK to encrypt the user and root volumes of your WorkSpace\. For details, see [How Amazon WorkSpaces uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-workspaces.html) in the *AWS Key Management Service Developer Guide*\.

You can view this AWS managed CMK, including its policies and grants, and can track its use in AWS CloudTrail logs, but you cannot use or manage this CMK\. Amazon WorkSpaces creates and manages this CMK\. Only Amazon WorkSpaces can use this CMK, and it can use it only to encrypt WorkSpaces resources in your account\. AWS managed CMKs, including the one that Amazon WorkSpaces supports, are rotated every three years\. For details, see [Rotating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html) in the *AWS Key Management Service Developer Guide*\.

Alternatively, you can select a symmetric customer managed CMK that you created using AWS KMS\. You can view, use, and manage this CMK, including setting its policies\. For more information about creating CMKs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. For more information about creating CMKs using the AWS KMS API, see [Working with Keys](https://docs.aws.amazon.com/kms/latest/developerguide/programming-keys.html) in the *AWS Key Management Service Developer Guide*\.

You must meet the following requirements to use an AWS KMS CMK to encrypt your WorkSpaces:
+ The CMK must be symmetric\. Amazon WorkSpaces does not support asymmetric CMKs\. For information about distinguishing between symmetric and asymmetric CMKs, see [ Identifying Symmetric and Asymmetric CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/find-symm-asymm.html) in the *AWS Key Management Service Developer Guide*\.
+ The CMK must be enabled\. To determine whether a CMK is enabled, see [ Displaying CMK Details](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys-console.html#viewing-console-details) in the *AWS Key Management Service Developer Guide*\.
+ You must have the correct permissions and policies associated with the key\. For more information, see [IAM Permissions and Roles for Encryption](#IAM_permissions)\.

**Important**  
There is a limit of 500 WorkSpaces per CMK\. This limit is due to the Grants per grantee principal quota in AWS KMS\. For more information about this quota, see [ Grants per grantee principal](https://docs.aws.amazon.com/kms/latest/developerguide/resource-limits.html#grants-per-principal-per-key) in the *AWS Key Management Service Developer Guide*\.  
When you are encrypting WorkSpaces, create a CMK for every 500 WorkSpaces\. For example, if you are encrypting 850 WorkSpaces, create two CMKs\.  
If you are trying to launch encrypted WorkSpaces and you receive the error message "The specified key is not available\. Please provide a valid key for encryption," the Grants per grantee principal quota for the existing CMK has been reached\.

## Limits<a name="encryption_limits"></a>
+ You can't encrypt an existing WorkSpace\. You must encrypt a WorkSpace when you launch it\.
+ Creating a custom image from an encrypted WorkSpace is not supported\.
+ Disabling encryption for an encrypted WorkSpace is not currently supported\.
+ WorkSpaces launched with root volume encryption enabled might take up to an hour to provision\.
+ To reboot or rebuild an encrypted WorkSpace, first make sure that the AWS KMS CMK is enabled; otherwise, the WorkSpace becomes unusable\. To determine whether a CMK is enabled, see [ Displaying CMK Details](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys-console.html#viewing-console-details) in the *AWS Key Management Service Developer Guide*\.

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

## IAM Permissions and Roles for Encryption<a name="IAM_permissions"></a>

If you select a customer managed CMK to use for encryption, you must establish policies that allow Amazon WorkSpaces to use the CMK on behalf of an IAM user in your account who launches encrypted WorkSpaces\. That user also needs permission to use Amazon WorkSpaces\.

Amazon WorkSpaces encryption requires limited access to the CMK\. The following is a sample key policy that you can use\. This policy separates the principals who can manage the AWS KMS CMK from those who can use it\. Before you use this sample key policy, replace the example account ID and IAM user name with actual values from your account\.

The first statement matches the default AWS KMS key policy\. It gives your account permission to use IAM policies to control access to the CMK\. The second and third statements define which AWS principals can manage and use the key, respectively\. The fourth statement enables AWS services that are integrated with AWS KMS to use the key on behalf of the specified principal\. This statement enables AWS services to create and manage grants\. The statement uses a condition element that limits grants on the CMK to those made by AWS services on behalf of users in your account\.

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