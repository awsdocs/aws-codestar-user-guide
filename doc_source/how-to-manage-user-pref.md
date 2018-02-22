# Manage Display Information for Your AWS CodeStar User Profile<a name="how-to-manage-user-pref"></a>

You can change your display name and email information in AWS CodeStar\. This information is part of your AWS CodeStar user profile, which is not project\-specific, but instead displays in every project you belong to within an AWS region\. Because this information is associated with your IAM user, it will be applied across the AWS CodeStar projects you belong to in that region\. If you belong to projects in more than one AWS region, you will have a separate user profile in each region\. 

You can only manage your own user profile in the AWS CodeStar console\. If you have the `AWSCodeStarFullAccess` policy, you can view and manage other profiles using the AWS CLI\.

**Note**  
The information in this topic covers only your AWS CodeStar user profile\. If your project uses resources outside of AWS, for example a GitHub repository or issues in Atlassian JIRA, those resource providers may use separate user profiles, which may have different settings\. For more information, see the resource provider's documentation\.


+ [Manage Your User Profile Using the AWS CodeStar Console](#how-to-manage-user-pref-console)
+ [Manage User Profiles Using the AWS CLI](#how-to-manage-user-pref-cli)

## Manage Your User Profile Using the AWS CodeStar Console<a name="how-to-manage-user-pref-console"></a>

You can manage your user profile in the AWS CodeStar console by navigating to any project where you are a team member and changing your profile information\. Because user profiles are user\-specific and not project\-specific, your user profile changes will appear in every project where you are a team member within an AWS region\.

**Important**  
To change the display information for a user in the console, you must be signed in as that IAM user\. No other user, even those with AWS CodeStar Owner role for a project or with the **AWSCodeStarFullAccess** policy applied, can change your display information in the console\.

**To change your display information in all projects within an AWS region \(console\)**

1. Open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

   Choose a project where you are a team member\.

1. In the navigation bar for the project, choose **Team**\.  
![\[Team members in an AWS CodeStar project\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-team-list.png)

1. On the **Team members** page, find the name of your IAM user \(the team member that has your IAM name in parentheses, and has **\[You\]** in brackets next to the display name\), and then choose **Edit**\.  
![\[Customizable information for your IAM user in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-team-displayinfo-limit.png)

1. Edit the display name, the email address, or both, and then choose **Save**\.
**Note**  
Both a display name and an email address are required\. For more information, see [Limits in AWS CodeStar](limits.md)\. 

## Manage User Profiles Using the AWS CLI<a name="how-to-manage-user-pref-cli"></a>

You can use the AWS CLI to create and manage your user profile in AWS CodeStar\. You can also use the AWS CLI to view your user profile information, and to view all user profiles configured for your AWS account in an AWS region\. 

Make sure that your AWS profile is configured for the region where you want to create, manage, or view user profiles, as user profiles are region\-specific\. 

**To create a user profile \(AWS CLI\)**

1. Open a terminal or command window\.

1. Run the create\-user\-profile command, including the `user-arn`, `display-name`, and `email-address` parameters\. For example:

   ```
   aws codestar create-user-profile --user-arn arn:aws:iam:111111111111:user/John_Stiles --display-name "John Stiles" --email-address "john_stiles@example.com"
   ```

   This command returns output similar to the following:

   ```
   {
   	"createdTimestamp":1.491439687681E9,"
   	displayName":"John Stiles",
   	"emailAddress":"john.stiles@example.com",
   	"lastModifiedTimestamp":1.491439687681E9,
   	"userArn":"arn:aws:iam::111111111111:user/Jane_Doe"
   }
   ```

**To view your display information \(AWS CLI\)**

1. Open a terminal or command window\.

1. Run the describe\-user\-profile command, including the `user-arn` parameter\. For example:

   ```
   aws codestar describe-user-profile --user-arn arn:aws:iam:111111111111:user/Mary_Major
   ```

   This command returns output similar to the following:

   ```
   {
   	"createdTimestamp":1.490634364532E9,
   	"displayName":"Mary Major",
   	"emailAddress":"mary.major@example.com",
   	"lastModifiedTimestamp":1.491001935261E9,
   	"sshPublicKey":"EXAMPLE=",
   	"userArn":"arn:aws:iam::111111111111:user/Mary_Major"
   }
   ```

**To change your display information \(AWS CLI\)**

1. Open a terminal or command window\.

1. Run the update\-user\-profile command, including the `user-arn` parameter and the profile parameters you want to change, such as, `display-name` or `email-address` parameters\. For example, if a user with the display name "Jane Doe" wanted to change her display name to "Jane Mary Doe":

   ```
   aws codestar update-user-profile --user-arn arn:aws:iam:111111111111:user/Jane_Doe --display-name "Jane Mary Doe"
   ```

   This command returns output similar to the following:

   ```
   {
   	"createdTimestamp":1.491439687681E9,
   	"displayName":"Jane Mary Doe",
   	"emailAddress":"jane.doe@example.com",
   	"lastModifiedTimestamp":1.491442730598E9,
   	"sshPublicKey":"EXAMPLE1",
   	"userArn":"arn:aws:iam::111111111111:user/Jane_Doe"
   }
   ```

**To list all user profiles in an AWS region in your AWS account \(AWS CLI\)**

1. Open a terminal or command window\.

1. Run the aws codestar list\-user\-profiles command\. For example:

   ```
   aws codestar list-user-profiles 
   ```

   This command returns output similar to the following:

   ```
   {
     "userProfiles":[
   	{
   		"displayName":"Jane Doe",
   		"emailAddress":"jane.doe@example.com",
   		"sshPublicKey":"EXAMPLE1",
   		"userArn":"arn:aws:iam::111111111111:user/Jane_Doe"
   	},
   	{
   		"displayName":"John Doe",
   		"emailAddress":"john.doe@example.com",
   		"sshPublicKey":"EXAMPLE2",
   		"userArn":"arn:aws:iam::111111111111:user/John_Doe"
   	},
   	{
   		"displayName":"Mary Major",
   		"emailAddress":"mary.major@example.com",
   		"sshPublicKey":"EXAMPLE=",
   		"userArn":"arn:aws:iam::111111111111:user/Mary_Major"
   	},
   	{
   		"displayName":"John Stiles",
   		"emailAddress":"john.stiles@example.com",
   		"sshPublicKey":"",
   		"userArn":"arn:aws:iam::111111111111:user/John_Stiles"
   	}
     ]
   }
   ```