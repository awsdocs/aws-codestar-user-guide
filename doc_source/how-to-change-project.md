# Change AWS Resources in an AWS CodeStar Project<a name="how-to-change-project"></a>

After you create a project in AWS CodeStar, you can change the default set of AWS resources that AWS CodeStar adds to the project\. 

## Supported Resource Changes<a name="how-to-change-project-supported-actions"></a>

The following table lists the supported changes to default AWS resources in an AWS CodeStar project\.


| Change | Notes | 
| --- | --- | 
| Add a stage to AWS CodePipeline\. | See [Add a Stage to AWS CodePipeline](#how-to-change-project-codepipeline)\. | 
| Change Elastic Beanstalk environment settings\. | See [Change AWS Elastic Beanstalk Environment Settings](#how-to-change-project-beanstalk)\. | 
| Change an AWS Lambda function's code or settings, its IAM role, or its API in Amazon API Gateway\. | See [Change an AWS Lambda Function in Source Code](#how-to-change-project-lambda)\. | 
| Add a resource to an AWS Lambda project and expand permissions to create and access the new resource\. | See [Add a Resource to a Project](#customize-project-template)\. | 
| Add traffic shifting with AWS CodeDeploy for an AWS Lambda function\. | See [Shift Traffic for an AWS Lambda Project](how-to-modify-serverless-project.md)\. | 
| Add AWS X\-Ray support | See [Enable Tracing for a Project](#customize-project-xray)\. | 
| Modify the buildspec\.yml file for your project to add a unit test build phase AWS CodeBuild runs\. | See [Step 7: Add a Unit Test to the Web Service](sam-tutorial.md#sam-tutorial-add-unit-tests)\. | 
| Add your own IAM role to your project\. | See [Add an IAM Role to a Project](#add-iam-role) | 
| Change an IAM role definition\. | For roles defined in the application stack\. You cannot change roles defined in the toolchain or AWS CloudFormation stacks\. | 
| Edit the buildspec\.yml file for your project to add a unit test build phase AWS CodeBuild runs\. | See [Step 7: Add a Unit Test to the Web Service](sam-tutorial.md#sam-tutorial-add-unit-tests)\. | 
| Modify your Lambda project to add an endpoint |  | 
| Modify your EC2 project to add an endpoint |  | 
| Modify your Elastic Beanstalk project to add an endpoint | TBD | 
| Edit your project to add a Prod stage and endpoint | See [Add a Prod Stage and Endpoint to a Project](#customize-ec2-multi-endpoints)\. | 

The following changes are not supported\.
+ Switch to a different deployment target \(for example, deploy to AWS Elastic Beanstalk instead of AWS CodeDeploy\)\.
+ Add a friendly web endpoint name\.
+ Change the AWS CodeCommit repository name \(for an AWS CodeStar project connected to AWS CodeCommit\)\.
+ For an AWS CodeStar project connected to GitHub, disconnect the GitHub repository, and then reconnect the repository to that project, or connect any other repository to that project\. You can use the AWS CodePipeline console \(not the AWS CodeStar console\) to disconnect and reconnect to GitHub in a pipeline's **Source** stage\. However, if you reconnect the **Source** stage to a different GitHub repository, in the AWS CodeStar dashboard for the project, the information in the **Application endpoints**, **Commit history**, and **GitHub Issues** tiles might be wrong or out of date\. Disconnecting the GitHub repository does not remove that repository's information from the commit history and GitHub issues tiles in the AWS CodeStar project dashboard\. To remove this information, use the GitHub website to disable access to GitHub from the AWS CodeStar project\. To revoke access, on the GitHub website, use the **Authorized OAuth Apps** section of the settings page for your GitHub account profile\.
+ Disconnect the AWS CodeCommit repository \(for an AWS CodeStar project connected to AWS CodeCommit\) and then reconnect the repository to that project, or connect any other repository to that project\.

## Add a Stage to AWS CodePipeline<a name="how-to-change-project-codepipeline"></a>

You can add a new stage to a pipeline that AWS CodeStar creates in a project\. For more information, see [Edit a Pipeline in AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-edit.html) in the *AWS CodePipeline User Guide*\.

**Note**  
If the new stage depends on any AWS resources that AWS CodeStar did not create, the pipeline might break\. This is because the IAM role that AWS CodeStar created for AWS CodePipeline might not have access to those resources by default\.   
To attempt to give AWS CodePipeline access to AWS resources that AWS CodeStar did not create, you might want to change the IAM role that AWS CodeStar created\. This is not supported because AWS CodeStar might remove your IAM role changes when it performs regular update checks on the project\.

## Change AWS Elastic Beanstalk Environment Settings<a name="how-to-change-project-beanstalk"></a>

You can change the settings of an Elastic Beanstalk environment that AWS CodeStar creates in a project\. For more information, see [AWS Elastic Beanstalk Environment Management Console](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-console.html) in the *AWS Elastic Beanstalk Developer Guide*\.

## Change an AWS Lambda Function in Source Code<a name="how-to-change-project-lambda"></a>

You can change the code or settings of a Lambda function, or its IAM role or API Gateway API, that AWS CodeStar creates in a project\. To do this, we recommend that you use the AWS Serverless Application Model \(AWS SAM\) along with the `template.yaml` file in your project's AWS CodeCommit repository\. This `template.yaml` file defines your function's name, handler, runtime, IAM role, and API in API Gateway\. For more information, see [How to Create Serverless Applications Using AWS SAM](https://github.com/awslabs/serverless-application-model/blob/master/HOWTO.md) on the GitHub website\.

## Enable Tracing for a Project<a name="customize-project-xray"></a>

AWS X\-Ray offers tracing, which you can use to analyze performance behavior of distributed applications \(for example, latencies in response times\)\. After you add traces to your AWS CodeStar project, you can use the AWS X\-Ray console to view application views and response times\.

**Note**  
You can use these steps for the following projects, created with the following project support changes:   
Any Lambda project\.
For Amazon EC2 or Elastic Beanstalk projects created after August 3, 2018, AWS CodeStar provisioned a `/template.yml`file in the project repository\.

Each AWS CodeStar template includes an AWS CloudFormation file that models your application's AWS runtime dependencies, such as database tables and Lambda functions\. This file is stored in your source repository in the file `/template.yml`\.

You can modify this file to add tracing by adding the AWS X\-Ray resource to the `Resources` section\. You then modify the IAM permissions for your project to allow AWS CloudFormation to create the resource\. For information about template elements and formatting, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)\.

These are the high\-level steps to follow to customize your template\. 

1.  [Step 1: Edit the Worker Role in IAM for Tracing](#customize-project-xray-edit-worker)

1. [Step 2: Modify the template\.yml File for Tracing](#customize-project-xray-edit-template)

1. [Step 3: Commit and Push Your Template Change for Tracing](#customize-project-xray-commit-template)

1. [Step 4: Monitor the AWS CloudFormation Stack Update for Tracing ](#customize-project-xray-stack-update) 

### Step 1: Edit the Worker Role in IAM for Tracing<a name="customize-project-xray-edit-worker"></a>

You must be signed in as an administrator to perform steps 1 and 4\. This step shows an example of editing permissions for a Lambda project\.

**Note**  
You can skip this step if your project was provisioned with a permissions boundary policy\.  
For projects created after December 6, 2018 PDT, AWS CodeStar provisioned your project with a permissions boundary policy\.

1. Sign in to the AWS Management Console and open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Create a project or choose an existing project with a `template.yml file`, and then open the **Project resources** page\.

1. Under **Project Resources**, locate the IAM role created for the CodeStarWorker/Lambda role in the resource list\. The role name follows this format: `role/CodeStarWorker-Project_name-lambda-Function_name`\. Choose the ARN for the role\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-project-resources-xray.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. The role opens in the IAM console\. Choose **Attach policies**\. Search for the **AWSXrayWriteOnlyAccess** policy, select the box next to it, and then choose **Attach Policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-xray-modify-cloudformationrole.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

### Step 2: Modify the template\.yml File for Tracing<a name="customize-project-xray-edit-template"></a>

1. Open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Choose your serverless project and then open the **Code** page\. In the top level of your repository, locate and edit the `template.yml` file\. Under `Resources`, paste the resource into the `Properties` section \.

   ```
         Tracing: Active
   ```

   This example shows a modified template:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-xray-template-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

### Step 3: Commit and Push Your Template Change for Tracing<a name="customize-project-xray-commit-template"></a>
+ Commit and push the changes in the `template.yml` file\.
**Note**  
This starts your pipeline\. If you commit the changes before you update IAM permissions, your pipeline starts, the AWS CloudFormation stack update encounters errors, and the stack update is rolled back\. If this happens, correct the permissions and then restart your pipeline\.

### Step 4: Monitor the AWS CloudFormation Stack Update for Tracing<a name="customize-project-xray-stack-update"></a>

1. The AWS CloudFormation stack update starts when the pipeline for your project starts the **Deploy** stage\. To see the status of the stack update, on your AWS CodeStar dashboard, choose the AWS CloudFormation stage in your pipeline\.

   If the stack update in AWS CloudFormation returns errors, see troubleshooting guidelines in [AWS CloudFormation: Stack Creation Rolled Back for Missing Permissions](troubleshooting.md#troubleshooting-cloudformation-stack-creation-permissions)\. If permissions are missing from the worker role, edit the policy attached to your project's Lambda worker role\. See [Step 1: Edit the Worker Role in IAM for Tracing](#customize-project-xray-edit-worker)\.

1. Use the dashboard to view the successful completion of your pipeline\. Tracing is now enabled on your application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-xray-successful-pipeline.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1.  Verify that tracing is enabled by viewing the details for your function in the Lambda console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-xray-tracing-enabled.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. Choose the application endpoint for your project\. This interaction with your application is traced\. You can view tracing information in the AWS X\-Ray console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-xray-console-tracelist.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

## Add a Resource to a Project<a name="customize-project-template"></a>

Each CodeStar template for all projects come with an AWS CloudFormation file that models your application's AWS runtime dependencies, such as database tables and Lambda functions\. This is stored within your source repository in the file `/template.yml`\.

**Note**  
You can use these steps for the following projects, created with the following project support changes:   
Any Lambda project\.
For Amazon EC2 or Elastic Beanstalk projects created after August 3, 2018, AWS CodeStar provisioned a `/template.yml`file in the project repository\.

You can modify this file by adding AWS CloudFormation resources to the `Resources` section\. Modifying the `template.yml` file allows AWS CodeStar and AWS CloudFormation to add the new resource to your project\. Some resources require you to add other permissions to the policy for your project's CloudFormation worker role\. For information about template elements and formatting, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)\.

After you determine which resources you must add to your project, these are the high\-level steps to follow to customize a template\. For a list of AWS CloudFormation resources and their required properties, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\.

1.  [Step 1: Edit the CloudFormation Worker Role in IAM](#customize-project-template-sqs-2) \(if necessary\)

1. [Step 2: Modify the template\.yml File](#customize-project-template-sqs-1)

1. [Step 3: Commit and Push Your Template Change](#customize-project-template-sqs-3)

1. [Step 4: Monitor the AWS CloudFormation Stack Update ](#customize-project-template-sqs-4) 

1. [Step 5: Add Resource Permissions with an Inline Policy](#customize-project-template-sqs-5)

Use the steps in this section to modify your AWS CodeStar project template to add a resource and then expand the project's CloudFormation worker role's permissions in IAM\. In this example, the [AWS::SQS::Queue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sqs-queues.html) resource is added to the `template.yml` file\. The change starts an automated response in AWS CloudFormation that adds an Amazon Simple Queue Service queue to your project\.

### Step 1: Edit the CloudFormation Worker Role in IAM<a name="customize-project-template-sqs-2"></a>

You must be signed in as an administrator to perform steps 1 and 5\.

**Note**  
You can skip this step if your project was provisioned with a permissions boundary policy\.  
For projects created after December 6, 2018 PDT, AWS CodeStar provisioned your project with a permissions boundary policy\.

1. Sign in to the AWS Management Console and open the AWS CodeStar console, at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Create a project or choose an existing project with a `template.yml file`, and then open the **Project resources** page\.

1. Under **Project Resources**, locate the IAM role created for the CodeStarWorker/AWS CloudFormation role in the resource list\. The role name follows this format: `role/CodeStarWorker-Project_name-CloudFormation`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-project-resources-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. The role opens in the IAM console\. On the **Permissions** tab, in **Inline Policies**, expand the row for your service role policy, and choose **Edit Policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-modify-cloudformationrole.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. Choose the **JSON** tab to edit the policy\.
**Note**  
The policy attached to the worker role is `CodeStarWorkerCloudFormationRolePolicy`\.

1. In the **JSON** field, add the following policy statement in the `Statement` element\.

   ```
   {
     "Action": [
       "sqs:CreateQueue",
       "sqs:DeleteQueue",
       "sqs:GetQueueAttributes",
       "sqs:SetQueueAttributes",
       "sqs:ListQueues",
       "sqs:GetQueueUrl"
     ],
     "Resource": [
       "*"
     ],
     "Effect": "Allow"
   }
   ```

1. Choose **Review policy** to ensure the policy contains no errors, and then choose **Save changes**\.

### Step 2: Modify the template\.yml File<a name="customize-project-template-sqs-1"></a>

1. Open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Choose your serverless project and then open the **Code** page\. In the top level of your repository, make a note of the location of `template.yml`\.

1. Use an IDE, the console, or the command line in your local repository to edit the `template.yml` file in your repository\. Paste the resource into the `Resources` section\. In this example, when the following text is copied, it adds the `Resources` section\.

   ```
   Resources:
     TestQueue:
       Type: AWS::SQS::Queue
   ```

   This example shows a modified template:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-template-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

### Step 3: Commit and Push Your Template Change<a name="customize-project-template-sqs-3"></a>
+ Commit and push the changes in the `template.yml` file that you saved in step 2\.
**Note**  
This starts your pipeline\. If you commit the changes before you update IAM permissions, your pipeline starts and the AWS CloudFormation stack update encounters errors, which causes the stack update to be rolled back\. If this happens, correct the permissions and then restart your pipeline\.

### Step 4: Monitor the AWS CloudFormation Stack Update<a name="customize-project-template-sqs-4"></a>

1. When the pipeline for your project starts the **Deploy** stage, the AWS CloudFormation stack update starts\. You can choose the AWS CloudFormation stage in your pipeline on your AWS CodeStar dashboard to see the stack update\.

   **Troubleshooting:**

   The stack update fails if required resource permissions are missing\. View the failure status in the AWS CodeStar dashboard view for your project's pipeline\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-pipeline-deployfailed.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

   Choose the **CloudFormation** link in your pipeline's **Deploy** stage to troubleshoot the failure in the AWS CloudFormation console\. In the console, in the **Events** list, choose your project to view stack creation details\. There is a message with the failure details\. In this example, the `sqs:CreateQueue` permission is missing\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-pipeline-deployfailed-stack.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

   Add any missing permissions by editing the policy attached to your project's AWS CloudFormation worker role\. See [Step 1: Edit the CloudFormation Worker Role in IAM](#customize-project-template-sqs-2)\.

1. After a successful run of your pipeline, the resources are created in your AWS CloudFormation stack\. In the **Resources** list in AWS CloudFormation, view the resource created for your project\. In this example, the TestQueue queue is listed in the **Resources ** section\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-CFn-resources-window-lambda.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

   The queue URL is available in AWS CloudFormation\. The queue URL follows this format:

   ```
   https://{REGION_ENDPOINT}/queue.|api-domain|/{YOUR_ACCOUNT_NUMBER}/{YOUR_QUEUE_NAME}
   ```

   For more information, see [Send an Amazon SQS Message](https://docs.aws.amazon.com/sdk-for-net/v2/developer-guide/SendMessage.html#send-sqs-message), [Receive a Message from an Amazon SQS Queue](https://docs.aws.amazon.com/sdk-for-net/v2/developer-guide/ReceiveMessage.html#receive-sqs-message), and [Delete a Message from an Amazon SQS Queue](https://docs.aws.amazon.com/sdk-for-net/v2/developer-guide/DeleteMessage.html#delete-sqs-message)\. 

### Step 5: Add Resource Permissions with an Inline Policy<a name="customize-project-template-sqs-5"></a>

Grant team members access to your new resource by adding the appropriate inline policy to the user's role\. Not all resources require that you add permissions\. To perform the following steps, you must have signed in to the console either as a root user, an IAM administrator user in the account, or an IAM user or federated user with the `AdministratorAccess` managed policy or equivalent\.

1. In the IAM console, choose **Users**, and then choose the user account\.

1. On the **Permissions** tab, choose **Add inline policy**\. On the **Create policy** page, choose the **JSON** tab\.

1. On the **JSON** tab, paste the policy for your new resource\. Use the permissions that are appropriate for the level of access you want to provide\. This example provides the view\-only policy for the Amazon SQS resource\. Copy and paste the JSON policy provided here\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "sqs:Get*",
           "sqs:List*",
           "sqs:Receive*"
         ],
         "Resource": "*"
       }
     ]
   }
   ```

1. Choose **Review policy**, and then choose **Create policy**\.

1. In **Name**, enter a name for your inline policy\. To make your policy easier to find and modify, include your AWS CodeStar project name in the policy title\.

1. The updated statement is displayed as attached directly to the user\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-policy-inline.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

1. If necessary, you can copy the policy to other team members\.
**Important**  
AWS CodeStar does not manage the inline policy created for an added resource\. To remove the user's permissions or clean up deleted resources, you must manually delete the inline policy\.

## Add an IAM Role to a Project<a name="add-iam-role"></a>

As of December 6, 2018 PDT you can define your own roles and polices in the application stack \(template\.yml\)\. To mitigate risks of privilege escalation and destructive actions, you are required to set the project\-specific permissions boundary for every IAM entity you create\. If you have a Lambda project with multiple functions, it is a best practice to create an IAM role for each function\.

**To add an IAM role to your project**

1. Edit the `template.yml` file for your project\.

1. In the `Resources:` section, add your IAM resource, using the format in the following example:

   ```
     SampleRole:
     Description: Sample Lambda role
     Type: AWS::IAM::Role
     Properties:
       AssumeRolePolicyDocument:
         Statement:
         - Effect: Allow
           Principal:
             Service: [lambda.amazonaws.com]
           Action: sts:AssumeRole
       ManagedPolicyArns:
         -  arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
       PermissionsBoundary: !Sub 'arn:${AWS::Partition}:iam::${AWS::AccountId}:policy/CodeStar_${ProjectId}_PermissionsBoundary'
   ```

1. Release your changes through the pipeline and verify success\.

## Add a Prod Stage and Endpoint to a Project<a name="customize-ec2-multi-endpoints"></a>

Use the procedures in this section to add a new production \(Prod\) stage to your pipeline and a manual approval stage between your pipeline's Deploy and Prod stages\. This creates an additional resource stack when your project pipeline runs\. 

**Note**  
You can use these procedures if:   
For projects created after August 3, 2018, AWS CodeStar provisioned your Amazon EC2, Elastic Beanstalk, or Lambda project with a template\.yml file in the project repository\.
For projects created after December 6, 2018 PDT, AWS CodeStar provisioned your project with a permissions boundary policy\.

All AWS CodeStar projects use an AWS CloudFormation template file that models your application's AWS runtime dependencies, such as Linux instances and Lambda functions\. The `/template.yml` file is stored in your source repository \.

In the `/template.yml` file, use the `Stage` parameter to add a resource stack for a new stage in the project pipeline\.

```
  Stage:
    Type: String
    Description: The name for a project pipeline stage, such as Staging or Prod, for which resources are provisioned and deployed.
    Default: ''
```

The `Stage` parameter is applied to all named resources with the project ID referenced in the resource\. For example, the following role name is a named resource in the template:

```
RoleName: !Sub 'CodeStar-${ProjectId}-WebApp${Stage}'
```

**Prerequisite:**

Use the template options in the AWS CodeStar console to create a project\. 

**Topics**
+ [Step 1: Create an Application and Deployment Group in AWS CodeDeploy \(Amazon EC2 Projects Only\)](customize-ec2-multi-endpoints-newdeployment.md)
+ [Step 2: Add a New Pipeline Stage for the Prod Stage](customize-ec2-multi-endpoints-newstage.md)
+ [Step 3: Add a Manual Approval Stage](customize-ec2-multi-endpoints-approval.md)
+ [Step 4: Push a Change and Monitor the AWS CloudFormation Stack Update](customize-ec2-multi-endpoints-stack.md)