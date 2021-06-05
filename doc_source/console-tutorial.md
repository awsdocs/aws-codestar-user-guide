# Tutorial: Create a Project with a GitHub Source Repository<a name="console-tutorial"></a>

 With AWS CodeStar, you can set up your repository to create, review, and merge pull requests with your project team\.

In this tutorial, you create a project with sample web application source code in a GitHub repository, a pipeline that deploys your changes, and EC2 instances where your application is hosted in the cloud\. After your project is created, this tutorial shows you how to create and merge a GitHub pull request that makes a change to your web application's home page\.

**Topics**
+ [Step 1: Create the project and create your GitHub repository](#console-tutorial-create)
+ [Step 2: View your source code](#console-tutorial-source)
+ [Step 3: Create a GitHub Pull Request](#console-tutorial-pullrequest)

## Step 1: Create the project and create your GitHub repository<a name="console-tutorial-create"></a>

In this step, use the console to create your project and create a connection to your new GitHub repository\. To access your GitHub repository, you create a connection resource that AWS CodeStar uses to manage authorization with GitHub\. When the project is created, its additional resources are provisioned for you\.

1. Sign in to the AWS Management Console, and then open the AWS CodeStar console at [https://console\.aws\.amazon\.com/codestar/](https://console.aws.amazon.com/codestar/)\.

1. Choose the AWS Region where you want to create the project and its resources\.

1. On the **AWS CodeStar** page, choose **Create project**\.

1. On the **Choose a project template** page, select the **Web application**, **Node\.js**, and **Amazon EC2** check boxes\. Then choose from the templates available for that set of options\.

   For more information, see [AWS CodeStar Project Templates](templates.md)\.

1. Choose **Next**\.

1. For **Project name**, enter a name for the project \(for example, **MyTeamProject**\)\. If you use a different name, be sure to use it throughout this tutorial\.

1. Under **Project repository**, choose **GitHub**\.

1. If you chose **GitHub**, you will need to choose or create a connection resource\. If you have an existing connection, choose it in the search field\. Otherwise, you will create a new connection here\. Choose **Connect to GitHub**\.

   The **Create a connection** page displays\.
**Note**  
To create a connection, you must have a GitHub account\. If you are creating a connection for an organization, you must be the organization owner\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/create-proj-github-connection-create.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

   1. Under **Create GitHub App connection**, in **Connection name**, enter a name for your connection\. Choose **Connect to GitHub**\.

      The **Connect to GitHub** page displays and shows the **GitHub Apps** field\.

   1. Under **GitHub Apps**, choose an app installation or choose **Install a new app** to create one\.
**Note**  
You install one app for all of your connections to a particular provider\. If you have already installed the AWS Connector for GitHub app, choose it and skip this step\.

   1. On the **Install AWS Connector for GitHub** page, choose the account where you want to install the app\.
**Note**  
If you previously installed the app, you can choose **Configure** to proceed to a modification page for your app installation, or you can use the back button to return to the console\.

   1. If the **Confirm password to continue** page is displayed, enter your GitHub password, and then choose **Sign in**\.

   1. On the **Install AWS Connector for GitHub** page, leave the defaults, and choose **Install**\.

   1. On the **Connect to GitHub** page, the installation ID for your new installation appears in **GitHub Apps**\.

      After the connection is successfully created, in the CodeStar create project page, the message **Ready to connect** displays\.
**Note**  
You can view your connection under Settings in the Developer Tools console\. For more information, see [Getting started with connections](https://docs.aws.amazon.com/dtconsole/latest/userguide/getting-started-connections.html)\.  
![\[Console screenshot showing the completed connection set up for a GitHub repository.\]](http://docs.aws.amazon.com/codestar/latest/userguide/images/create-proj-github-connections.png)![\[Console screenshot showing the completed connection set up for a GitHub repository.\]](http://docs.aws.amazon.com/codestar/latest/userguide/)![\[Console screenshot showing the completed connection set up for a GitHub repository.\]](http://docs.aws.amazon.com/codestar/latest/userguide/)

   1. For **Repository owner**, choose the GitHub organization or your personal GitHub account\.

   1. For **Repository name**, accept the default GitHub repository name, or enter a different one\.

   1. Choose **Public** or **Private**\.
**Note**  
If you want to use AWS Cloud9 as your development environment, you must choose a public repository\.

   1. \(Optional\) For **Repository description**, enter a description for the GitHub repository\.

1. Configure your Amazon EC2 instances in **Amazon EC2 Configuration** if your project is deployed to Amazon EC2 instances and you want to make changes\. For example, you can choose from available instance types for your project\. 

   In **Key pair**, choose the Amazon EC2 key pair you created in [Step 4: Create an Amazon EC2 Key Pair for AWS CodeStar Projects](setting-up.md#setting-up-create-ec2-key)\. Select **I acknowledge that I have access to the private key file**\.

1. Choose **Next**\.

1. Review the resources and configuration details\.

1. Choose **Next** or **Create project**\. \(The displayed choice depends on your project template\.\)

   Allow a few minutes while your project is created\.

1. After your project is created, choose the link under **Application endpoints** to view your web application\.

## Step 2: View your source code<a name="console-tutorial-source"></a>

In this step, you view your source code and the tools you can use for your source repository\.

1. In the navigation bar for your project, choose **Repository**\.

   To view a list of commits in GitHub, choose **View commits**\. This opens your commit history in GitHub\.

   To view issues, choose the **Issues** tab for your project\. To create a new issue in GitHub, choose **Create GitHub issue**\. This opens your repository issue form in GitHub\.

1. Under the **Repository** tab, choose the link under **Repository name**, and your project's repository opens in a new tab or window\. This repository contains the source code for your project\.

## Step 3: Create a GitHub Pull Request<a name="console-tutorial-pullrequest"></a>

In this step, you make a minor change to your source code and create a pull request\.

1. In GitHub, create a new feature branch in your repository\. Choose the main branch drop\-down field and enter a new branch in the field named `feature-branch`\. Choose **Create new branch**\. The branch is created and checked out for you\.

1. In GitHub, make a change in the `feature-branch` branch\. Open the public folder and open the `index.html` file\.

1. In the AWS CodeStar console, under **Pull requests**, to create a pull request in GitHub, choose **Create pull request**\. This opens your repository pull request form in GitHub\. In GitHub, choose the pencil icon to edit the file\.

   After `Congratulations!`, add the string `Well done, <name>!` and replace `<name>` with your name\. Choose **Commit changes**\. The change is committed to your feature branch\.

1. In the AWS CodeStar console, choose your project\. Choose the **Repository** tab\. Under Pull requests, choose **Create pull request**\.

   The form opens in GitHub\. Leave the main branch in the base branch\. For **Compare to**, choose your feature branch\. View the diff\. 

1. In GitHub, choose **Create pull request**\. A pull request named *Update index\.html* is created\.

1. In the AWS CodeStar console, view the new pull request\. Choose **Merge changes** to commit the changes to the repository and merge the pull request with the main branch of your repository\.

1. Return to the project in AWS CodeStar and check the **Pipeline** page\. You should now see the pipeline deploying\.

1. After your project is created, choose the link under **Application endpoints** to view your web application\.