# Control Access to Amazon WorkSpaces Resources<a name="workspaces-access-control"></a>

By default, IAM users don't have permissions for Amazon WorkSpaces resources and operations\. To allow IAM users to manage Amazon WorkSpaces resources, you must create an IAM policy that explicitly grants them permissions, and attach the policy to the IAM users or groups that require those permissions\. For more information about IAM policies, see [Permissions and Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide* guide\.

Amazon WorkSpaces also creates an IAM role to allow the Amazon WorkSpaces service access to required resources\.

For more information about IAM, see [Identity and Access Management \(IAM\)](http://aws.amazon.com/iam) and the [IAM User Guide](http://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

**Example 1: Perform all Amazon WorkSpaces tasks**  
The following policy statement grants an IAM user permission to perform all Amazon WorkSpaces tasks, including creating and managing directories, as well as running the quick setup procedure\.  
Note that although Amazon WorkSpaces fully supports the `Action` and `Resource` elements when using the API and command\-line tools, you must set them both to "\*" in order to use the Amazon WorkSpaces console successfully\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "workspaces:*",
        "ds:*",
        "iam:PassRole", 
        "iam:GetRole", 
        "iam:CreateRole", 
        "iam:PutRolePolicy", 
        "kms:ListAliases",
        "kms:ListKeys",
        "ec2:CreateVpc", 
        "ec2:CreateSubnet", 
        "ec2:CreateNetworkInterface", 
        "ec2:CreateInternetGateway", 
        "ec2:CreateRouteTable", 
        "ec2:CreateRoute", 
        "ec2:CreateTags", 
        "ec2:CreateSecurityGroup", 
        "ec2:DescribeInternetGateways", 
        "ec2:DescribeRouteTables", 
        "ec2:DescribeVpcs", 
        "ec2:DescribeSubnets", 
        "ec2:DescribeNetworkInterfaces", 
        "ec2:DescribeAvailabilityZones", 
        "ec2:AttachInternetGateway", 
        "ec2:AssociateRouteTable", 
        "ec2:AuthorizeSecurityGroupEgress", 
        "ec2:AuthorizeSecurityGroupIngress", 
        "ec2:DeleteSecurityGroup", 
        "ec2:DeleteNetworkInterface", 
        "ec2:RevokeSecurityGroupEgress", 
        "ec2:RevokeSecurityGroupIngress",
        "workdocs:RegisterDirectory",
        "workdocs:DeregisterDirectory",
        "workdocs:AddUserToGroup",
        "workdocs:RemoveUserFromGroup"
      ],
      "Resource": "*"
    }
  ]
}
```

**Example 2: Perform WorkSpace\-specific tasks**  
The following policy statement grants an IAM user permission to perform WorkSpace\-specific tasks, such as launching and removing WorkSpaces\. The user can perform some tasks on the WorkSpaces directory, such as enabling or disabling the settings for Internet access, local administrator access, and maintenance mode\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "workspaces:*",
        "ds:*"
      ],
      "Resource": "*"
    }
  ]
}
```
To also grant the user the ability to enable Amazon WorkDocs for users within Amazon WorkSpaces, add the `workdocs` operations shown here:  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "workspaces:*",
        "ds:*",
        "workdocs:AddUserToGroup",
        "workdocs:RemoveUserFromGroup"
      ],
      "Resource": "*"
    }
  ]
}
```
To also grant the user the ability to use the Launch WorkSpaces wizard, add the `kms` operations shown here:  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "workspaces:*",
        "ds:*",
        "workdocs:AddUserToGroup",
        "workdocs:RemoveUserFromGroup",
        "kms:ListAliases",
        "kms:ListKeys"
      ],
      "Resource": "*"
    }
  ]
}
```
To grant the user the ability to perform WorkSpaces\-specific tasks except for certificate tasks, add a statement to deny the certificate operations:  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "workspaces:*",
        "ds:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "Action": [
        "workspaces:DeleteCertificate",
        "workspaces:ImportCertificate",
        "workspaces:DescribeCertificates"
      ],
      "Resource": "*"
    }
  ]
}
```

## Specifying Amazon WorkSpaces Resources in an IAM Policy<a name="wsp_iam_resource"></a>

To specify an Amazon WorkSpaces resource in the `Resource` element of the policy statement, you need to use the Amazon Resource Name \(ARN\) of the resource\. You control access to your Amazon WorkSpaces resources by either allowing or denying permissions to use the API actions specified in the `Action` element of your IAM policy statement\. Amazon WorkSpaces defines ARNs for WorkSpaces and bundles\.

### WorkSpace ARN<a name="wsp_arn_syntax"></a>

A WorkSpace ARN has the following syntax:

```
arn:aws:workspaces:region:account_id:workspace/workspace_identifier
```

*region*  
The region that the WorkSpace is in \(for example, `us-east-2`\)\.

*account\_id*  
The ID of the AWS account, with no hyphens \(for example, `123456789012`\)\.

*workspace\_identifier*  
The ID of the WorkSpace \(for example, `ws-0123456789`\)\.

The following is the format of the `Resource` element of a policy statement that identifies a specific WorkSpace:

```
"Resource": "arn:aws:workspaces:region:account_id:workspace/workspace_identifier"
```

You can use the \* wildcard to specify all WorkSpaces that belong to a specific account in a specific region\.

### Bundle ARN<a name="bundle_arn_syntax"></a>

A bundle ARN has the following syntax:

```
arn:aws:workspaces:region:account_id:workspacebundle/bundle_identifier
```

*region*  
The region that the WorkSpace is in \(for example, `us-east-2`\)\.

*account\_id*  
The ID of the AWS account, with no hyphens \(for example, `123456789012`\)\.

*bundle\_identifier*  
The ID of the WorkSpace bundle \(for example, `wsb-0123456789`\)\.

The following is the format of the `Resource` element of a policy statement that identifies a specific bundle:

```
"Resource": "arn:aws:workspaces:region:account_id:workspacebundle/bundle_identifier"
```

You can use the \* wildcard to specify all bundles that belong to a specific account in a specific region\.