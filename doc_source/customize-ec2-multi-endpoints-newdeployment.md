# Step 1: Create an Application and Deployment Group in CodeDeploy \(Amazon EC2 Projects Only\)<a name="customize-ec2-multi-endpoints-newdeployment"></a>

You choose your CodeDeploy application and then add a new deployment group associated with the new instance\.

**Note**  
If your project is a Lambda or Elastic Beanstalk project, you can skip this step\.

1. Open the CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.

1. Choose **Create application**\.

1. In **Application name**, enter a name for your application that includes the project name and the Prod stage \(for example, <*project\-id*>\-*prod*\)\.

1. In **Compute platform**, choose **EC2/On\-premises**\.

1. Choose **Create application**\.

1. Under **Deployment groups**, choose **Create deployment group**\.

1. In **Deployment group name**, enter ***<project\-id>\-prod\-Env***\.

1. In **Service role**, choose the toolchain worker role for your AWS CodeStar project\.

1. Under **Deployment type**, choose **In\-place**\.

1. Under **Environment configuration**, choose the **Amazon EC2 Instances** tab\.

1. Under the tag group, under **Key**, choose `aws:cloudformation:stack-name`\. Under **Value**, choose `awscodestar-<projectid>-infrastructure-prod` \(the stack to be created for the **GenerateChangeSet** action\)\.

1. In **Deployment settings**, choose `CodeDeployDefault.AllAtOnce`\.

1. Clear **Choose a load balancer**\.

1. Choose **Create deployment group**\.

   Now your second deployment group has been created\.