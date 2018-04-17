# Remove Team Members from an AWS CodeStar Project<a name="how-to-remove-team-member"></a>

After you remove from an AWS CodeStar project, the user will still appear in the commit history for the project repository, but will no longer have access to the AWS CodeCommit repository or any other project resources, such as the project pipeline\. \(The exception to this rule is an IAM user who has other policies applied that grant access to those resources\.\) The user will not be able to access the project dashboard, and the project will no longer appear in the list of projects that user sees on the AWS CodeStar dashboard\.

**Important**  
Although removing a team member from a project will deny remote access to project Amazon EC2 instances, it will not close any of the user's active SSH sessions\.  
Removing a team member does not affect that team member's access to any resources that are outside of AWS, for example a GitHub repository or issues in Atlassian JIRA\. Those access permissions are controlled by the resource provider, not AWS CodeStar\. For more information, consult the resource provider's documentation\.  
Removing a team member from a project does not automatically delete that team member's related AWS Cloud9 development environments or prevent that member from participating in any related AWS Cloud9 development environments they have been invited to\. To delete a development environment, see [Delete an AWS Cloud9 Environment from a Project](setting-up-ide-cloud9.md#setting-up-ide-cloud9-delete)\. To prevent a team member from participating in a shared environment, see [Share an AWS Cloud9 Environment with a Project Team Member](setting-up-ide-cloud9.md#setting-up-ide-cloud9-share)\.

To remove a team member from a project, you must have the AWS CodeStar Owner role for that project or have the **AWSCodeStarFullAccess** policy applied to your account\.

**Topics**
+ [Remove Team Members Using the Console](#how-to-remove-team-member-console)
+ [Remove Team Members Using the AWS CLI](#how-to-remove-team-member-cli)

## Remove Team Members Using the Console<a name="how-to-remove-team-member-console"></a>

You can remove team members from your project team using the AWS CodeStar console\. 

**To remove a team member from a project**

1. Open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

   Choose the project\.

1. In the navigation bar for the project, choose **Team**\.

1. On the **Team members** page, find the name of the team member you want to remove, and then choose **Remove**\.

## Remove Team Members Using the AWS CLI<a name="how-to-remove-team-member-cli"></a>

You can remove team members from your project team using the AWS CLI\. 

**To remove a team member \(AWS CLI\)**

1. Open a terminal or command window\.

1. Run the disassociate\-team\-member command, including the `--project-id` and `-user-arn` parameters to remove a team member from your project\. For example:

   ```
   aws codestar disassociate-team-member --project-id my-first-projec --user-arn arn:aws:iam:111111111111:user/John_Doe 
   ```

   This command returns output similar to the following:

   ```
   {
       "projectId": "my-first-projec", 
       "userArn": "arn:aws:iam::111111111111:user/John_Doe"
   }
   ```