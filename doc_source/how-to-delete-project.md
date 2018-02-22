# Delete an AWS CodeStar Project<a name="how-to-delete-project"></a>

If you no longer need a project, you can delete it and its resources so that you do not incur any further charges in AWS\. When you delete a project, all team members are removed from that project\. Their project roles are removed from their IAM users, but their user profiles in AWS CodeStar are not changed\.<a name="adh-keep-resources"></a>

**Important**  
Deleting a project in AWS CodeStar cannot be undone\. By default all AWS resources for the project are deleted in your AWS account, including:  
The AWS CodeCommit repository for the project along with anything stored in that repository\.
The AWS CodeStar project roles and the associated IAM policies configured for the project and its resources\.
Any Amazon EC2 instances created for the project\.
The deployment application and associated resources, such as:  
An AWS CodeDeploy application and associated deployment groups\.
An AWS Lambda function and associated API Gateway APIs\.
An AWS Elastic Beanstalk application and associated environment\.
The continuous deployment pipeline for the project in AWS CodePipeline\.
The AWS CloudFormation stacks associated with the project\.
Any AWS Cloud9 development environments created with the AWS CodeStar console\. All uncommitted code changes in the environments will be lost\. 
To delete all project resources along with the project, select the **Delete associated resources along with AWS CodeStar project** check box\. If you clear this option, the project will be deleted in AWS CodeStar, and the project roles that enabled access to those resources will be deleted in IAM, but all other resources will be retained\. You might continue to incur charges for these resources in AWS\. If you decide you no longer want one or more of these resources, you must manually delete them\. For more information about manually deleting resources after a project has been deleted, see [Project deletion: An AWS CodeStar project was deleted, but resources still exist](troubleshooting.md#troubleshooting-pd1)\.  
If you decide to keep resources when deleting a project, as a best practice, copy the list of resources from the project details page before you delete an AWS CodeStar project\. This way, you will have a record of all resources that you have kept, even though the project no longer exists\.


+ [Delete a Project in AWS CodeStar Using the Console](#how-to-delete-project-console)
+ [Delete a Project in AWS CodeStar Using the AWS CLI](#how-to-delete-project-cli)

## Delete a Project in AWS CodeStar Using the Console<a name="how-to-delete-project-console"></a>

Use the AWS CodeStar console to delete a project\.<a name="adh-delete-project"></a>

**To delete a project in AWS CodeStar**

1. Open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Find the project in the list, and from the ellipsis menu, choose **Delete**\.

   Alternatively, open the project, and in the navigation pane, choose **Project**\. On the project details page, choose **Delete project**\.

1. In the box next to **Type the following project ID to confirm**, type the ID of the project, and then choose **Delete**\.

   Deleting a project can take several minutes\. After it's deleted, the project no longer appears in the list of projects in the AWS CodeStar console\. 
**Important**  
By default, when you delete a project, all resources listed under **Project resources** are deleted\. If you clear the check box, the project resources will be retained\. For more information, go [here](#adh-keep-resources)\.   
If your project uses resources outside of AWS, for example a GitHub repository or issues in Atlassian JIRA, those resources are not deleted, even if the check box is selected\.

## Delete a Project in AWS CodeStar Using the AWS CLI<a name="how-to-delete-project-cli"></a>

You can use the AWS CLI to delete a project in AWS CodeStar\. 

**To delete a project in AWS CodeStar \(AWS CLI\)**

1. >At a terminal \(Linux, macOS, or Unix\) or command prompt \(Windows\), run the delete\-project command, including the name of the project\. For example, to delete a project with the ID *my\-2nd\-project*: 

   ```
   aws codestar delete-project --id my-2nd-project
   ```

   This command returns output similar to the following:

   ```
   {
       "projectArn":"arn:aws:codestar:us-east-2:111111111111:project/my-2nd-project"
   }
   ```

1. Run the list\-projects command and verify that the deleted project no longer appears in the list of projects associated with your AWS account\.

   ```
   aws codestar list-projects
   ```