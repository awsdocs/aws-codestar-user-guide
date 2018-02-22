# Tutorial: Creating and Managing a Serverless Project in AWS CodeStar<a name="sam-tutorial"></a>

In this tutorial, you will use AWS CodeStar to create a project that uses the AWS Serverless Application Model \(AWS SAM\) to create and manage AWS resources for a web service hosted in AWS Lambda\.

AWS CodeStar uses AWS SAM, which relies on AWS CloudFormation, to provide a simplified way of creating and managing supported AWS resources, including Amazon API Gateway APIs, AWS Lambda functions, and Amazon DynamoDB tables\. \(This project does not use any Amazon DynamoDB tables\.\)

For more information about AWS SAM, see [AWS Serverless Application Model \(AWS SAM\)](https://github.com/awslabs/serverless-application-model) on GitHub\.

**Prerequisite:** Complete the steps in [Setting Up AWS CodeStar](setting-up.md)\.

**Note**  
Your AWS account may be charged for costs related to this tutorial, including costs for AWS services used by AWS CodeStar\. For more information, see [AWS CodeStar Pricing](https://aws.amazon.com/codestar/pricing)\.


+ [Overview](#sam-tutorial-overview)
+ [Step 1: Create the Project](#sam-tutorial-create-project)
+ [Step 2: Explore Project Resources](#sam-tutorial-explore-project)
+ [Step 3: Test the Web Service](#sam-tutorial-test-service)
+ [Step 4: Set Up Your Local Workstation to Edit Project Code](#sam-tutorial-setup-workstation)
+ [Step 5: Add Logic to the Web Service](#sam-tutorial-add-logic)
+ [Step 6: Test the Enhanced Web Service](#sam-tutorial-test-enhancements)
+ [Step 7: Add a Unit Test to the Web Service](#sam-tutorial-add-unit-tests)
+ [Step 8: View Unit Test Results](#sam-tutorial-view-unit-tests)
+ [Step 9: Clean Up](#sam-tutorial-clean-up)
+ [Next Steps](#sam-tutorial-next-steps)

## Overview<a name="sam-tutorial-overview"></a>

In this tutorial, you will do the following:

1. Use AWS CodeStar to create a project that uses AWS SAM to build and deploy a Python\-based web service\. This web service is hosted in AWS Lambda and can be accessed through Amazon API Gateway\.

1. Explore the project's main resources, which include:

   + The AWS CodeCommit repository where the project's source code is stored\. This source code includes the web service's logic and defines related AWS resources\.

   + The AWS CodePipeline pipeline that automates the building of the source code\. This pipeline uses AWS SAM to create and deploy a function to AWS Lambda, create a related API in Amazon API Gateway, and connect the API to the function\.

   + The function that is deployed to AWS Lambda\.

   + The API that is created in Amazon API Gateway\.

1. Manually test the web service to confirm that AWS CodeStar built and deployed the web service as expected\.

1. Set up your local workstation to work with the project's source code\.

1. Change the project's source code using your local workstation\. You add a function to the project and then push your changes to the source code, which instructs AWS CodeStar to rebuild and redeploy the web service\.

1. Manually test the web service again to confirm that AWS CodeStar rebuilt and redeployed as expected\.

1. Write a unit test using your local workstation to replace some of your manual testing with an automated test\. Push the unit test, which instructs AWS CodeStar to rebuild and redeploy the web service and run the unit test automatically\.

1. View the results of the unit tests\.

1. Clean up the project\. This step is to keep AWS from charging your AWS account for costs related to this tutorial\.

## Step 1: Create the Project<a name="sam-tutorial-create-project"></a>

In this step, you use the AWS CodeStar console to create a project\. This project uses AWS SAM to create and deploy a Python\-based web service that is hosted in AWS Lambda and can be accessed through Amazon API Gateway\.

1. Sign in to the AWS Management Console and open the AWS CodeStar console, at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.
**Note**  
You must sign in to the AWS Management Console using credentials associated with the IAM user you created or identified in [Setting Up AWS CodeStar](setting-up.md)\. This user must have the AWS managed policy named **AWSCodeStarFullAccess** attached\.

1. Choose the AWS region where you want to create the project and its resources\.   
![\[Choosing the region where you create the project in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/adh-region.png)

    For information about AWS regions where AWS CodeStar is available, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#codestar_region) in the *AWS General Reference*\.

1. Choose **Create a new project** \(or, if **Create a new project** is not displayed, **Start a new project**\)\.

1. On the **Choose a project template** page:: 

   + For **Application category**, select **Web service**\.

   + For **Programming languages**, select **Python**\.

   + For **AWS services**, select **AWS Lambda**\.

1. Choose the box that contains your selections\.  
![\[Choosing the AWS SAM project in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-create.png)

1. For **Project name**, type a name for the project \(for example, **My SAM Project**\)\. If you type a name different from the example, be sure to use it throughout this tutorial\.

   For **Project ID**, AWS CodeStar chooses a related identifier for this project \(for example, **my\-sam\-project**\)\. If you see a different project ID, be sure to use it throughout this tutorial\.

   Leave **AWS CodeCommit** selected, and do not change the **Repository name** value\. 

1. Choose **Next**\.

1. Leave the **AWS CodeStar would like permission to administer AWS resources on your behalf** box selected, and then choose **Create Project**\.

   If this is your first time using AWS CodeStar in this AWS region, then for **Display Name** and **Email**, type the display name and email address you want AWS CodeStar to use for your IAM user\. Choose **Next**\.

1. On the **Choose how you want to edit your project code** page, choose **Skip**\. You set up your local workstation to edit the project's code in a later step\.

1. Wait while AWS CodeStar creates the project\. This might take several minutes\. Do not proceed until you see **Welcome to My SAM Project\!**\.  
![\[The AWS SAM project is ready to use\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-created.png)

## Step 2: Explore Project Resources<a name="sam-tutorial-explore-project"></a>

In this step, you explore four of the project's AWS resources to understand how the project works\. 

+ The AWS CodeCommit repository where the project's source code is stored\. AWS CodeStar gives the repository the name **my\-sam\-project**, where **my\-sam\-project** is the name of the project\.

+ The AWS CodePipeline pipeline that uses AWS CodeBuild and AWS SAM to automate building and deploying the web service's Lambda function and API in API Gateway\. AWS CodeStar gives the pipeline the name **my\-sam\-project\-\-Pipeline**, where **my\-sam\-project** is the ID of the project\.

+ The Lambda function that contains the logic of the web service\. AWS CodeStar gives the function the name **awscodestar\-my\-sam\-project\-lambda\-HelloWorld\-*RANDOM\_ID***, where:

  + **my\-sam\-project** is the ID of the project\.

  + **HelloWorld** is the function ID as specified in the `template.yaml` file in the AWS CodeCommit repository\. You explore this file later\.

  + *RANDOM\_ID* is a random ID that AWS SAM assigns to the function to help ensure uniqueness\.

+ The API in API Gateway that makes it easier to call the Lambda function\. AWS CodeStar gives the API the name **awscodestar\-my\-sam\-project\-\-lambda**, where **my\-sam\-project** is the ID of the project\.

**To explore the source code repository in AWS CodeCommit**

1. With your project open in the AWS CodeStar console, on the side navigation bar, choose **Code**\.  
![\[The Code button on the AWS CodeStar side navigation bar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-side-nav.png)

1. In the AWS CodeCommit console, on the **Code** page, the source code files for the project are displayed\. 

   + `buildspec.yml`, which AWS CodePipeline instructs AWS CodeBuild to use during the build phase, to package the web service using AWS SAM\.

   + `index.py`, which contains the logic for the Lambda function\. This function simply outputs the string `Hello World`, along with a timestamp in ISO format\.

   + `README.md`, which contains general information about the repository\.

   + `template.yml`, which AWS SAM uses to package the web service and create the API in API Gateway\.  
![\[The project source code files in AWS CodeCommit\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cc.png)

   To view the contents of a file, simply choose it from the list\.

   For more information about using the AWS CodeCommit console, see the [AWS CodeCommit User Guide](http://docs.aws.amazon.com/codecommit/latest/userguide/)\.

**To explore the pipeline in AWS CodePipeline**

1. To view information about the pipeline, with your project open in the AWS CodeStar console, on the side navigation bar, choose **Dashboard**\. On the **Continuous deployment** tile, you see the pipeline contains:

   + A **Source** stage for getting the source code from AWS CodeCommit\.

   + A **Build** stage for building the source code with AWS CodeBuild\.

   + A **Deploy** stage for deploying the built source code and AWS resources with AWS SAM\.  
![\[The Continuous deployment tile in the AWS CodeStar dashboard\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cp.png)

1. To view detailed information about the pipeline, on the **Continuous deployment** tile, choose the **AWS CodePipeline details** link or, on the side navigation bar, choose **Pipeline** to open the pipeline in the AWS CodePipeline console\.

For information about using the AWS CodePipeline console, see the [AWS CodePipeline User Guide](http://docs.aws.amazon.com/codepipeline/latest/userguide/)\.

**To explore the function in Lambda**

1. With your project open in the AWS CodeStar console, on the side navigation bar, choose **Project**\.

1. In the **Project Resources** list, choose the link in the **ARN** column for the Lambda function\.  
![\[Opening the Lambda function from AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-lambda.png)

   The function's code is displayed in the Lambda console\. 

For information about using the Lambda console, see the [AWS Lambda Developer Guide](http://docs.aws.amazon.com/lambda/latest/dg/)\.

**To explore the API in API Gateway**

1. With your project open in the AWS CodeStar console, on the side navigation bar, choose **Project**\.

1. In the **Project Resources** list, choose the link in the **ARN** column for the Amazon API Gateway API\.

   Settings for the API are displayed in the API Gateway console\.

For information about using the API Gateway console, see the [API Gateway Developer Guide](http://docs.aws.amazon.com/apigateway/latest/developerguide/)\.

## Step 3: Test the Web Service<a name="sam-tutorial-test-service"></a>

In this step, you will test the web service that AWS CodeStar just built and deployed\.

1. With your project still open from the previous step, on the side navigation bar, choose **Dashboard**\.

1. On the **Continuous deployment** tile, make sure **Succeeded** is displayed for the **Source**, **Build**, and **Deploy** stages before you continue\. This might take several minutes\.   
![\[The AWS CodeStar dashboard showing that the pipeline has successfully run\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cp-ok.png)
**Note**  
If **Failed** is displayed for any of the stages, see the following for troubleshooting help:  
For the **Source** stage, see [Troubleshooting AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/troubleshooting.html) in the *AWS CodeCommit User Guide*\.
For the **Build** stage, see [Troubleshooting AWS CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/troubleshooting.html) in the *AWS CodeBuild User Guide*\.
For the **Deploy** stage, see [Troubleshooting AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) in the *AWS CloudFormation User Guide*\.
For other issues, see [Troubleshooting AWS CodeStar](troubleshooting.md)\.

1. Choose the link on the **Application endpoints** tile\. It should look like **https://*API\_ID*\.execute\-api\.*REGION\_ID*\.amazonaws\.com/Prod/**, where:

   + *API\_ID* represents the ID that API Gateway assigned to the API\.

   + *REGION\_ID* is the ID of the related AWS region\.

   + **Prod** is the name of the related API deployment stage in API Gateway\.

On the new tab that opens in your web browser,\.the web service displays the following response output:

```
{"output": "Hello World", "timestamp": "2017-08-30T15:53:42.682839"}
```

## Step 4: Set Up Your Local Workstation to Edit Project Code<a name="sam-tutorial-setup-workstation"></a>

In this step, you will set up your local workstation to edit the source code in the AWS CodeStar project\. Your local workstation can be a physical or virtual computer running macOS, Windows, or Linux\.

1. With your project still open from the previous step, do one of the following:

   + If **You must connect to your project's repository before you can start working on the code** is displayed, choose the **Connect Tools** button\.

   + In the side navigation pane, choose **Project**, and then choose the **Connect tools** button\.  
![\[The AWS CodeStar dashboard showing the Connect tools button\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-connect-banner.png)  
![\[The AWS CodeStar Project page showing the Connect tools button\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-connect-page.png)

1. Choose the **Command line tools** tile\.

   If you have Visual Studio or Eclipse installed, choose the **Visual Studio** or **Eclipse** tile instead, follow the instructions, and then skip to [Step 5: Add Logic to the Web Service](#sam-tutorial-add-logic)\.

1. On the **Connect to your tools** page, for **Operating System**, choose the operating system running on your local workstation\.

1. For **Connection Method**, choose **HTTPS**\.

   We recommend that you choose **HTTPS** instead of **SSH** because HTTPS has fewer setup tasks\. If you must use SSH, choose **SSH**, follow the instructions, and then skip to [Step 5: Add Logic to the Web Service](#sam-tutorial-add-logic)\.

1. Follow the instructions to complete the following tasks:

   1. Set up Git on your local workstation\.

   1. Use the IAM console to generate Git credentials for your IAM user\.

   1. Clone the project's AWS CodeCommit repository onto your local workstation\.

## Step 5: Add Logic to the Web Service<a name="sam-tutorial-add-logic"></a>

In this step, you use your local workstation to add logic to the web service\. Specifically, you add a Lambda function and then connect it to the API in API Gateway\.

1. On your local workstation, go to the directory that contains the cloned source code repository\.

1. In that directory, create a file named `hello.py`\. Add the following code to the file, and then save the file:

   ```
   import json
   
   def handler(event, context):
     data = {
       'output': 'Hello ' + event["pathParameters"]["name"]
     }
     return {'statusCode': 200,
       'body': json.dumps(data),
       'headers': {'Content-Type': 'application/json'}}
   ```

   The preceding code simply outputs the string `Hello` along with whatever string the caller sends to the function\.

1. In the same directory, open the `template.yml` file\. Add the following code to the end of the file, and then save the file: 

   ```
     Hello:
       Type: AWS::Serverless::Function
       Properties:
         Handler: hello.handler
         Runtime: python2.7
         Role:
           Fn::ImportValue:
             !Join ['-', [!Ref 'ProjectId', !Ref 'AWS::Region', 'LambdaTrustRole']]
         Events:
           GetEvent:
             Type: Api
             Properties:
               Path: /hello/{name}
               Method: get
   ```

   AWS SAM will use the preceding code to create a function in Lambda, add a new method and path to the API in API Gateway, and then connect this method and path to the new function\.
**Note**  
The indentation of the preceding code is important\. If you don't add the code exactly it's shown, the project might not build correctly\.

1. Use Git to add your file changes to the cloned repository's staging area by running the command `git add .` Do not forget the dot \(`.`\), which adds all changed files\.
**Note**  
If you are using Visual Studio or Eclipse instead of the command line, the instructions for using Git might be different\. Consult the Visual Studio or Eclipse documentation\.

1. Use Git to commit your staged files in the cloned repository by running the command `git commit -m "Added hello.py and updated template.yaml."`

1. Use Git to push your commit to the remote repository by running the `command git push`\.
**Note**  
You might be prompted for the user name and password IAM generated for you earlier\. To keep from being prompted each time you interact with the remote repository, consider installing and configuring a Git credential manager\. For example, on macOS or Linux you can run `git config credential.helper 'cache --timeout 900'` in the terminal to be prompted no sooner than every 15 minutes\. Or you can run `git config credential.helper 'store --file ~/.git-credentials'` to never be prompted again\. Git will store your credentials in clear text in a plain file in your home directory\. For more information, see [Git Tools \- Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage) on the Git website\.

After AWS CodeStar detects the push, it instructs AWS CodePipeline to use AWS CodeBuild and AWS SAM to rebuild and redeploy the web service automatically\. 

AWS SAM gives the new function the name **awscodestar\-my\-sam\-project\-lambda\-Hello\-*RANDOM\_ID***, where:

+ **my\-sam\-project** is the ID of the project\.

+ **Hello** is the function ID as specified in the `template.yaml` file\.

+ *RANDOM\_ID* is a random ID that AWS SAM assigns to the function for uniqueness\.

## Step 6: Test the Enhanced Web Service<a name="sam-tutorial-test-enhancements"></a>

In this step, you test the enhanced web service that AWS CodeStar built and deployed, based on the logic you added in the previous step\. 

1. With your project still open in the AWS CodeStar console, on the side navigation bar, choose **Dashboard**\.

1. On the **Continuous deployment** tile, make sure the pipeline has run again and that **Succeeded** is displayed for the **Source**, **Build**, and **Deploy** stages before you continue\. This might take several minutes\. 
**Note**  
If **Failed** is displayed for any of the stages, see the following for troubleshooting help:  
For the **Source** stage, see [Troubleshooting AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/troubleshooting.html) in the *AWS CodeCommit User Guide*\.
For the **Build** stage, see [Troubleshooting AWS CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/troubleshooting.html) in the *AWS CodeBuild User Guide*\.
For the **Deploy** stage, see [Troubleshooting AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) in the *AWS CloudFormation User Guide*\.
For other issues, see [Troubleshooting AWS CodeStar](troubleshooting.md)\.

1. Choose the link on the **Application endpoints** tile\. It should look like **https://*API\_ID*\.execute\-api\.*REGION\_ID*\.amazonaws\.com/Prod/**, where:

   + *API\_ID* represents the ID that API Gateway assigned to the API\.

   + *REGION\_ID* is the ID of the related AWS region\.

   + **Prod** is the name of the related API deployment stage in API Gateway\.

   On the new tab that opens in your web browser, the web service displays the following response output:

   ```
   {"output": "Hello World", "timestamp": "2017-08-30T15:53:42.682839"}
   ```

1. In the tab's address box, add the path **/hello/** and your first name to the end of the URL and then press `Enter`\. For example, **https://*API\_ID*\.execute\-api\.*REGION\_ID*\.amazonaws\.com/Prod/hello/*YOUR\_FIRST\_NAME***\.

If your first name is Mary, the web service displays the following response output:

```
{"output": "Hello Mary"}
```

## Step 7: Add a Unit Test to the Web Service<a name="sam-tutorial-add-unit-tests"></a>

In this step, you use your local workstation to add a test that AWS CodeStar runs automatically on the web service\. This test replaces the manual testing you did earlier\.

1. On your local workstation, go to the directory that contains the cloned source code repository\.

1. In that directory, create a file named `hello_test.py`\. Add the following code to the file, and then save the file:

   ```
   from hello import handler
   
   def test_hello_handler():
   
     event = {
       'pathParameters': {
         'name': 'testname'
       }
     }
   
     context = {}
   
     expected = {
       'body': '{"output": "Hello testname"}',
       'headers': {
         'Content-Type': 'application/json'
       },
       'statusCode': 200
     }
   
     assert handler(event, context) == expected
   ```

   This test checks whether the output of the Lambda function is in the expected format\. If so, the test succeeds\. Otherwise, the test fails\.

1. In the same directory, open the `buildspec.yml` file\. Replace the file's contents with the following code, and then save the file:

   ```
   version: 0.2
   
   phases:
   
     install:
       commands:
         - pip install pytest
   
     pre_build:
       commands:
         - pytest
   
     build:
       commands:
         - aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template template-export.json
   
   artifacts:
     type: zip
     files:
       - template-export.json
   ```

   This build specification instructs AWS CodeBuild to install pytest, the Python test framework, into its build environment\. AWS CodeBuild uses pytest to run the unit test\. The rest of the build specification is the same as before\.

1. Use Git to push these changes to the remote repository\.

   ```
   git add .
   
   git commit -m "Added hello_test.py and updated buildspec.yml."
   
   git push
   ```

## Step 8: View Unit Test Results<a name="sam-tutorial-view-unit-tests"></a>

In this step, you see whether the unit test succeeded or failed\.

1. With your project still open in the AWS CodeStar console, on the side navigation bar, choose **Dashboard**\.

1. On the **Continuous deployment** tile, make sure the pipeline has run again before you continue\. This might take several minutes\. 

   If the unit test was successful, **Succeeded** is displayed for the **Build** stage\. If the unit test failed, **Failed** is displayed\.

1. To view the unit test result details, on the **Continuous deployment** tile, choose the **CodeBuild** link in the **Build** stage\.  
![\[Connecting to AWS CodeBuild from AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cp-cb.png)

1. In the AWS CodeBuild console, on the **Build Project: my\-sam\-project** page, in **Build history**, choose the link in the **Build run** column of the table\.  
![\[Opening the build details page in AWS CodeBuild\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cb-run.png)

1. On the **my\-sam\-project:*BUILD\_ID*** page, in **Build logs**, choose the **View entire log** link\.  
![\[Opening the build details page in AWS CodeBuild\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cb-log.png)

1. In the Amazon CloudWatch Logs console, look in the log output for a test result similar to the following\. In the following test result, the test passed:

   ```
   ...
   ============================= test session starts ==============================
   platform linux2 -- Python 2.7.12, pytest-3.2.1, py-1.4.34, pluggy-0.4.0
   rootdir: /codebuild/output/src123456789/src, inifile:
   collected 1 item
   
   hello_test.py .
   
   =========================== 1 passed in 0.01 seconds ===========================
   ...
   ```

   If the test failed, there should be details in the log output to help you troubleshoot the failure\.

## Step 9: Clean Up<a name="sam-tutorial-clean-up"></a>

In this step, you clean up the project to avoid ongoing charges related to this project\.

If you want to keep using this project, you can skip this step, but your AWS account might continue to be charged\.

1. With your project still open in the AWS CodeStar console, on the side navigation bar, choose **Project**\.

1. Choose **Delete project**\.

1. Type the name of the project, keep the **Delete associated resources along with AWS CodeStar project** box selected, and then choose **Delete**\.
**Important**  
If you clear this box, the project record will be deleted from AWS CodeStar, but many of the project's AWS resources will remain, and your AWS account might be charged for ongoing related costs\.

An Amazon S3 bucket that AWS CodeStar created for this project might still remain\. To delete this remaining bucket:

1. Open the AWS CodeCommit console, at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, choose the icon next to **aws\-codestar\-*REGION\_ID*\-*ACCOUNT\_ID*\-my\-sam\-project\-\-pipe**, where:

   + *REGION\_ID* is the ID of the AWS region for the project you just deleted\.

   + *ACCOUNT\_ID* is your AWS account ID\.

   + **my\-sam\-project** is the ID of the project you just deleted\.

1. Choose **Empty Bucket**\. Type the name of the bucket, and then choose **Confirm**\.

1. Choose **Delete Bucket**\. Type the name of the bucket, and then choose **Confirm**\.

## Next Steps<a name="sam-tutorial-next-steps"></a>

Now that you have completed this tutorial, we suggest you review the following resources:

+ The [Getting Started with AWS CodeStar](getting-started.md) walkthrough uses a project that creates and deploys a Node\.js\-based web application running on an Amazon EC2 instance\.

+ [AWS CodeStar Project Templates](templates.md) describes other types of projects you can create\.

+ [Customize an AWS CodeStar Dashboard](how-to-customize.md) shows you how to customize your projects' dashboards, integrate with JIRA, and more\.

+ [Working with AWS CodeStar Teams](working-with-teams.md) shows you how others can help you work on your projects\.