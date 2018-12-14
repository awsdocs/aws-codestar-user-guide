# Step 4: Push a Change and Monitor the AWS CloudFormation Stack Update<a name="customize-ec2-multi-endpoints-stack"></a>

1. Make a change in your repository to start your pipeline\.

1. When the pipeline starts the **Deploy** stage, the AWS CloudFormation stack update starts\. You can choose the AWS CloudFormation stage in your pipeline on your AWS CodeStar dashboard to see the stack update notification\. To view stack creation details, in the console, choose your project from the **Events** list\. 

1. After successful completion of your pipeline, the resources are created in your AWS CloudFormation stack\. In the AWS CloudFormation console, choose the infrastructure stack for your project\. Stack names follow this format:
   + Lambda projects: `awscodestar-<project_name>-lambda-prod`
   + Amazon EC2 and Elastic Beanstalk projects: `awscodestar-<project_name>-infrastructure-prod`

   In the **Resources ** list in the AWS CloudFormation console, view the resource created for your project\. In this example, the new Amazon EC2 instance appears in the **Resources** section\.

1. Access the endpoint for your production stage:
   + For an Elastic Beanstalk project, open the new stack in the AWS CloudFormation console and expand **Resources**\. Choose the Elastic Beanstalk application\. The link opens in the Elastic Beanstalk console\. Choose **Environments**\. Choose the URL in **URL** to open the endpoint in a browser\.
   + For a Lambda project, open the new stack in the AWS CloudFormation console and expand **Resources**\. Choose the API Gateway resource\. The link opens in the API Gateway console\. Choose **Stages**\. Choose the URL in **Invoke URL** to open the endpoint in a browser\.
   + For an Amazon EC2 project, choose the new Amazon EC2 instance in your project resources list in the AWS CodeStar console\. The link opens on the **Instance** page of the Amazon EC2 console\. Choose the **Description** tab, copy the URL in **Public DNS \(IPv4\)**, and open the URL in a browser\.

1. Verify that your change is deployed\.