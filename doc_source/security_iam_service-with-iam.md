# How AWS CodeStar Works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to AWS CodeStar, you should understand what IAM features are available to use with AWS CodeStar\. To get a high\-level view of how AWS CodeStar and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [AWS CodeStar Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies)
+ [AWS CodeStar Resource\-Based Policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization Based on AWS CodeStar Tags](#security_iam_service-with-iam-tags)
+ [AWS CodeStar IAM Roles](#security_iam_service-with-iam-roles)
+ [IAM User Access to AWS CodeStar](#security_iam_service-with-iam-roles-user)
+ [Federated User Access to AWS CodeStar](#security_iam_service-with-iam-roles-federated)
+ [Using Temporary Credentials with AWS CodeStar](#security_iam_service-with-iam-roles-tempcreds)
+ [Service\-Linked Roles](#security_iam_service-with-iam-roles-service-linked)
+ [Service Roles](#security_iam_service-with-iam-roles-service)

## AWS CodeStar Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources and the conditions under which actions are allowed or denied\. AWS CodeStar creates several identity\-based policies on your behalf, which allow AWS CodeStar to create and manage resources within the scope of an AWS CodeStar project\. AWS CodeStar supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in AWS CodeStar use the following prefix before the action: `codestar:`\. For example, to allow a specified IAM user to edit the attributes of an AWS CodeStar project, such as its project description, you could use the following policy statement:

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

 Policy statements must include either an `Action` or `NotAction` element\. AWS CodeStar defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "codestar:action1",
      "codestar:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `List`, include the following action:

```
"Action": "codestar:List*"
```



To see a list of AWS CodeStar actions, see [Actions Defined by AWS CodeStar](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscodestar.html#awscodestar-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```



The AWS CodeStar project resource has the following ARN:

```
arn:aws:codestar:region:account:project/resource-specifier
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, the following specifies the AWS CodeStar project named `my-first-projec` registered to the AWS account `111111111111` in the AWS Region `us-east-2`:

```
arn:aws:codestar:us-east-2:111111111111:project/my-first-projec
```

The following specifies any AWS CodeStar project that begins with the name `my-proj` registered to the AWS account `111111111111` in the AWS Region `us-east-2`:

```
arn:aws:codestar:us-east-2:111111111111:project/my-proj*
```

Some AWS CodeStar actions, such as for listing projects, cannot be performed on a resource\. In those cases, you must use the wildcard \(\*\)\.

```
"LisProjects": "*"
```

To see a list of AWS CodeStar resource types and their ARNs, see [Resources Defined by AWS CodeStar](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscodestar.html#awscodestar-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS CodeStar](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscodestar.html#awscodestar-actions-as-permissions)\.

### Condition Keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

AWS CodeStar does not provide any service\-specific condition keys, but it does support using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of AWS CodeStar identity\-based policies, see [AWS CodeStar Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

## AWS CodeStar Resource\-Based Policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

AWS CodeStar does not support resource\-based policies\.

## Authorization Based on AWS CodeStar Tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to AWS CodeStar projects or pass tags in a request to AWS CodeStar\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `codestar:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging AWS CodeStar resources, see [Working with Project Tags in AWS CodeStar](working-with-project-tags.md)\.

To view an example identity\-based policy for limiting access to an AWS CodeStar project based on the tags on that project, see [Viewing AWS CodeStar Projects Based on Tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-widget-tags)\.

## AWS CodeStar IAM Roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity in your AWS account that has specific permissions\.

 You can use AWS CodeStar as an [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html), a federated user, the root user, or an assumed role\. All user types with the appropriate permissions can manage project permissions to their AWS resources, but AWS CodeStar manages project permissions automatically for IAM users\. [IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) and [roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) grant permissions and access to that user based on the project role\. You can use the IAM console to create other policies that assign AWS CodeStar and other permissions to an IAM user\. 

For example, you might want to allow a user to view, but not change, an AWS CodeStar project\. In this case, you add the IAM user to an AWS CodeStar project with the viewer role\. Every AWS CodeStar project has a set of policies that help you control access to the project\. In addition, you can control which users have access to AWS CodeStar\.

AWS CodeStar access is handled differently for IAM users and federated users\. Only IAM users can be added to teams\. To grant IAM users permissions to projects, you add the user to the project team and assign the user a role\. To grant federated users permissions to projects, you manually attach the AWS CodeStar project role's managed policy to the federated user's role\.

 This table summarizes the tools available for each type of access\.


****  

| Permissions feature | IAM user | Federated user | Root user | 
| --- | --- | --- | --- | 
| SSH key management for remote access for Amazon EC2 and Elastic Beanstalk projects | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 
| AWS CodeCommit SSH access | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 
|  IAM user permissions managed by AWS CodeStar  | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 
|  Project permissions managed manually  |  | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) | 
| Users can be added to project as team members | ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-checkmark.png) |  |  | 

## IAM User Access to AWS CodeStar<a name="security_iam_service-with-iam-roles-user"></a>

When you add an IAM user to a project and choose a role for the user, AWS CodeStar applies the appropriate policy to the IAM user automatically\. For IAM users, you don't need to directly attach or manage policies or permissions in IAM\. For information about adding an IAM user to an AWS CodeStar project, see [Add Team Members to an AWS CodeStar Project ](how-to-add-team-member.md)\. For information about removing an IAM user from an AWS CodeStar project, see [Remove Team Members from an AWS CodeStar Project ](how-to-remove-team-member.md)\.

### Attach an Inline Policy to an IAM User<a name="security_iam_service-with-iam-roles-user-policy"></a>

When you add a user to a project, AWS CodeStar automatically attaches the managed policy for the project that matches the user's role\. You should not manually attach an AWS CodeStar managed policy for a project to an IAM user\. With the exception of `AWSCodeStarFullAccess`, we do not recommend that you attach policies that change an IAM user's permissions in an AWS CodeStar project\. If you decide to create and attach your own policies, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

## Federated User Access to AWS CodeStar<a name="security_iam_service-with-iam-roles-federated"></a>

Instead of creating an IAM user or using the root user, you can use user identities from AWS Directory Service, your enterprise user directory, a web identity provider, or IAM users assuming roles\. These are known as *federated users*\.

Grant federated users access to your AWS CodeStar project by manually attaching the managed policies described in [AWS CodeStar Project\-Level Policies and Permissions](security_iam-proj.md) to the user's IAM role\. You attach the owner, contributor, or viewer policy after AWS CodeStar creates your project resources and IAM roles\. 



**Prerequisites: **
+ You must have set up an identity provider\. For example, you could set up a SAML identity provider and set up AWS authentication through the provider\. For more information about setting up an identity provider, see [Creating IAM Identity Providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create.html)\. For more information about SAML federation, see [About SAML 2\.0\-based Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)\.
+ You must have created a role for a federated user to assume when access is requested through an [identity provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html)\. An STS trust policy must be attached to the role that allows federated users to assume the role\. For more information, see [Federated Users and Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html#intro-access-roles) in the *IAM User Guide*\.
+ You must have created your AWS CodeStar project and know the project ID\.

For more information about creating a role for identity providers, see [Creating a Role for a Third\-Party Identity Provider \(Federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html)\.

### Attach the AWSCodeStarFullAccess Managed Policy to the Federated User's Role<a name="security_iam_service-with-iam-roles-federated-attach-FullAccess"></a>

Grant a federated user permissions to create a project by attaching the `AWSCodeStarFullAccess` managed policy\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated `AdministratorAccess` managed policy or equivalent\.

**Note**  
After you create the project, your project owner permissions are not applied automatically\. Using a role with administrative permissions for your account, attach the owner managed policy, as described in [Attach Your Project's AWS CodeStar Viewer/Contributor/Owner Managed Policy to the Federated User's Role](#security_iam_service-with-iam-roles-federated-attach-CodeStar)\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter `AWSCodeStarFullAccess` in the search field\. The policy name is displayed, with a policy type of **AWS managed**\. You can expand the policy to see the permissions in the policy statement\.

1. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\. Choose **Attach**\.

1. On the **Attach Policy** page, filter for the federated user's role in the search field\. Select the box next to the name of the role, and then choose **Attach policy**\. The **Attached entities** tab displays the new attachment\.

### Attach Your Project's AWS CodeStar Viewer/Contributor/Owner Managed Policy to the Federated User's Role<a name="security_iam_service-with-iam-roles-federated-attach-CodeStar"></a>

Grant federated users access to your project by attaching the appropriate owner, contributor, or viewer managed policy to the user's role\. The managed policy gives the appropriate level of permissions\. Unlike IAM users, you must manually attach and detach managed policies for federated users\. This is equivalent to assigning project permissions to team members in AWS CodeStar\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated `AdministratorAccess` managed policy or equivalent\.

**Prerequisites:**
+ You must have created a role or have an existing role that your federated user assumes\.
+ You must know which level of permissions you want to grant\. The managed policies attached to the owner, contributor, and viewer roles provide role\-based permissions for your project\.
+ Your AWS CodeStar project must have been created\. The managed policy is not available in IAM until the project is created\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter your project ID in the search field\. The policy name matching your project is displayed, with a policy type of **Customer managed**\. You can expand the policy to see the permissions in the policy statement\.

1. Choose one of these managed policies\. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\. Choose **Attach**\.

1. On the **Attach Policy** page, filter for the federated user's role in the search field\. Select the box next to the name of the role and then choose **Attach policy**\. The **Attached entities** tab displays the new attachment\.

### Detach an AWS CodeStar Managed Policy from the Federated User's Role<a name="security_iam_service-with-iam-roles-federated-detach-CodeStar"></a>

Before you delete your AWS CodeStar project, you must manually detach any managed policies you attached to a federated user's role\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated `AdministratorAccess` managed policy or equivalent\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter your project ID in the search field\.

1. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\.

1. Filter for the federated user's role in the search field\. Choose **Detach**\.

### Attach an AWS Cloud9 Managed Policy to the Federated User's Role<a name="security_iam_service-with-iam-roles-federated-attach-Cloud9"></a>

If you are using an AWS Cloud9 development environment, grant federated users access to it by attaching the `AWSCloud9User` managed policy to the user's role\. Unlike IAM users, you must manually attach and detach managed policies for federated users\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated `AdministratorAccess` managed policy or equivalent\.

**Prerequisites:**
+ You must have created a role or have an existing role that your federated user assumes\.
+ You must know which level of permissions you want to grant:
  + The `AWSCloud9User` managed policy allows the user to do the following:
    + Create their own AWS Cloud9 development environments\.
    + Get information about their environments\.
    + Change the settings for their environments\.
  + The `AWSCloud9Administrator` managed policy allows the user to do the following for themselves or others:
    + Create environments\.
    + Get information about environments\.
    + Delete environments\.
    + Change the settings of environments\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter the policy name in the search field\. The managed policy is displayed, with a policy type of **AWS managed**\. You can expand the policy to see the permissions in the policy statement\.

1. Choose one of these managed policies\. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\. Choose **Attach**\.

1. On the **Attach Policy** page, filter for the federated user's role in the search field\. Choose the box next to the name of the role and then choose **Attach policy**\. The **Attached entities** tab displays the new attachment\.

### Detach an AWS Cloud9 Managed Policy from the Federated User's Role<a name="security_iam_service-with-iam-roles-federated-detach-Cloud9"></a>

If you are using an AWS Cloud9 development environment, you can remove a federated user's access to it by detaching the policy that grants access\. To perform these steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the associated `AdministratorAccess` managed policy or equivalent\.

1. Open the IAM console\. In the navigation pane, choose **Policies**\.

1. Enter your project name in the search field\.

1. Select the circle next to the policy, and then under **Policy actions**, choose **Attach**\.

1. On the **Summary** page, choose the **Attached entities** tab\.

1. Filter for the federated user's role in the search field\. Choose **Detach**\.

## Using Temporary Credentials with AWS CodeStar<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

AWS CodeStar supports the use of temporary credentials, but the AWS CodeStar team member functionality doesn't work for federated access\. AWS CodeStar team member functionality only supports adding an IAM user as a team member\.

## Service\-Linked Roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view, but cannot edit, the permissions for service\-linked roles\.

AWS CodeStar does not support service\-linked roles\. 

## Service Roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

AWS CodeStar supports service roles\. AWS CodeStar uses a service role, aws\-codestar\-service\-role, when it creates and manages the resources for your project\. For more information, see [Roles Terms and Concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) in the *IAM User Guide*\.

**Important**  
You must be signed in as an IAM administrator user or root account to create this service role\. For more information, see [First\-Time Access Only: Your Root User Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_identity-management.html#intro-identity-first-time-access) and [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.

This role is created for you the first time you create a project in AWS CodeStar\. The service role acts on your behalf to:
+ Create the resources you choose when you create a project\.
+ Display information about those resources in the AWS CodeStar project dashboard\.

It also acts on your behalf when you manage the resources for a project\. For an example of this policy statement, see [ AWSCodeStarServiceRole Policy](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-service-role)\. 

In addition, AWS CodeStar creates several project\-specific service roles, depending on the project type\. AWS CloudFormation and toolchain roles are created for each project type\.
+ AWS CloudFormation roles allow AWS CodeStar to access AWS CloudFormation to create and modify stacks for your AWS CodeStar project\.
+ Toolchain roles allow AWS CodeStar to access other AWS services to create and modify resources for your AWS CodeStar project\.