# Setting Up AWS CodeStar<a name="setting-up"></a>

Before you can start using AWS CodeStar, you must complete the following steps\. The account you use to sign in to AWS must be configured to allow the following actions:

**Topics**
+ [Step 1: Create an AWS Account](#setting-up-create-account)
+ [Step 2: Create the AWS CodeStar Service Role](#setting-up-create-service-role)
+ [Step 3: Create or Use an IAM User](#setting-up-create-iam-user)
+ [Step 4: Create an Amazon EC2 Key Pair for AWS CodeStar Projects](#setting-up-create-ec2-key)
+ [Step 5: Open the AWS CodeStar Console](#setting-up-open-console)
+ [Next Steps](#setting-up-next-steps)

## Step 1: Create an AWS Account<a name="setting-up-create-account"></a>

Create an AWS account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **Sign Up**\.

## Step 2: Create the AWS CodeStar Service Role<a name="setting-up-create-service-role"></a>

AWS CodeStar requires the creation of a [service role](access-permissions.md#access-permissions-service-role) in order to create and manage AWS resources and IAM permissions\. You only need to create the service role once\. 

**Important**  
You must be signed in as an IAM administrative user \(or root account\) in order to create this service role\. For more information about administrative users, see [Creating Your First IAM User and Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\.

1. Open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Choose **Start project**\. \(If you do not see **Start project** but instead are directed to the projects list page, the service role has been created\. You can jump ahead to [Step 3: Create or Use an IAM User](#setting-up-create-iam-user)\.\)

1. In the **Create service role** dialog, choose **Yes, create role**\.

1. Exit project creation\. You'll come back to this later\.

## Step 3: Create or Use an IAM User<a name="setting-up-create-iam-user"></a>

To use AWS CodeStar, create an [IAM user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) \(or use an existing one in your AWS account\), and then sign in to the console with that IAM user\. You need an AWS access key ID and an AWS secret access key associated with your IAM user\. 

**Important**  
To create IAM users, you must be logged in as an administrative user\.  
AWS CodeStar does not support federated users\. Using AWS CodeStar with a root account is not recommended\.

After you have an IAM user, do one of the following:
+ If you want to create projects in AWS CodeStar, apply the **AWSCodeStarFullAccess** managed policy to your IAM user\. For more information, see [Working with Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#attach-managed-policy-console)\. 

  Alternatively, you can create projects if you have an IAM administrative user with full permissions for all AWS services\. 
+ If your IAM user has already been added to one or more AWS CodeStar projects, it already has the policies and permissions required to access the service and resources for the projects you belong to\. To set up your local computer for working with AWS CodeStar projects, follow the steps in [Getting Started](getting-started.md#git-credentials)\. You can also [sign in to the AWS CodeStar console](https://console.aws.amazon.com/codestar/) and configure your user profile\. For more information, see [Manage Display Information for Your AWS CodeStar User Profile ](how-to-manage-user-pref.md) and [Add a Public Key to Your AWS CodeStar User Profile ](how-to-add-ec2-key.md)\.

## Step 4: Create an Amazon EC2 Key Pair for AWS CodeStar Projects<a name="setting-up-create-ec2-key"></a>

Many AWS CodeStar projects use AWS CodeDeploy or AWS Elastic Beanstalk to deploy code to Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. To access Amazon EC2 instances associated with your project, create an Amazon EC2 key pair for your IAM user\. Your IAM user must have permissions to create and manage Amazon EC2 keys \(for example, permission to take the actions `ec2:CreateKeyPair` and `ec2:ImportKeyPair`\)\. For more information, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

## Step 5: Open the AWS CodeStar Console<a name="setting-up-open-console"></a>

Sign in to the AWS Management Console, and then open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

## Next Steps<a name="setting-up-next-steps"></a>

Congratulations, you have completed the setup\! To start working with AWS CodeStar, see [Getting Started with AWS CodeStar](getting-started.md)\. 