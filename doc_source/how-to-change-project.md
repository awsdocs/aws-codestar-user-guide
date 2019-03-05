# Change AWS Resources in an AWS CodeStar Project<a name="how-to-change-project"></a>

After you create a project in AWS CodeStar, you can change the default set of AWS resources that AWS CodeStar adds to the project\. 

## Supported Resource Changes<a name="how-to-change-project-supported-actions"></a>

The following table lists the supported changes to default AWS resources in an AWS CodeStar project\.


| Change | Notes | 
| --- | --- | 
| Add a stage to AWS CodePipeline\. | See [Add a Stage to AWS CodePipeline](how-to-change-project-codepipeline.md)\. | 
| Change Elastic Beanstalk environment settings\. | See [Change AWS Elastic Beanstalk Environment Settings](#how-to-change-project-beanstalk)\. | 
| Change an AWS Lambda function's code or settings, its IAM role, or its API in Amazon API Gateway\. | See [Change an AWS Lambda Function in Source Code](#how-to-change-project-lambda)\. | 
| Add a resource to an AWS Lambda project and expand permissions to create and access the new resource\. | See [Add a Resource to a Project](#customize-project-template)\. | 
| Add traffic shifting with CodeDeploy for an AWS Lambda function\. | See [Shift Traffic for an AWS Lambda Project](how-to-modify-serverless-project.md)\. | 
| Add AWS X\-Ray support | See [Enable Tracing for a Project](#customize-project-xray)\. | 
| Edit the buildspec\.yml file for your project to add a unit test build phase for AWS CodeBuild to run\. | See [Step 7: Add a Unit Test to the Web Service](sam-tutorial.md#sam-tutorial-add-unit-tests) in [ Tutorial: Creating and Managing a Serverless Project in AWS CodeStarServerless Project Tutorial  Demonstrates how to create and manage an AWS CodeStar project that uses the AWS Serverless Application Model \(AWS SAM\)\.   In this tutorial, you use AWS CodeStar to create a project that uses the AWS Serverless Application Model \(AWS SAM\) to create and manage AWS resources for a web service hosted in AWS Lambda\. AWS CodeStar uses AWS SAM, which relies on AWS CloudFormation, to provide a simplified way of creating and managing supported AWS resources, including Amazon API Gateway APIs, AWS Lambda functions, and Amazon DynamoDB tables\. \(This project does not use any Amazon DynamoDB tables\.\) For more information, see [AWS Serverless Application Model \(AWS SAM\)](https://github.com/awslabs/serverless-application-model) on GitHub\. **Prerequisite:** Complete the steps in [Setting Up AWS CodeStar](setting-up.md)\.  Your AWS account might be charged for costs related to this tutorial, including costs for AWS services used by AWS CodeStar\. For more information, see [AWS CodeStar Pricing](https://aws.amazon.com/codestar/pricing)\.    Overview  In this tutorial, you:   Use AWS CodeStar to create a project that uses AWS SAM to build and deploy a Python\-based web service\. This web service is hosted in AWS Lambda and can be accessed through Amazon API Gateway\.   Explore the project's main resources, which include:   The AWS CodeCommit repository where the project's source code is stored\. This source code includes the web service's logic and defines related AWS resources\.   The AWS CodePipeline pipeline that automates the building of the source code\. This pipeline uses AWS SAM to create and deploy a function to AWS Lambda, create a related API in Amazon API Gateway, and connect the API to the function\.   The function that is deployed to AWS Lambda\.   The API that is created in Amazon API Gateway\.     Test the web service to confirm that AWS CodeStar built and deployed the web service as expected\.   Set up your local workstation to work with the project's source code\.   Change the project's source code using your local workstation\. When you add a function to the project and then push your changes to the source code, AWS CodeStar rebuilds and redeploys the web service\.   Test the web service again to confirm that AWS CodeStar rebuilt and redeployed as expected\.   Write a unit test using your local workstation to replace some of your manual testing with an automated test\. When you push the unit test, AWS CodeStar rebuilds and redeploys the web service and runs the unit test\.   View the results of the unit tests\.   Clean up the project\. This step helps you avoid charges to your AWS account for costs related to this tutorial\.     Step 1: Create the Project  In this step, you use the AWS CodeStar console to create a project\.    Sign in to the AWS Management Console and open the AWS CodeStar console, at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.  You must sign in to the AWS Management Console using credentials associated with the IAM user you created or identified in [Setting Up AWS CodeStar](setting-up.md)\. This user must have the **`AWSCodeStarFullAccess`** managed policy attached\.    Choose the AWS Region where you want to create the project and its resources\.   For information about AWS Regions where AWS CodeStar is available, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codestar_region) in the *AWS General Reference*\.   Choose **Create a new project**\. If **Create a new project** is not displayed, choose **Start a new project**\.   On the **Choose a project template** page:    For **Application category**, select **Web service**\.   For **Programming languages**, select **Python**\.   For **AWS services**, select **AWS Lambda**\.     Choose the box that contains your selections\. 

![\[Choosing the AWS SAM project in AWS CodeStar\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-create.png)   For **Project name**, enter a name for the project \(for example, **My SAM Project**\)\. If you use a name different from the example, be sure to use it throughout this tutorial\. For **Project ID**, AWS CodeStar chooses a related identifier for this project \(for example, **my\-sam\-project**\)\. If you see a different project ID, be sure to use it throughout this tutorial\. Leave **AWS CodeCommit** selected, and do not change the **Repository name** value\.    Choose **Next**\.   Leave **AWS CodeStar would like permission to administer AWS resources on your behalf** selected, and then choose **Create Project**\. If this is your first time using AWS CodeStar in this AWS Region, for **Display Name** and **Email**, enter the display name and email address you want AWS CodeStar to use for your IAM user\. Choose **Next**\.   On **Choose how you want to edit your project code**, choose **Skip**\. You set up your local workstation to edit the project's code in a later step\.   Wait while AWS CodeStar creates the project\. This might take several minutes\. Do not continue until you see **Welcome to My SAM Project\!**\.     Step 2: Explore Project Resources  In this step, you explore four of the project's AWS resources to understand how the project works:    The AWS CodeCommit repository where the project's source code is stored\. AWS CodeStar gives the repository the name **my\-sam\-project**, where **my\-sam\-project** is the name of the project\.   The AWS CodePipeline pipeline that uses CodeBuild and AWS SAM to automate building and deploying the web service's Lambda function and API in API Gateway\. AWS CodeStar gives the pipeline the name **my\-sam\-project\-\-Pipeline**, where **my\-sam\-project** is the ID of the project\.   The Lambda function that contains the logic of the web service\. AWS CodeStar gives the function the name **awscodestar\-my\-sam\-project\-lambda\-HelloWorld\-*RANDOM\_ID***, where:   **my\-sam\-project** is the ID of the project\.   **HelloWorld** is the function ID as specified in the `template.yaml` file in the AWS CodeCommit repository\. You explore this file later\.   *RANDOM\_ID* is a random ID that AWS SAM assigns to the function to help ensure uniqueness\.     The API in API Gateway that makes it easier to call the Lambda function\. AWS CodeStar gives the API the name **awscodestar\-my\-sam\-project\-\-lambda**, where **my\-sam\-project** is the ID of the project\.   To explore the source code repository in CodeCommit  With your project open in the AWS CodeStar console, on the side navigation bar, choose **Code**\.   In the CodeCommit console, on the **Code** page, the source code files for the project are displayed:    `buildspec.yml`, which CodePipeline instructs CodeBuild to use during the build phase, to package the web service using AWS SAM\.   `index.py`, which contains the logic for the Lambda function\. This function simply outputs the string `Hello World` and a timestamp, in ISO format\.   `README.md`, which contains general information about the repository\.   `template.yml`, which AWS SAM uses to package the web service and create the API in API Gateway\.   

![\[The project source code files in CodeCommit\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-cc.png) To view the contents of a file, choose it from the list\. For more information about using the CodeCommit console, see the [AWS CodeCommit User Guide](https://docs.aws.amazon.com/codecommit/latest/userguide/)\.   To explore the pipeline in CodePipeline  To view information about the pipeline, with your project open in the AWS CodeStar console, on the side navigation bar, choose **Dashboard**\. On the **Continuous deployment** tile, you see the pipeline contains:   A **Source** stage for getting the source code from CodeCommit\.   A **Build** stage for building the source code with CodeBuild\.   A **Deploy** stage for deploying the built source code and AWS resources with AWS SAM\.     To view more information about the pipeline, on the **Continuous deployment** tile, choose the **CodePipeline details** link or, on the side navigation bar, choose **Pipeline** to open the pipeline in the CodePipeline console\.   For information about using the CodePipeline console, see the [AWS CodePipeline User Guide](https://docs.aws.amazon.com/codepipeline/latest/userguide/)\. To explore AWS service resources on the **Project** page  Open your project in the AWS CodeStar console and from the navigation bar, choose **Project**\.    Open the **Project Details **and **Project Resources** lists\.   To explore the function in Lambda  With your project open in the AWS CodeStar console, on the side navigation bar, choose **Project**\.   In **Project Resources**, in the **ARN** column, choose the link for the Lambda function\. The function's code is displayed in the Lambda console\.    For information about using the Lambda console, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\. To explore the API in API Gateway  With your project open in the AWS CodeStar console, on the side navigation bar, choose **Project**\.   In **Project Resources**, in the **ARN** column, choose the link for the Amazon API Gateway API\. Settings for the API are displayed in the API Gateway console\.   For information about using the API Gateway console, see the [API Gateway Developer Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/)\.   Step 3: Test the Web Service  In this step, you test the web service that AWS CodeStar just built and deployed\.   With your project still open from the previous step, on the side navigation bar, choose **Dashboard**\.   On the **Continuous deployment** tile, make sure **Succeeded** is displayed for the **Source**, **Build**, and **Deploy** stages before you continue\. This might take several minutes\.   If **Failed** is displayed for any of the stages, see the following for troubleshooting help:   For the **Source** stage, see [Troubleshooting AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/troubleshooting.html) in the *AWS CodeCommit User Guide*\.     For the **Build** stage, see [Troubleshooting AWS CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/troubleshooting.html) in the *AWS CodeBuild User Guide*\.     For the **Deploy** stage, see [Troubleshooting AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) in the *AWS CloudFormation User Guide*\.     For other issues, see [Troubleshooting AWS CodeStar](troubleshooting.md)\.      Choose the link on the **Application endpoints** tile\. It should look like **https://*API\_ID*\.execute\-api\.*REGION\_ID*\.amazonaws\.com/Prod/**, where:   *API\_ID* is the ID that API Gateway assigned to the API\.   *REGION\_ID* is the ID of the AWS Region\.   **Prod** is the name of the API deployment stage in API Gateway\.     On the new tab that opens in your web browser, the web service displays the following response output: 

```
{"output": "Hello World", "timestamp": "2017-08-30T15:53:42.682839"}
```   Step 4: Set Up Your Local Workstation to Edit Project Code  In this step, you set up your local workstation to edit the source code in the AWS CodeStar project\. Your local workstation can be a physical or virtual computer running macOS, Windows, or Linux\.   With your project still open from the previous step, do one of the following:   If **You must connect to your project's repository before you can start working on the code** is displayed, choose **Connect Tools**\.   In the side navigation pane, choose **Project**, and then choose **Connect tools**\.   

![\[The AWS CodeStar Project page showing the Connect tools button\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sam-project-connect-page.png)   Choose the **Command line tools** tile\. If you have Visual Studio or Eclipse installed, choose the **Visual Studio** or **Eclipse** tile instead, follow the instructions, and then skip to [Step 5: Add Logic to the Web Service](#sam-tutorial-add-logic)\.   On **Connect to your tools**, for **Operating System**, choose the operating system running on your local workstation\.   For **Connection Method**, choose **HTTPS**\. We recommend that you choose HTTPS instead of SSH because HTTPS has fewer setup tasks\. If you must use SSH, choose **SSH**, follow the instructions, and then skip to [Step 5: Add Logic to the Web Service](#sam-tutorial-add-logic)\.   Follow the instructions to complete the following tasks:   Set up Git on your local workstation\.   Use the IAM console to generate Git credentials for your IAM user\.   Clone the project's CodeCommit repository onto your local workstation\.       Step 5: Add Logic to the Web Service  In this step, you use your local workstation to add logic to the web service\. Specifically, you add a Lambda function and then connect it to the API in API Gateway\.   On your local workstation, go to the directory that contains the cloned source code repository\.   In that directory, create a file named `hello.py`\. Add the following code, and then save the file: 

   ```
   import json
   
   def handler(event, context):
     data = {
       'output': 'Hello ' + event["pathParameters"]["name"]
     }
     return {'statusCode': 200,
       'body': json.dumps(data),
       'headers': {'Content-Type': 'application/json'}}
   ``` The preceding code outputs the string `Hello` and string the caller sends to the function\.   In the same directory, open the `template.yml` file\. Add the following code to the end of the file, and then save the file:  

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
   ``` AWS SAM uses this code to create a function in Lambda, add a new method and path to the API in API Gateway, and then connect this method and path to the new function\.  The indentation of the preceding code is important\. If you don't add the code exactly as it's shown, the project might not build correctly\.    Run git add \. to add your file changes to the staging area of the cloned repository Do not forget the period \(\.\), which adds all changed files\.  If you are using Visual Studio or Eclipse instead of the command line, the instructions for using Git might be different\. See the Visual Studio or Eclipse documentation\.    Run git commit \-m "Added hello\.py and updated template\.yaml\." to commit your staged files in the cloned repository   Run git push to push your commit to the remote repository \.  You might be prompted for the user name and password IAM generated for you earlier\. To keep from being prompted each time you interact with the remote repository, consider installing and configuring a Git credential manager\. For example, on macOS or Linux, you can run git config credential\.helper 'cache \-\-timeout 900' in the terminal to be prompted no sooner than every 15 minutes\. Or you can run git config credential\.helper 'store \-\-file \~/\.git\-credentials' to never be prompted again\. Git stores your credentials in clear text in a plain file in your home directory\. For more information, see [Git Tools \- Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage) on the Git website\.    After AWS CodeStar detects the push, it instructs CodePipeline to use CodeBuild and AWS SAM to rebuild and redeploy the web service\.  AWS SAM gives the new function the name **awscodestar\-my\-sam\-project\-lambda\-Hello\-*RANDOM\_ID***, where:   **my\-sam\-project** is the ID of the project\.   **Hello** is the function ID, as specified in the `template.yaml` file\.   *RANDOM\_ID* is a random ID that AWS SAM assigns to the function for uniqueness\.     Step 6: Test the Enhanced Web Service  In this step, you test the enhanced web service that AWS CodeStar built and deployed, based on the logic you added in the previous step\.    With your project still open in the AWS CodeStar console, on the side navigation bar, choose **Dashboard**\.   On the **Continuous deployment** tile, make sure the pipeline has run again and that **Succeeded** is displayed for the **Source**, **Build**, and **Deploy** stages before you continue\. This might take several minutes\.   If **Failed** is displayed for any of the stages, see the following for troubleshooting help:   For the **Source** stage, see [Troubleshooting AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/troubleshooting.html) in the *AWS CodeCommit User Guide*\.     For the **Build** stage, see [Troubleshooting AWS CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/troubleshooting.html) in the *AWS CodeBuild User Guide*\.     For the **Deploy** stage, see [Troubleshooting AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) in the *AWS CloudFormation User Guide*\.     For other issues, see [Troubleshooting AWS CodeStar](troubleshooting.md)\.      Choose the link on the **Application endpoints** tile\. It should look like **https://*API\_ID*\.execute\-api\.*REGION\_ID*\.amazonaws\.com/Prod/**, where:   *API\_ID* is the ID that API Gateway assigned to the API\.   *REGION\_ID* is the ID of the AWS Region\.   **Prod** is the name of the API deployment stage in API Gateway\.   On the new tab that opens in your web browser, the web service displays the following response output: 

   ```
   {"output": "Hello World", "timestamp": "2017-08-30T15:53:42.682839"}
   ```   In the tab's address box, add the path **/hello/** and your first name to the end of the URL and then press **Enter** \(for example, **https://*API\_ID*\.execute\-api\.*REGION\_ID*\.amazonaws\.com/Prod/hello/*YOUR\_FIRST\_NAME***\)\.   If your first name is Mary, the web service displays the following response output: 

```
{"output": "Hello Mary"}
```   Step 7: Add a Unit Test to the Web Service  In this step, you use your local workstation to add a test that AWS CodeStar runs on the web service\. This test replaces the manual testing you did earlier\.   On your local workstation, go to the directory that contains the cloned source code repository\.   In that directory, create a file named `hello_test.py`\. Add the following code, and then save the file\. 

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
   ``` This test checks whether the output of the Lambda function is in the expected format\. If so, the test succeeds\. Otherwise, the test fails\.   In the same directory, open the `buildspec.yml` file\. Replace the file's contents with the following code, and then save the file\. 

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
         - aws cloudformation package --template template.yml --s3-bucket $S3_BUCKET --output-template template-export.yml
   
   artifacts:
     type: zip
     files:
       - template-export.yml
   ``` This build specification instructs CodeBuild to install pytest, the Python test framework, into its build environment\. CodeBuild uses pytest to run the unit test\. The rest of the build specification is the same as before\.   Use Git to push these changes to the remote repository\. 

   ```
   git add .
   
   git commit -m "Added hello_test.py and updated buildspec.yml."
   
   git push
   ```     Step 8: View Unit Test Results  In this step, you see whether the unit test succeeded or failed\.   With your project still open in the AWS CodeStar console, on the side navigation bar, choose **Dashboard**\.   On the **Continuous deployment** tile, make sure the pipeline has run again before you continue\. This might take several minutes\.  If the unit test was successful, **Succeeded** is displayed for the **Build** stage\.    To view the unit test result details, on the **Continuous deployment** tile, in the **Build** stage, choose the **CodeBuild** link\.   In the CodeBuild console, on the **Build Project: my\-sam\-project** page, in **Build history**, choose the link in the **Build run** column of the table\.   On the **my\-sam\-project:*BUILD\_ID*** page, in **Build logs**, choose the **View entire log** link\.   In the Amazon CloudWatch Logs console, look in the log output for a test result similar to the following\. In the following test result, the test passed: 

   ```
   ...
   ============================= test session starts ==============================
   platform linux2 -- Python 2.7.12, pytest-3.2.1, py-1.4.34, pluggy-0.4.0
   rootdir: /codebuild/output/src123456789/src, inifile:
   collected 1 item
   
   hello_test.py .
   
   =========================== 1 passed in 0.01 seconds ===========================
   ...
   ``` If the test failed, there should be details in the log output to help you troubleshoot the failure\.     Step 9: Clean Up  In this step, you clean up the project to avoid ongoing charges for this project\. If you want to keep using this project, you can skip this step, but your AWS account might continue to be charged\.   With your project still open in the AWS CodeStar console, on the side navigation bar, choose **Project**\.   Choose **Delete project**\.   Enter the name of the project, keep the **Delete associated resources along with AWS CodeStar project** box selected, and then choose **Delete**\.  If you clear this box, the project record is deleted from AWS CodeStar, but many of the project's AWS resources are retained\. Your AWS account might continue to be charged\.    If there is still an Amazon S3 bucket that AWS CodeStar created for this project, follow these steps to delete it\. :   Open the CodeCommit console, at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.   In the list of buckets, choose the icon next to **aws\-codestar\-*REGION\_ID*\-*ACCOUNT\_ID*\-my\-sam\-project\-\-pipe**, where:   *REGION\_ID* is the ID of the AWS Region for the project you just deleted\.   *ACCOUNT\_ID* is your AWS account ID\.   **my\-sam\-project** is the ID of the project you just deleted\.     Choose **Empty Bucket**\. Enter the name of the bucket, and then choose **Confirm**\.   Choose **Delete Bucket**\. Enter the name of the bucket, and then choose **Confirm**\.     Next Steps  Now that you have completed this tutorial, we suggest you review the following resources:   The [Getting Started with AWS CodeStar](getting-started.md) tutorial uses a project that creates and deploys a Node\.js\-based web application running on an Amazon EC2 instance\.   [AWS CodeStar Project Templates](templates.md) describes other types of projects you can create\.   [Customize an AWS CodeStar Dashboard](how-to-customize.md) shows you how to customize your projects' dashboards, integrate with JIRA, and more\.   [Working with AWS CodeStar Teams](working-with-teams.md) shows you how others can help you work on your projects\.    ](sam-tutorial.md)\. | 
| Add your own IAM role to your project\. | See [Add an IAM Role to a Project](#add-iam-role) | 
| Change an IAM role definition\. | For roles defined in the application stack\. You cannot change roles defined in the toolchain or AWS CloudFormation stacks\. | 
| Modify your Lambda project to add an endpoint\. |  | 
| Modify your EC2 project to add an endpoint\. |  | 
| Modify your Elastic Beanstalk project to add an endpoint\. |  | 
| Edit your project to add a Prod stage and endpoint\. | See [Add a Prod Stage and Endpoint to a Project](#customize-ec2-multi-endpoints)\. | 
| Securely use SSM parameters in an AWS CodeStar project\. | See [Securely Use SSM Parameters in an AWS CodeStar Project](ssm-parameters.md)\. | 

The following changes are not supported\.
+ Switch to a different deployment target \(for example, deploy to AWS Elastic Beanstalk instead of AWS CodeDeploy\)\.
+ Add a friendly web endpoint name\.
+ Change the CodeCommit repository name \(for an AWS CodeStar project connected to CodeCommit\)\.
+ For an AWS CodeStar project connected to GitHub, disconnect the GitHub repository, and then reconnect the repository to that project, or connect any other repository to that project\. You can use the CodePipeline console \(not the AWS CodeStar console\) to disconnect and reconnect to GitHub in a pipeline's **Source** stage\. However, if you reconnect the **Source** stage to a different GitHub repository, in the AWS CodeStar dashboard for the project, the information in the **Application endpoints**, **Commit history**, and **GitHub Issues** tiles might be wrong or out of date\. Disconnecting the GitHub repository does not remove that repository's information from the commit history and GitHub issues tiles in the AWS CodeStar project dashboard\. To remove this information, use the GitHub website to disable access to GitHub from the AWS CodeStar project\. To revoke access, on the GitHub website, use the **Authorized OAuth Apps** section of the settings page for your GitHub account profile\.
+ Disconnect the CodeCommit repository \(for an AWS CodeStar project connected to CodeCommit\) and then reconnect the repository to that project, or connect any other repository to that project\.

## Change AWS Elastic Beanstalk Environment Settings<a name="how-to-change-project-beanstalk"></a>

You can change the settings of an Elastic Beanstalk environment that AWS CodeStar creates in a project\. For more information, see [AWS Elastic Beanstalk Environment Management Console](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-console.html) in the *AWS Elastic Beanstalk Developer Guide*\.

## Change an AWS Lambda Function in Source Code<a name="how-to-change-project-lambda"></a>

You can change the code or settings of a Lambda function, or its IAM role or API Gateway API, that AWS CodeStar creates in a project\. To do this, we recommend that you use the AWS Serverless Application Model \(AWS SAM\) along with the `template.yaml` file in your project's CodeCommit repository\. This `template.yaml` file defines your function's name, handler, runtime, IAM role, and API in API Gateway\. For more information, see [How to Create Serverless Applications Using AWS SAM](https://github.com/awslabs/serverless-application-model/blob/master/HOWTO.md) on the GitHub website\.

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

1. The role opens in the IAM console\. Choose **Attach policies**\. Search for the **AWSXrayWriteOnlyAccess** policy, select the box next to it, and then choose **Attach Policy**\.

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

1.  Verify that tracing is enabled by viewing the details for your function in the Lambda console\.

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

1. The role opens in the IAM console\. On the **Permissions** tab, in **Inline Policies**, expand the row for your service role policy, and choose **Edit Policy**\.

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

   Choose the **CloudFormation** link in your pipeline's **Deploy** stage to troubleshoot the failure in the AWS CloudFormation console\. In the console, in the **Events** list, choose your project to view stack creation details\. There is a message with the failure details\. In this example, the `sqs:CreateQueue` permission is missing\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/acs-sqs-pipeline-deployfailed-stack.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

   Add any missing permissions by editing the policy attached to your project's AWS CloudFormation worker role\. See [Step 1: Edit the CloudFormation Worker Role in IAM](#customize-project-template-sqs-2)\.

1. After a successful run of your pipeline, the resources are created in your AWS CloudFormation stack\. In the **Resources** list in AWS CloudFormation, view the resource created for your project\. In this example, the TestQueue queue is listed in the **Resources ** section\.

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
+ [Step 1: Create an Application and Deployment Group in CodeDeploy \(Amazon EC2 Projects Only\)](customize-ec2-multi-endpoints-newdeployment.md)
+ [Step 2: Add a New Pipeline Stage for the Prod Stage](customize-ec2-multi-endpoints-newstage.md)
+ [Step 3: Add a Manual Approval Stage](customize-ec2-multi-endpoints-approval.md)
+ [Step 4: Push a Change and Monitor the AWS CloudFormation Stack Update](customize-ec2-multi-endpoints-stack.md)