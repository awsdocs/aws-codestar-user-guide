# Step 2: Add a New Pipeline Stage for the Prod Stage<a name="customize-ec2-multi-endpoints-newstage"></a>

Add a stage with the same set of deployment actions as your project's Deploy stage\. For example, the new Prod stage for an Amazon EC2 project should have the same actions as the Deploy stage created for the project\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-codestar-add-prod-stage.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

**To copy parameters and fields from the Deploy stage**

1. From your AWS CodeStar project dashboard, choose **Pipeline Details** to open your pipeline in the AWS CodePipeline console\.

1. Choose **Edit**\.

1. In the Deploy stage, choose **Edit stage**\.

1. Choose the edit icon on the **GenerateChangeSet** action\. Make a note of the values in the following fields\. You use these values when you create your new action\.
   + **Stack name**
   + **Change set name**
   + **Template**
   + **Input artifacts**

1. Expand **Advanced**, and in **Parameters**, copy the parameters for your project\. You paste these parameters into your new action\. For example, copy the parameters that are shown here in JSON format:
   + Lambda projects:

     ```
     {
       "ProjectId":"MyProject"
     }
     ```
   + Amazon EC2 projects:

     ```
     {
     
       "ProjectId":"MyProject",
       "InstanceType":"t2.micro",
       "WebAppInstanceProfile":"awscodestar-MyProject-WebAppInstanceProfile-EXAMPLEY5VSFS",
       "ImageId":"ami-EXAMPLE1",
       "KeyPairName":"my-keypair",
       "SubnetId":"subnet-EXAMPLE",
       "VpcId":"vpc-EXAMPLE1"
     }
     ```
   + Elastic Beanstalk projects:

     ```
     {
       "ProjectId":"MyProject",
       "InstanceType":"t2.micro",
       "KeyPairName":"my-keypair",
       "SubnetId":"subnet-EXAMPLE",
       "VpcId":"vpc-EXAMPLE",
       "SolutionStackName":"64bit Amazon Linux 2018.03 v3.0.5 running Tomcat 8 Java 8",
       "EBTrustRole":"CodeStarWorker-myproject-EBService",
       "EBInstanceProfile":"awscodestar-myproject-EBInstanceProfile-11111EXAMPLE"
     }
     ```

1. In the stage edit pane, choose **Cancel**\.

**To create a GenerateChangeSet action in your new Prod stage**

1. If your pipeline is not already displayed, from your AWS CodeStar project dashboard, choose **Pipeline Details** to open your pipeline in the console\.

1. Choose **Edit**\.

1. At the bottom of the diagram, choose **\+ Add stage**\.

1. Enter a stage name \(for example, **Prod**\), and then choose **\+ Add action group**\.

1. In **Action name**, enter a name \(for example, **GenerateChangeSet**\)\.

1. In **Action provider**, choose **AWS CloudFormation**\.

1. In **Action mode**, choose **Create or replace a change set**\.

1. In **Stack name**, enter a new name for the AWS CloudFormation stack that is to be created by this action\. Start with a name that is identical to the Deploy stack name, and then add **\-prod**:
   + Lambda projects: `awscodestar-<project_name>-lambda-prod`
   + Amazon EC2 and Elastic Beanstalk projects: `awscodestar-<project_name>-infrastructure-prod`
**Note**  
The stack name must start with **awscodestar\-<project\_name>\-** exactly, or the stack creation fails\.

1. In **Change set name**, enter the same change set name as provided in the existing Deploy stage \(for example, **pipeline\-changeset**\)\.

1. In **Template**, enter the same change template name as provided in the existing Deploy stage \(for example, **<project\-ID>\-BuildArtifact::template\.yml**\)\.

1. In **Capabilities**, choose **CAPABILITY\_NAMED\_IAM**\.

1. In **Role name**, choose the name of your project's AWS CloudFormation worker role\.

1. Expand **Advanced**, and in **Parameters**, paste the parameters for your project\. Include the `Stage` parameter, shown here in JSON format, for an Amazon EC2 project:

   ```
   {
   
     "ProjectId":"MyProject",
     "InstanceType":"t2.micro",
     "WebAppInstanceProfile":"awscodestar-MyProject-WebAppInstanceProfile-EXAMPLEY5VSFS",
     "ImageId":"ami-EXAMPLE1",
     "KeyPairName":"my-keypair",
     "SubnetId":"subnet-EXAMPLE",
     "VpcId":"vpc-EXAMPLE1",
     "Stage":"Prod"
   }
   ```
**Note**  
Make sure to paste all of the parameters for the project, not just new parameters or parameters you want to change\.

1. In **Input artifacts**, choose the build artifact\.

1. Choose **Save**\.

1. In the AWS CodePipeline pane, choose **Save pipeline change**, and then choose **Save change**\. View your updated pipeline\.

**To create an ExecuteChangeSet action in your new Prod stage**

1. If not already viewing your pipeline, from your AWS CodeStar project dashboard, choose **Pipeline Details** to open your pipeline in the console\.

1. Choose **Edit**\.

1. In your new Prod stage, after the new **GenerateChangeSet** action, choose **\+ Add action group**\.

1. In **Action name**, enter a name \(for example, **ExecuteChangeSet**\)\.

1. In **Action provider**, choose **AWS CloudFormation**\.

1. In **Action mode**, choose **Execute a change set**\.

1. In **Stack name**, enter the new name for the AWS CloudFormation stack that you entered in the GenerateChangeSet action \(for example, **awscodestar\-<project\-ID>\-infrastructure\-prod**\)\.

1. In **Change set name**, enter the same change set name used in the Deploy stage \(for example,**pipeline\-changeset**\)\.

1. Choose **Save**\.

1. In the AWS CodePipeline pane, choose **Save pipeline change**, and then choose **Save change**\. View your updated pipeline\.

**To create an AWS CodeDeploy Deploy action in your new Prod stage \(Amazon EC2 projects only\)**

1. After the new actions in your Prod stage, choose **\+ Action**\.

1. In **Action name**, enter a name \(for example, **Deploy**\)\.

1. In **Action provider**, choose **AWS CodeDeploy**\.

1. In **Application name**, choose the name of the new AWS CodeDeploy application you created in step 2\.

1. In **Deployment group**, choose the name of the new AWS CodeDeploy deployment group you created in step 2\.

1. In **Input artifacts**, choose the same build artifact used in the existing stage\.

1. Choose **Save**\.

1. In the AWS CodePipeline pane, choose **Save pipeline change**, and then choose **Save change**\. View your updated pipeline\.