# AWS CodeStar Project Templates<a name="templates"></a>

You can use an AWS CodeStar project template to quickly configure AWS CodeStar to support your development project\. These preconfigured AWS CloudFormation templates create projects based on your choices\. They include support for development projects like websites, web services, microservices, Alexa skills, and more\. You can use the search box or the filter bar to find a template\.

**Topics**
+ [How Do I Choose the Right Template?](#templates-choose)
+ [Web Application](#templates-webapps)
+ [Web Service](#templates-webservice)
+ [Amazon Alexa Skill](#templates-alexa)
+ [Use AWS CodeStar Templates with Built\-in Unit Testing](#templates-unit-testing)

## How Do I Choose the Right Template?<a name="templates-choose"></a>

Each AWS CodeStar project template includes the supported programming language in its title and description\. The template name also indicates whether your project is hosted on servers in the cloud \(Amazon EC2, either in a managed application environment \(AWS Elastic Beanstalk\) or that you manage yourself \) or run serverless \(without Amazon EC2 instances, for example on AWS Lambda\)\. If you see two AWS CodeStar project templates that look the same, check the description and the service bar to determine the differences\. 

After you choose an AWS CodeStar project template, the page displays a list of resources to be created for the project\. All of these resources are configured for you\. You do not have to perform any manual configuration to get started with your project\. If your project template includes Amazon EC2 instances, you can choose **Edit Amazon EC2 Configuration** to modify your configuration\. Some of these choices, such as instance type, might affect the cost of your project\. For more information, see [Create a Project in AWS CodeStar](how-to-create-project.md) and [Pricing](https://aws.amazon.com/codestar/pricing/)\.

![\[Editing the configuration details for a project before it's created\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-create-new2a.png)

Many of the AWS CodeStar project templates allow you to choose from a variety of options to host your web application or service in the cloud\. All options offer high availability and scaling\. The option you choose is configured when the project is created\. You don't have to worry about configuring interoperation or setting permissions\. The following table can help you determine the best fit for your software project\.


**Which hosting option is right for my AWS CodeStar project?**  

| [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) | Amazon EC2 with [AWS CodeDeploy](https://aws.amazon.com/codedeploy/) | [AWS Lambda](https://aws.amazon.com/lambda/)  | 
| --- | --- | --- | 
| Automated deployments to Amazon EC2 instances\. | Automated deployments to Amazon EC2 instances\. | Serverless \(no servers or instances to manage or administer\)\. | 
| Automated management of capacity and load balancing\. | Flexible deployment to any instance\. | AWS CodeBuild configured to build your artifacts automatically\. | 
| Team member access to Amazon EC2 instances \(if your project owner allows it\)\. | Team member access to Amazon EC2 instances \(if your project owner allows it\)\. | Amazon API Gateway configured automatically as a Lambda proxy for GET and POST calls\. | 
| End\-to\-end application management solution\. | Building block service focused on deploying and updating software\. | Code executed in response to events\. | 

After your project is created, you can view the sample source code included in your project, including a readme file that provides details of files and directories\. This readme file also includes suggestions for how to get started with the sample code\. 

## Web Application<a name="templates-webapps"></a>

The AWS CodeStar project templates in this category support development in Ruby, Java, ASP\.NET, PHP, and more\. A source repository and continuous delivery pipeline is configured for you automatically, along with a sample application that you can use to evaluate the AWS CodeStar project\. You can choose AWS services to use for your application\.

All web application projects include the following resources:
+ A source code repository in AWS CodeCommit or GitHub\.
+ A continuous deployment pipeline in AWS CodePipeline\. 
+ A CPU utilization monitor for Amazon EC2 instances \(Amazon EC2 and AWS Elastic Beanstalk projects\) or an Invocations and Errors monitor \(AWS Lambda projects\) in Amazon CloudWatch\.
+ Project roles and associated policies in IAM\. Policies are applied automatically to IAM users when you add those users to your project team\.
+ Sample code for your project, including a README\.md with details of the sample\.

If you choose an AWS CodeStar project template that uses Lambda, your project also includes the following resources:
+ A build server and environment in AWS CodeBuild\. 
+ A sample function in Lambda\.
+ A RESTful API that exposes the Lambda function in Amazon API Gateway\.
+ Roles for job workers in IAM\.

If you chose to create a project with AWS Lambda, you can add resources to your AWS CodeStar project by editing the template\.yaml file that is included in the sample code for Lambda projects\. Configurable resources include:
+ Applications and deployment groups in AWS CodeDeploy\.
+ Applications and environments in AWS Elastic Beanstalk\.
+ Stages and actions in a pipeline in AWS CodePipeline\.
+ Events in Amazon CloudWatch\.
+ Build projects in AWS CodeBuild\.

## Web Service<a name="templates-webservice"></a>

This template supports development in Ruby, Java, ASP\.NET, PHP, and more\. A source repository, build server, and continuous delivery pipeline are configured for you automatically, along with CloudWatch metrics\. This template also includes some sample code that you can use to help evaluate the AWS CodeStar project and its resources\. 

All web service projects include the following resources:
+ A source code repository in AWS CodeCommit or GitHub\.
+ A continuous deployment pipeline in AWS CodePipeline\. 
+ A CPU utilization monitor for Amazon EC2 instances \(Amazon EC2 and AWS Elastic Beanstalk projects\) or an Invocations and Errors monitor \(AWS Lambda projects\) in Amazon CloudWatch\.
+ Sample code for your project, including a README\.md with details of the sample\.

If you choose a AWS CodeStar project template that uses Lambda, your project also includes the following resources:
+ A build server and environment in AWS CodeBuild \.
+ A sample function in Lambda\.
+ A RESTful API that exposes the Lambda function in Amazon API Gateway\.
+ Roles for job workers in IAM\.

To view application activity in an AWS CodeStar project template that uses Lambda, you must first invoke the function by choosing to visit the host\. The host link appears on the **Continuous deployment** tile of your project\.

If you chose to create a project with AWS Lambda, you can add resources to your AWS CodeStar project by editing the template\.yaml file that is included in the sample code for Lambda projects\. Configurable resources include:
+ Applications and deployment groups in AWS CodeDeploy\.
+ Applications and environments in AWS Elastic Beanstalk\.
+ Stages and actions in a pipeline in AWS CodePipeline\.
+ Events in Amazon CloudWatch\.
+ Build projects in AWS CodeBuild\.

## Amazon Alexa Skill<a name="templates-alexa"></a>

Choose this template if you want a project for a AWS Lambda function based on an Alexa skills blueprint for [Amazon Alexa](https://developer.amazon.com/alexa-skills-kit)\. The function returns an Amazon Resource Name \(ARN\) that you can use as a service endpoint for your Alexa skill when you configure it in the Alexa Developer Portal\. For more information, see [Creating an AWS Lambda Function for a Custom Skill](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/developing-an-alexa-skill-as-a-lambda-function)\.

**Note**  
Lambda functions for Alexa skills are supported in the US East \(N\. Virginia\) and EU \(Ireland\) Regions only\.

All Alexa skill projects include the following resources:
+ A source code repository in AWS CodeCommit or GitHub\.
+ A continuous deployment pipeline in AWS CodePipeline\. 
+ An Invocations and Errors monitor in Amazon CloudWatch\.
+ Sample code for your project, including a README\.md with details of the sample\.
+ A build server and environment in AWS CodeBuild\.
+ A sample function in Lambda\.
+ A RESTful API that exposes the Lambda function in Amazon API Gateway\.
+ Roles for job workers in IAM\.

To view application activity in an AWS CodeStar project template that uses Lambda, you must first invoke the function by choosing to visit the host\. The host link appears on the **Continuous deployment** tile of your project\.

You can add resources to your AWS CodeStar project by editing the template\.yaml file that is included in the sample code for Lambda projects\. You can configure these resources:
+ Applications and deployment groups in AWS CodeDeploy\.
+ Applications and environments in AWS Elastic Beanstalk\.
+ Stages and actions in a pipeline in AWS CodePipeline\.
+ Events in Amazon CloudWatch\.
+ Build projects in AWS CodeBuild\.

## Use AWS CodeStar Templates with Built\-in Unit Testing<a name="templates-unit-testing"></a>

Most AWS CodeStar project templates come preconfigured with a unit testing framework and sample unit tests \(in all but PHP, static HTML, and Alexa skill project templates\)\. This enables you to quickly include unit testing in your project's included build process\. Source code for these tests can be found in your project's source repository\. Unit testing details can be found in the README file for your selected template\.

You can also add unit testing manually by modifying the buildspec\.yml file in your project's source code repository\. For an example for the Python programming language, see [Step 7: Add a Unit Test to the Web Service](sam-tutorial.md#sam-tutorial-add-unit-tests)\.