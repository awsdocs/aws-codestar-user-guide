# AWS CodeStar Access Permissions Reference<a name="access-permissions"></a>

 You can use AWS CodeStar as an IAM user, a federated user, the root user, or an assumed role\. All user types with the appropriate permissions can manage project permissions to their AWS resources, but AWS CodeStar manages project permissions automatically for IAM users\. IAM policies and roles grant permissions and access to that user based on the project role\. You can use the IAM console to create other policies that assign AWS CodeStar and other permissions to an IAM user\. 

For example, you might want to allow a user to view but not change an AWS CodeStar project\. In this case, you add the IAM user to an AWS CodeStar project with the Viewer role\. Every AWS CodeStar project has a set of policies that help you control access to the project\. In addition, you can control which users have access to AWS CodeStar\.

In the [Setting Up AWS CodeStar](setting-up.md) instructions, you attached a policy named AWSCodeStarFullAccess to your IAM user\. This policy allows full access to AWS CodeStar\. That policy statement looks similar to this:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CodeStarEC2",
      "Effect": "Allow",
      "Action": [
        "codestar:*",
        "ec2:DescribeKeyPairs",
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodeStarCF",
      "Effect": "Allow",
      "Action": [
        "cloudformation:DescribeStack*",
        "cloudformation:GetTemplateSummary"
      ],
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/awscodestar-*"
      ]
    }
  ]
}
```

 This policy statement allows the user to perform all available actions in AWS CodeStar with all available AWS CodeStar resources associated with the AWS account\. This includes creating and deleting projects\. You might not want to give all users this much access\. Instead, you can add project\-level permissions using project roles managed by AWS CodeStar\. The roles grant specific levels of access to AWS CodeStar projects and are named as follows: 
+ Owner
+ Contributor
+ Viewer

AWS CodeStar access is handled differently for IAM users and federated users\. Only IAM users can be added to teams\. To grant IAM users permissions to projects, you add the user to the project role by adding them to the team\. To grant federated users permissions to projects, you manually attach the AWS CodeStar project role's managed policy to the federated user's role\. This table summarizes the tools available for each type of access\.


****  

| Permissions feature | IAM user | Federated user | Root user | 
| --- | --- | --- | --- | 
| SSH key management for remote access for EC2 and Elastic Beanstalk projects | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 
| AWS CodeCommit SSH access | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 
|  IAM user permissions managed by AWS CodeStar  | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 
|  Project permissions managed manually  |  | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) | 
| Users can be added to project as team members | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 

**Topics**
+ [AWS CodeStar Project\-Level Policies and Permissions](#access-permissions-proj)
+ [IAM User Access to AWS CodeStar](#access-permissions-user)
+ [Federated User Access to AWS CodeStar](#access-permissions-federated)
+ [AWS CodeStar Service Role Policy and Permissions](#access-permissions-service-role)
+ [Action and Resource Syntax](#access-permissions-syntax)

## AWS CodeStar Project\-Level Policies and Permissions<a name="access-permissions-proj"></a>

There are three roles in AWS CodeStar projects: Owner, Contributor, and Viewer\. Each role is specific to a project and defined by an IAM managed policy, where *project\-id* is the ID of the AWS CodeStar project \(for example, *my\-first\-projec*\): 
+ CodeStar\_*project\-id*\_Owner
+ CodeStar\_*project\-id*\_Contributor
+ CodeStar\_*project\-id*\_Viewer

**Important**  
These policies are subject to change by AWS CodeStar\. They should not be modified manually\. If you want to add or change permissions, attach additional policies to the IAM user\.

When you add a user to a project and choose a role for the user, the appropriate policy is applied automatically to the IAM user\. Under most circumstances, you don't need to directly attach or manage policies or permissions in IAM\. Manually attaching an AWS CodeStar role policy to an IAM user is not recommended\. If absolutely necessary, as a supplement to an AWS CodeStar role policy, you can create your own managed policies to apply your own level of permissions to an IAM user\.

**Note**  
The policies for roles in an AWS CodeStar project apply to that project only\. This helps ensure that users can only see and interact with the AWS CodeStar projects they have permissions to, at the level determined by their role\. Only users who will create AWS CodeStar projects should have a policy applied that allows access to all AWS CodeStar resources, regardless of project\.

All AWS CodeStar role policies vary, depending on the AWS resources associated with the project with which the roles are associated\. Unlike other AWS services, these policies are customized when the project is created and updated as project resources change\. Therefore, there is no one canonical Owner, Contributor, or Viewer managed policy\. 

### AWS CodeStar Owner Role Policy<a name="access-permissions-proj-owner"></a>

The CodeStar\_*project\-id*\_Owner managed policy allows a user to perform all actions in the AWS CodeStar project with no restrictions\. This is the only policy that allows a user to add or remove team members\. Although the contents of the policy vary, depending on the resources associated with the project, the CodeStar\_*project\-id*\_Owner managed policy contains the following AWS CodeStar permissions\. As an AWS managed policy, it is subject to change without notice\.

```
...
{
  "Effect": "Allow",
  "Action": [
    ...
    "codestar:*",
    ...
  ],
  "Resource": [
    "arn:aws:codestar:us-east-2:111111111111:project/project-id",
    "arn:aws:iam::account-id:policy/CodeStar_project-id_Owner"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:DescribeUserProfile",
    "codestar:ListProjects",
    "codestar:ListUserProfiles",
    "codestar:VerifyServiceRole",
    ...
  ],
  "Resource": [
    "*"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:*UserProfile",
    ...
  ],
  "Resource": [
    "arn:aws:iam::account-id:user/user-name"
  ]
}
...
```

An IAM user with this policy can perform all AWS CodeStar actions in the project, but unlike an IAM user with the **AWSCodeStarFullAccess** policy, the user cannot create new projects\. The `codestar:*` permission is limited in scope to a specific resource \(the AWS CodeStar project associated with that project ID\)\.

### AWS CodeStar Contributor Role Policy<a name="access-permissions-proj-contributor"></a>

The CodeStar\_*project\-id*\_Contributor managed policy allows a user to contribute to the project and change the project dashboard, but does not allow a user to add or remove team members\. Although the contents of the policy vary, depending on the resources associated with the project, the CodeStar\_*project\-id*\_Contributor policy contains the following AWS CodeStar permissions\. As an AWS managed policy, it is subject to change without notice\. 

```
...
{
  "Effect": "Allow",
  "Action": [
    ...
    "codestar:Describe*",
    "codestar:Get*",
    "codestar:List*",
    "codestar:PutExtendedAccess",
    ...
  ],
  "Resource": [
    "arn:aws:codestar:us-east-2:111111111111:project/project-id",
    "arn:aws:iam::account-id:policy/CodeStar_project-id_Contributor"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:DescribeUserProfile",
    "codestar:ListProjects",
    "codestar:ListUserProfiles",
    "codestar:VerifyServiceRole",
    ...
  ],
  "Resource": [
    "*"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:*UserProfile",
    ...
  ],
  "Resource": [
    "arn:aws:iam::account-id:user/user-name"
  ]
}
...
```

### AWS CodeStar Viewer Role Policy<a name="access-permissions-proj-viewer"></a>

The CodeStar\_*project\-id*\_Viewer managed policy allows a user to view a project in AWS CodeStar, but not change its resources or add or remove team members\. Although the contents of the policy vary, depending on the resources associated with the project, the CodeStar\_*project\-id*\_Viewer policy contains the following AWS CodeStar permissions\. As an AWS managed policy, it is subject to change without notice\. 

```
...
{
  "Effect": "Allow",
  "Action": [
    ...
    "codestar:Describe*",
    "codestar:Get*",
    "codestar:List*",
    ...
  ],
  "Resource": [
    "arn:aws:codestar:us-east-2:111111111111:project/project-id",
    "arn:aws:iam::account-id:policy/CodeStar_project-id_Viewer"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:DescribeUserProfile",
    "codestar:ListProjects",
    "codestar:ListUserProfiles",
    "codestar:VerifyServiceRole",
    ...
  ],
  "Resource": [
    "*"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:*UserProfile",
    ...
  ],
  "Resource": [
    "arn:aws:iam::account-id:user/user-name"
  ]
}
...
```

## IAM User Access to AWS CodeStar<a name="access-permissions-user"></a>

When you add an IAM user to a project and choose a role for the user, AWS CodeStar applies the appropriate policy to the IAM user automatically\. For IAM users, you don't need to directly attach or manage policies or permissions in IAM\. 

### Add an IAM User to Your AWS CodeStar Project<a name="access-permissions-user-add-CodeStar"></a>

After you create your AWS CodeStar project, grant IAM users access to your project\. As an IAM user with the Owner role in an AWS CodeStar project or the **AWSCodeStarFullAccess** policy applied to your IAM user, you can add other IAM users to the project team\. When you select the project\-level role for the team member, AWS CodeStar attaches the appropriate managed policy to the IAM user based on the role you choose:
+ Owner
+ Contributor
+ Viewer

### Remove an IAM User from Your AWS CodeStar Project<a name="access-permissions-user-remove-CodeStar"></a>

When you delete a project, AWS CodeStar removes the policies that were applied automatically to IAM users that you added as team members\. For IAM users, you don't need to directly detach or manage policies or permissions in IAM\.

### Attach an Inline Policy to an IAM User<a name="access-permissions-user"></a>

When you add a user to a project, AWS CodeStar automatically attaches the managed policy for the project that matches the user's role\. You should not manually attach an AWS CodeStar managed policy for a project to an IAM user\. With the exception of AWSCodeStarFullAccess, we do not recommend that you attach policies that change an IAM user's permissions in an AWS CodeStar project\. If you decide to create and attach your own policies, do the following:

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Users**, and then choose the user to which you want to attach additional policies\.

1. On the **Permissions** tab, choose **Add permissions**\. Choose **Attach existing policies directly**, select the policy you want to apply, and then choose **Attach Policy**\.

   For example, if you want to add your own customized policy to a user, choose the policy name from the list of policies\.

1. If you do not want to attach an existing policy but instead want to create your own custom policy, on the **Permissions** tab, choose **Add inline policy**\. Choose **Custom Policy**, and then choose **Select**\.

   In **Policy Name**, type a name for this policy\. In the **Policy Document** box, type a policy that follows this format, and then choose **Apply Policy**:

   ```
   {
     "Version": "2012-10-17",
     "Statement" : [
       {
         "Effect" : "Allow",
         "Action" : [
           "action-statement"
         ],
         "Resource" : [
           "resource-statement"
         ]
       },
       {
         "Effect" : "Allow",
         "Action" : [
           "action-statement"
         ],
         "Resource" : [
           "resource-statement"
         ]
       }
     ]
   }
   ```

   In the preceding statement, for *action\-statement* and *resource\-statement*, specify the AWS CodeStar actions and resources the IAM user is allowed to perform or access\. \(By default, the IAM user does not have permissions unless a corresponding `Allow` statement is explicitly stated\. If you want to specifically deny a permission granted by another policy, such as the policy for an AWS CodeStar role, choose `Deny` instead of Allow\.\) You can add statements as needed\. The following sections describe the format of allowed actions and resources for AWS CodeStar\. Syntax examples are provided in these sections\.

## Federated User Access to AWS CodeStar<a name="access-permissions-federated"></a>

Instead of creating an IAM user or using the root user, you can use user identities from AWS Directory Service, your enterprise user directory, a web identity provider, or IAM users assuming roles\. These are known as *federated users*\.

Grant federated users access to your AWS CodeStar project by manually attaching the managed policies described in [AWS CodeStar Project\-Level Policies and Permissions](#access-permissions-proj) to the user's IAM role\. You wait to attach the Owner, Contributor, or Viewer policy until after AWS CodeStar creates your project resources and IAM roles\. 

**Prerequisites: **
+ You must have set up an identity provider\. For more information, see [Creating IAM Identity Providers](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create.html)\. For example, have a SAML identity provider set up and AWS authentication set up through the provider\. For more information about SAML federation, see [About SAML 2\.0\-based Federation](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)\.
+ You must have created a role for a federated user to assume when access is requested through an [identity provider](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html)\. An STS trust policy must be attached to the role that allows federated users to assume the role\. For more information, see [Federated Users and Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html#intro-access-roles) in the *IAM User Guide*\.
+ You must have created your AWS CodeStar project and know the project ID\.

For more information about creating a role for identity providers, see [Creating a Role for a Third\-Party Identity Provider \(Federation\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html)\.

### Attach the `AWSCodeStarFullAccess` Managed Policy to the Federated User's Role<a name="access-permissions-federated-attach-FullAccess"></a>

Grant a federated user permissions to create a project by attaching the `AWSCodeStarFullAccess` managed policy\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated AdministratorAccess managed policy or equivalent\.

**Note**  
After you create the project, your project Owner permissions are not applied automatically\. Using a role with administrative permissions for your account, attach the Owner managed policy as detailed in [Attach Your Project's AWS CodeStar Viewer/Contributor/Owner Managed Policy to the Federated User's Role](#access-permissions-federated-attach-CodeStar)\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter `AWSCodeStarFullAccess` in the search field\. The policy name is displayed, with a policy type of **AWS managed**\. You can expand the policy to see the permissions in the policy statement\.

1. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\. Choose **Attach**\.

1. On the **Attach Policy** page, filter for the federated user's role in the search field\. Select the box next to the name of the role and then choose **Attach policy**\. The **Attached entities** tab shows the new attachment\.

### Attach Your Project's AWS CodeStar Viewer/Contributor/Owner Managed Policy to the Federated User's Role<a name="access-permissions-federated-attach-CodeStar"></a>

Grant federated users access to your project by attaching the appropriate Owner, Contributor, or Viewer managed policy to the user's role\. The managed policy gives the appropriate level of permissions\. Unlike IAM users, you need to manually attach and detach managed policies for federated users\. This is equivalent to assigning project permissions to team members in AWS CodeStar\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated AdministratorAccess managed policy or equivalent\.

**Prerequisities:**
+ You must have created a role or have an existing role that your federated user assumes\.
+ You must know which level of permissions you want to grant\. The managed policies attached to the Owner, Contributor, and Viewer roles provide role\-based permissions for your project\.
+ Your AWS CodeStar project must have been created\. The managed policy is not available in IAM until the project is created\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter your project ID in the search field\. The policy name matching your project is displayed, with a policy type of **Customer managed**\. You can expand the policy to see the permissions in the policy statement\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-codestar-mgdpolicy-listing.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. Choose one of these managed policies\. This example attaches a Viewer managed policy\. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\. Choose **Attach**\.

1. On the **Attach Policy** page, filter for the federated user's role in the search field\. Select the box next to the name of the role and then choose **Attach policy**\. The **Attached entities** tab shows the new attachment\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-codestar-mgdpolicy-attached-entities.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

### Detach an AWS CodeStar Managed Policy from the Federated User's Role<a name="access-permissions-federated-detach-CodeStar"></a>

Before you delete your AWS CodeStar project, you must manually detach any managed policies you attached to a federated user's role\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated AdministratorAccess managed policy or equivalent\.

1. Open the IAM console\. In the navigation pane of the console, choose **Policies**\.

1. Enter your project ID in the search field\.

1. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\.

1. Filter for the federated user's role in the search field\. Choose **Detach**\.

### Attach an AWS Cloud9 Managed Policy to the Federated User's Role<a name="access-permissions-federated-attach-Cloud9"></a>

Grant federated users access to your AWS Cloud9 environment by attaching the `AWSCloud9User` managed policy to the user's role\. Unlike IAM users, you must manually attach and detach managed policies for federated users\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated AdministratorAccess managed policy or equivalent\.

**Prerequisities:**
+ You must have created a role or have an existing role that your federated user assumes\.
+ You must know which level of permissions you want to grant:
  + The `AWSCloud9User` managed policy allows the user to do the following:
    + Create their own AWS Cloud9 development environments\.
    + Get information about their own environments\.
    + Change the settings for their own environments\.
  + The `AWSCloud9Administrator` managed policy allows the user to do the following:
    + Create environments for themselves or others\.
    + Get information about environments for themselves or others\.
    + Delete environments for themselves or others\.
    + Change the settings of environments for themselves or others\.

1. Open the IAM console\. In the navigation pane of the console, choose **Policies**\.

1. Enter the policy name in the search field\. This example searches for the `AWSCloud9User` managed policy\. The managed policy is displayed, with a policy type of **AWS managed**\. You can expand the policy to see the permissions in the policy statement\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-cloud9-mgdpolicy-listing.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. Choose one of these managed policies\. This example attaches the `AWSCloud9User` managed policy\. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\. Choose **Attach**\.

1. On the **Attach Policy** page, filter for the federated user's role in the search field\. Choose the box next to the name of the role and then choose **Attach policy**\. The **Attached entities** tab shows the new attachment\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-cloud9-mgdpolicy-attached-entities.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

### Detach an AWS Cloud9 Managed Policy from the Federated User's Role<a name="access-permissions-federated-detach-Cloud9"></a>

To remove a federated user's AWS Cloud9 permissions, detach the policy\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated AdministratorAccess managed policy or equivalent\.

1. Open the IAM console\. In the navigation pane of the console, choose **Policies**\.

1. Enter your project name in the search field\.

1. Select the circle next to the policy and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\.

1. Filter for the federated user's role in the search field\. Choose **Detach**\.

## AWS CodeStar Service Role Policy and Permissions<a name="access-permissions-service-role"></a>

AWS CodeStar uses a service role, aws\-codestar\-service\-role, when creating and managing the resources for your project\. For more information, see "AWS service role" in [Roles Terms and Concepts](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) in the *IAM User Guide*\.

**Important**  
You must be signed in as an IAM administrator user or root account in order to create this service role\. For more information, see [First\-Time Access Only: Your Root User Credentials](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html#intro-identity-first-time-access) and [Creating Your First IAM Admin User and Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.

This role is created for you the first time you create a project in AWS CodeStar\. The service role acts on your behalf to create the resources you choose when creating a project and to display information about those resources in the AWS CodeStar project dashboard\. It also acts on your behalf when you manage the resources for a project\. It contains the following policy statement:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ProjectStack",
      "Effect": "Allow",
      "Action": [
        "cloudformation:*Stack*",
        "cloudformation:*ChangeSet*",
        "cloudformation:GetTemplate"
      ],
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/awscodestar-*",
        "arn:aws:cloudformation:*:*:stack/awseb-*",
        "arn:aws:cloudformation:*:*:stack/aws-cloud9-*",
        "arn:aws:cloudformation:*:aws:transform/CodeStar*"
      ]
    },
    {
      "Sid": "ProjectStackTemplate",
      "Effect": "Allow",
      "Action": [
        "cloudformation:GetTemplateSummary",
        "cloudformation:DescribeChangeSet"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ProjectQuickstarts",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::awscodestar-*/*"
      ]
    },
    {
      "Sid": "ProjectS3Buckets",
      "Effect": "Allow",
      "Action": [
        "s3:*"
      ],
      "Resource": [
        "arn:aws:s3:::aws-codestar-*",
        "arn:aws:s3:::aws-codestar-*/*",
        "arn:aws:s3:::elasticbeanstalk-*",
        "arn:aws:s3:::elasticbeanstalk-*/*"
      ]
    },
    {
      "Sid": "ProjectServices",
      "Effect": "Allow",
      "Action": [
        "codestar:*Project",
        "codestar:*Resource*",
        "codestar:List*",
        "codestar:Describe*",
        "codestar:Get*",
        "codestar:AssociateTeamMember",
        "codecommit:*",
        "codepipeline:*",
        "codedeploy:*",
        "codebuild:*",
        "ec2:RunInstances",
        "autoscaling:*",
        "cloudwatch:Put*",
        "ec2:*",
        "elasticbeanstalk:*",
        "elasticloadbalancing:*",
        "iam:ListRoles",
        "logs:*",
        "sns:*",
        "cloud9:CreateEnvironmentEC2",
        "cloud9:DeleteEnvironmentEC2",
        "cloud9:DescribeEnvironment*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ProjectWorkerRoles",
      "Effect": "Allow",
      "Action": [
        "iam:AttachRolePolicy",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:GetRole",
        "iam:PassRole",
        "iam:PutRolePolicy",
        "iam:SetDefaultPolicyVersion",
        "iam:CreatePolicy",
        "iam:DeletePolicy",
        "iam:AddRoleToInstanceProfile",
        "iam:CreateInstanceProfile",
        "iam:DeleteInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile"
      ],
      "Resource": [
        "arn:aws:iam::*:role/CodeStarWorker*",
        "arn:aws:iam::*:policy/CodeStarWorker*",
        "arn:aws:iam::*:instance-profile/awscodestar-*"
      ]
    },
    {
      "Sid": "ProjectTeamMembers",
      "Effect": "Allow",
      "Action": [
        "iam:AttachUserPolicy",
        "iam:DetachUserPolicy"
      ],
      "Resource": "*",
      "Condition": {
        "ArnEquals": {
          "iam:PolicyArn": [
            "arn:aws:iam::*:policy/CodeStar_*"
          ]
        }
      }
    },
    {
      "Sid": "ProjectRoles",
      "Effect": "Allow",
      "Action": [
        "iam:CreatePolicy",
        "iam:DeletePolicy",
        "iam:CreatePolicyVersion",
        "iam:DeletePolicyVersion",
        "iam:ListEntitiesForPolicy",
        "iam:ListPolicyVersions"
      ],
      "Resource": [
        "arn:aws:iam::*:policy/CodeStar_*"
      ]
    },
    {
      "Sid": "InspectServiceRole",
      "Effect": "Allow",
      "Action": [
        "iam:ListAttachedRolePolicies"
      ],
      "Resource": [
        "arn:aws:iam::*:role/aws-codestar-service-role"
      ]
    },
    {    
      "Sid": "IAMLinkRole",
      "Effect": "Allow",
      "Condition": {
        "StringLike": {
          "iam:AWSServiceName": "cloud9.amazonaws.com"
        }
      },
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": "arn:aws:iam::*:role/aws-service-role/cloud9.amazonaws.com/AWSServiceRoleForCloud9*"
    }
  ]
}
```

## Action and Resource Syntax<a name="access-permissions-syntax"></a>

The following sections describe the format for specifying actions and resources\.

Actions follow this general format:

```
codestar:action
```

Where *action* is an available AWS CodeStar operation, such as `ListProjects` or `AssociateResource`\. To allow an action, use the `"Effect" : "Allow"` clause\. To explicitly deny an action, use the `"Effect" : "Deny"` clause\. By default, all actions are denied, unless specified otherwise in any other attached policy\. 

Resources follow this general format:

```
arn:aws:codestar:region:account:resource-specifier
```

Where *region* is a target region \(such as **us\-east\-2**\), *account* is the AWS account ID, and *resource\-specifier* is the project ID\. Wildcard \(`*`\) characters can be used to specify a partial name\.

For example, the following specifies the AWS CodeStar project named `my-first-projec` registered to the AWS account `111111111111` in the region `us-east-2`:

```
arn:aws:codestar:us-east-2:111111111111:project/my-first-projec
```

The following specifies any AWS CodeStar project that begins with the name `my-proj` registered to the AWS account `111111111111` in the region `us-east-2`:

```
arn:aws:codestar:us-east-2:111111111111:project/my-proj*
```

**Topics**
+ [Resource Scoping in AWS CodeStar](#access-permissions-resource-scoping)
+ [Projects](#access-permissions-syntax-projects)
+ [Resources](#access-permissions-syntax-resources)
+ [Teams](#access-permissions-syntax-teams)
+ [Users](#access-permissions-syntax-users)

### Resource Scoping in AWS CodeStar<a name="access-permissions-resource-scoping"></a>

Some of the permissions in AWS CodeStar cannot be scoped to a resource, but instead must be scoped to all, or the action will fail\. 

The following action cannot be scoped\. It must be set to \*:
+ ListProjects

### Projects<a name="access-permissions-syntax-projects"></a>

Allowed actions include: 
+ `CreateProject` to create an AWS CodeStar project\. 
+ `DeleteProject` to delete an AWS CodeStar project\.
+ `DescribeProject` to describe the attributes of an AWS CodeStar project\.
+ `ListProjects` to list all the AWS CodeStar projects\. 
+ `UpdateProject` to update the attributes of an AWS CodeStar project\.

The following example allows a specified IAM user to edit the attributes of an AWS CodeStar project, such as its project description:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codestar:UpdateProject"
      ],
      "Resource" : "arn:aws:codestar:us-east-2:project/my-first-projec"
    }
  ]
}
```

### Resources<a name="access-permissions-syntax-resources"></a>

Allowed actions include:
+ `ListResources` to list all the resources for an AWS CodeStar project\.

The following example allows an IAM user who has this policy attached to list resources for a project with the ID *my\-first\-projec*:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codestar:ListResources",
      ],
      "Resource" : "arn:aws:codestar:us-east-2:project/my-first-projec"
    }
  ]
}
```

### Teams<a name="access-permissions-syntax-teams"></a>

Allowed actions include:
+ `AssociateTeamMember` to add a user to an AWS CodeStar project\.
+ `DisassociateTeamMember` to remove a user from an AWS CodeStar project\.
+ `ListTeamMembers` to list all the users in an AWS CodeStar project\.
+ `UpdateTeamMember` to change the team member's attributes in a AWS CodeStar project \(for example, the user's project role\)\. 

The following example allows an IAM user who has this policy attached to add team members to an AWS CodeStar project with the project ID *my\-first\-projec*, but explicitly denies that user the ability to remove team members:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codestar:AssociateTeamMember",
      ],
      "Resource" : "arn:aws:codestar:us-east-2:project/my-first-projec"
    },
    {
      "Effect" : "Deny",
      "Action" : [
        "codestar:DisassociateTeamMember",
      ],
      "Resource" : "arn:aws:codestar:us-east-2:project/my-first-projec"
    }
      ]
    
  ]
}
```

### Users<a name="access-permissions-syntax-users"></a>

Allowed actions include:
+ `CreateUserProfile` to create a user profile in AWS CodeStar\. This profile contains data associated with the user, such as a display name, that appears across all AWS CodeStar projects\.
+ `DeleteUserProfile` to delete an AWS CodeStar user profile\.
+ `DescribeUserProfile` to retrieve information about an AWS CodeStar user profile\.
+ `ListUserProfiles` to list all AWS CodeStar user profiles for an AWS account\.
+ `UpdateUserProfile` to update an AWS CodeStar profile for a user\.

The following example allows an IAM user who has this policy attached to list all AWS CodeStar user profiles associated with an AWS account:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codestar:ListUserProfiles",
              ],
      "Resource" : "*"
    }
  ]
}
```