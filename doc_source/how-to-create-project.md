# Create a Project in AWS CodeStar<a name="how-to-create-project"></a>

You use the AWS CodeStar console to create a project\. If you use a project template, it sets up the required resources\. It also includes sample code you can use to start coding and to understand how the project resources work together\. 

To create a project, sign in to the AWS Management Console with an IAM user that has the **AWSCodeStarFullAccess** policy or equivalent permissions\.  For more information, see [Setting Up AWS CodeStar](setting-up.md)\.

**Note**  
You must complete the steps in [Setting Up AWS CodeStar](setting-up.md) before you can complete the procedures in this topic\.

## Create a Project in AWS CodeStar<a name="how-to-create-project-console"></a>

Use the AWS CodeStar console to create a project\.<a name="adh-create-project"></a>

**To create a project in AWS CodeStar**

1. Sign in to the AWS Management Console, and then open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

   Make sure that you are signed in to the AWS region where you want to create the project and its resources\. For example, to create a project in US East \(Ohio\), make sure you have that region selected\. For information about AWS regions where AWS CodeStar is available, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codestar_region) in the *AWS General Reference* \.  
![\[Choosing the region where you will create the project in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-region.png)

1. On the **AWS CodeStar** page, choose **Create a new project**\. \(If you are the first user to create a project, choose **Start a project**\.\)

1. On the **Choose a project template** page, choose the project type from the list of AWS CodeStar project templates\. You can use the filter bar to narrow your choices\. For example, for a web application project written in Node\.js that will be deployed to Amazon EC2 instances, select the **Web application**, **Node\.js**, and **Amazon EC2** check boxes\. Then choose from the templates available for that set of options\.  
![\[Using the filter bar to help choose the project template\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-create-new1.png)

   For more information, see [AWS CodeStar Project Templates](templates.md)\.

1. In **Project name**, type a name for the project, such as *My First Project*\. The ID for the project is derived from this project name, but is limited to 15 characters\. 

   For example, the default ID for a project named *My First Project* is *my\-first\-projec*\. This project ID is the basis for the names of all resources associated with the project\. For example, AWS CodeStar uses this project ID as part of the URL for your code repository as well as the names of related security access roles and policies in IAM\. After the project is created, the project ID cannot be changed, so make sure you are okay with this project ID\. To edit the project ID before you create the project, choose **Edit**\. 

   For information about the limits on project names and project IDs, see [Limits in AWS CodeStar](limits.md)\.
**Note**  
Project IDs must be unique for your AWS account within an AWS region\.   
![\[Providing a name and ID for your project in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-create-new2.png)

1. Choose the repository provider to store this project's source code with: **AWS CodeCommit** or **GitHub**\. 

1. If you chose **AWS CodeCommit**, for **Repository name**, accept the default AWS CodeCommit repository name that AWS CodeStar suggests, or type a different AWS CodeCommit repository name of your choice\. Then skip ahead to step 8 in this procedure\.

1. If you chose **GitHub**, then choose **Connect with GitHub**\. 

   1. If the **Sign in to GitHub** page is displayed, type your GitHub username or email address and password, and then choose **Sign in**\.
**Note**  
To complete this page, you must have a GitHub account\. For more information, see [Join GitHub](https://github.com/join) on the GitHub website\.

   1. If the **Two\-factor authentication** page displays, for **Authentication code**, type the code that GitHub sends you\. Then choose **Verify**\.

   1. On the **Authorize AWS CodeStar** page, choose **Authorize**\.
**Note**  
When you choose **Authorize**, you allow AWS CodeStar to create a GitHub repository for your personal GitHub account, or for any GitHub organization where you have permissions \(which is marked with a green check icon in **Organization access**\)\.  
To add a GitHub organization to the **Organization access** list, ask one of the organization's owners to invite you to the organization by following the instructions in [Inviting users to join your organization](https://help.github.com/articles/inviting-users-to-join-your-organization/) on the GitHub Help website\. After you join the organization, refresh the **Authorize AWS CodeStar** page to see the organization in the list\.   
To get permissions to authorize a GitHub organization that is in the list but does not have a green check icon, choose **Grant**\. If you see **Request** instead, choose it, and then ask one of the organization's owners to allow AWS CodeStar to create a GitHub repository in the organization by following the instructions in [Approving OAuth Apps for your organization](https://help.github.com/articles/approving-oauth-apps-for-your-organization/) on the GitHub Help website\. After the owner does this, refresh the **Authorize AWS CodeStar** page to see the **Grant** button\.

   1. For **Owner**, choose the GitHub organization or your personal GitHub account that you want AWS CodeStar to create the GitHub repository for\.

   1. For **Repository name**, accept the default GitHub repository name that AWS CodeStar suggests, or type a different GitHub repository name of your choice\.

   1. Choose **Public repository** or **Private repository** to make the GitHub repository public or private\.
**Note**  
Depending on your GitHub account type, GitHub may not allow you to create a private repository\. For more information, see [GitHub Pricing](https://github.com/pricing) on the GitHub website\.

   1. For **Repository description**, provide an optional description for the GitHub repository\.  
![\[Choosing GitHub repository settings for your project in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-create-project3.png)

1. Choose **Next**\.

1. Review the resources and configuration details\. Choose **Edit Amazon EC2 Configuration** \(where available\) if your project will deploy to Amazon EC2 instances and you want to make changes\. For example, you can choose from available instance types for your project\. 
**Note**  
Different Amazon EC2 instance types provide different levels of computing power and might have different associated costs\. For more information, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) and [Amazon EC2 Pricing](https://aws.amazon.com/ec2/pricing/)\.  
If you have more than one virtual private cloud \(VPC\) or multiple subnets created in Amazon Virtual Private Cloud, you can also choose the VPC and subnet to use\. However, if you choose an Amazon EC2 instance type that is not supported on dedicated instances, you cannot choose a VPC whose instance tenancy is set to **Dedicated**\.   
For more information, see [What Is Amazon VPC?](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) and [Dedicated Instance Basics](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/dedicated-instance.html#dedicated-howitworks)\.

1. Leave the **AWS CodeStar would like permission to administer AWS resources on your behalf** check box selected\. If this box is not selected, you will not be able to create a project\. For more information about the service role, the policy, and its permissions, see [AWS CodeStar Service Role Policy and Permissions](access-permissions.md#access-permissions-service-role)\.

   Choose **Next** or **Create project**\. \(The displayed choice depends on your project template\.\) 

1. In **Choose an Amazon EC2 Key Pair**, choose the Amazon EC2 key pair you created in [Step 4: Create an Amazon EC2 Key Pair for AWS CodeStar Projects](setting-up.md#setting-up-create-ec2-key) in *Setting Up*\. Select **I acknowledge that I have access to the private key file for this key pair**, and then choose **Create project**\.

1. It might take a few minutes to create the project \(including the repository\)\. After your project has a repository, you can use the **Set up tools** page to configure access to it, or you can choose **Skip** and configure access later\. After your project has been created, you will see a **Welcome** tile that contains useful links\. You can use these links to optionally configure other items, such as your [user profile in AWS CodeStar](working-with-user-info.md)\.   
![\[The Welcome tile displayed after you create a project\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-welcome-tile.png)

While your project is being created, you can [add team members](how-to-add-team-member.md) or [configure access](setting-up-ide.md) to your project repository from the command line or your favorite IDE\. 