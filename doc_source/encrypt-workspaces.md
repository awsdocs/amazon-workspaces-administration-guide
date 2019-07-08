# Encrypted WorkSpaces<a name="encrypt-workspaces"></a>

Amazon WorkSpaces is integrated with the AWS Key Management Service \(AWS KMS\)\. This enables you to encrypt storage volumes of WorkSpaces using customer master keys \(CMK\)\. When you launch a WorkSpace, you can encrypt the root volume \(for Microsoft Windows, the C: drive, for Linux, /\) and the user volume \(for Windows, the D: drive; for Linux, /home\)\. Doing so ensures that the data stored at rest, disk I/O to the volume, and snapshots created from the volumes are all encrypted\.

## Prerequisites<a name="encryption_prerequisites"></a>

You need an AWS KMS CMK before you can begin the encryption process\.

The first time you launch a WorkSpace from the Amazon WorkSpaces console in a region, a default CMK is created for you automatically\. You can select this key to encrypt the user and root volumes of your WorkSpace\.

Alternately, you can select a CMK that you created using AWS KMS\. For more information about creating keys, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. For more information about creating keys using the AWS KMS API, see [Working With Keys](https://docs.aws.amazon.com/kms/latest/developerguide/programming-keys.html) in the *AWS Key Management Service Developer Guide*\.

You must meet the following requirements to use an AWS KMS CMK to encrypt your WorkSpaces:
+ The key must be enabled\.
+ You must have the correct permissions and policies associated with the key\. For more information, see [IAM Permissions and Roles for Encryption](#IAM_permissions)\.

## Limits<a name="encryption_limits"></a>
+ Creating a custom image from an encrypted WorkSpace is not supported\.
+ Disabling encryption for an encrypted WorkSpace is not currently supported\.
+ WorkSpaces launched with root volume encryption enabled might take up to an hour to provision\.
+ To reboot or rebuild an encrypted WorkSpace, first make sure that the AWS KMS CMK is enabled; otherwise, the WorkSpace becomes unusable\.

## Encrypting WorkSpaces<a name="encrypt_workspace"></a>

**To encrypt a WorkSpace**

1. Open the Amazon WorkSpaces console at [https://console\.aws\.amazon\.com/workspaces/](https://console.aws.amazon.com/workspaces/)\.

1. Choose **Launch WorkSpaces** and complete the first three steps\.

1. For the **WorkSpaces Configuration** step, do the following:

   1. Select the volumes to encrypt: **Root Volume**, **User Volume**, or both volumes\.

   1. For **Encryption Key**, choose your AWS KMS CMK\.

   1. Choose **Next Step**\.

1. Choose **Launch WorkSpaces**\.

## Viewing Encrypted WorkSpaces<a name="maintain_encryption"></a>

To see which WorkSpaces and volumes have been encrypted from the Amazon WorkSpaces console, choose **WorkSpaces** from the navigation bar on the left\. The **Volume Encryption** column shows whether each WorkSpace has encryption enabled or disabled\. To see which specific volumes have been encrypted, expand the WorkSpace entry to see the **Encrypted Volumes** field\.

## IAM Permissions and Roles for Encryption<a name="IAM_permissions"></a>

Amazon WorkSpaces encryption privileges require limited AWS KMS access on a given key for the IAM user who launches encrypted WorkSpaces\. The following is a sample key policy that can be used\. This policy enables you to separate the principals that can manage the AWS KMS CMK from those that can use it\. The account ID and IAM user name must be modified to match your account\.

The first statement matches the default AWS KMS key policy\. The second and third statements define which AWS principals can manage and use the key, respectively\. The fourth statement enables AWS services that are integrated with AWS KMS to use the key on behalf of the specified principal\. This statement enables AWS services to create and manage grants\. The condition uses a context key that is set only for AWS KMS calls made by AWS services on behalf of the customers\.

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

The IAM policy for a user or role that is encrypting a WorkSpace should include usage permissions on the CMK, as well as access to WorkSpaces\. The following is a sample policy that can be attached to an IAM user to grant them WorkSpaces privileges\.

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
                "wam:CreateWorkspaces",
                "wam:DescribeWorkspaceBundles",
                "wam:DescribeWorkspaceDirectories",
                "wam:DescribeWorkspaces",
                "wam:RebootWorkspaces",
                "wam:RebuildWorkspaces"
            ],
            "Resource": "*"
        }
    ]
}
```

The following is the IAM policy required by the user for using AWS KMS\.

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