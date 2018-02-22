# Change AWS Resources in an AWS CodeStar Project<a name="how-to-change-project"></a>

After you create a project in AWS CodeStar, you may want to change the default set of AWS resources that AWS CodeStar adds to the project\. The following information describes the types of changes AWS CodeStar supports\.

## Supported Resource Changes<a name="how-to-change-project-supported-actions"></a>

The following table lists the types of changes you may want to make to default AWS resources in an existing AWS CodeStar project and whether they are supported\.


| Change | Supported? | Notes | 
| --- | --- | --- | 
| Add a stage to AWS CodePipeline | Yes | See [Add a Stage to AWS CodePipeline](#how-to-change-project-codepipeline) | 
| Change Elastic Beanstalk environment settings | Yes | See [Change AWS Elastic Beanstalk Environment Settings](#how-to-change-project-beanstalk) | 
| Change an AWS Lambda function's code or settings, its related IAM role, or its related API in Amazon API Gateway | Yes | See [Change an AWS Lambda Project](#how-to-change-project-lambda) | 
| Add AWS X\-Ray support | No |  | 
| Switch to a different deployment target \(for example, deploy to AWS Elastic Beanstalk instead of AWS CodeDeploy\) | No |  | 
| Change an IAM role definition | No |  | 
| Add a friendly web endpoint name | No |  | 
| Change the AWS CodeCommit repository name \(for an AWS CodeStar project connected to AWS CodeCommit\) | No |  | 
| Disconnect the GitHub repository \(for an AWS CodeStar project connected to GitHub\) and then reconnect the repository to that project, or connect any other repository to that project | No | You can use the AWS CodePipeline console \(not the AWS CodeStar console\) to disconnect and reconnect to GitHub in a pipeline's **Source** stage\. However, if you reconnect the **Source** stage to a different GitHub repository, then in the AWS CodeStar dashboard for the project, the information in the **Application endpoints**, **Commit history**, and **GitHub Issues** tiles may be wrong or out of date\.  Disconnecting the GitHub repository does not remove that repository's information from the commit history and GitHub issues tiles in the AWS CodeStar dashboard for the project\. To remove this information, use the GitHub website to disable access to GitHub from the AWS CodeStar project\. To do this, on the GitHub website, revoke access using the **Authorized OAuth Apps** section of the settings page for your GitHub account profile\.  | 
| Disconnect the AWS CodeCommit repository \(for an AWS CodeStar project connected to AWS CodeCommit\) and then reconnect the repository to that project, or connect any other repository to that project | No |  | 

## Add a Stage to AWS CodePipeline<a name="how-to-change-project-codepipeline"></a>

You can add a new stage to a pipeline that AWS CodeStar creates in a project\. For more information, see [Edit a Pipeline in AWS CodePipeline](http://docs.aws.amazon.com/codepipeline/latest/userguide/pipelines-edit.html) in the *AWS CodePipeline User Guide*\.

**Note**  
If the new stage depends on any AWS resources that AWS CodeStar did not create, then the pipeline may break\. This is because the IAM role that AWS CodeStar created for AWS CodePipeline may not have access to those resources by default\.   
To attempt to give AWS CodePipeline access to AWS resources that AWS CodeStar did not create, you may want to change the IAM role that AWS CodeStar created\. However, note that this is not supported\. This is because AWS CodeStar may remove your IAM role changes whenever it performs regular update checks on the project\.

## Change AWS Elastic Beanstalk Environment Settings<a name="how-to-change-project-beanstalk"></a>

You can change the settings of an Elastic Beanstalk environment that AWS CodeStar creates in a project\. For more information, see [The AWS Elastic Beanstalk Environment Management Console](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-console.html) in the *AWS Elastic Beanstalk Developer Guide*\.

## Change an AWS Lambda Project<a name="how-to-change-project-lambda"></a>

You can change the code or settings of a Lambda function, or its related IAM role or API Gateway API, that AWS CodeStar creates in a project\. To do this, we recommend you use the AWS Serverless Application Model \(AWS SAM\) along with the `template.yaml` file in your project's AWS CodeCommit repository\. This `template.yaml` file defines your function's name, handler, runtime, related IAM role, and related API in API Gateway\. For more information, see [How to create serverless applications using AWS SAM](https://github.com/awslabs/serverless-application-model/blob/master/HOWTO.md) on the GitHub website\.