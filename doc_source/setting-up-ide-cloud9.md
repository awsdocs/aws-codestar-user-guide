# Use AWS Cloud9 with AWS CodeStar<a name="setting-up-ide-cloud9"></a>

You can use AWS Cloud9 to make code changes and develop software in an AWS CodeStar project\. AWS Cloud9 is an online IDE, which you access through your web browser\. The IDE offers a rich code editing experience with support for several programming languages and runtime debuggers, as well as a built\-in terminal\. You can configure the IDE to your preferences\. These include switching color themes, binding shortcut keys, enabling programming language\-specific syntax coloring and code formatting, and more\. In the background, an Amazon EC2 instance hosts an AWS Cloud9 development environment\. This environment provides the AWS Cloud9 IDE and access to the AWS CodeStar project's code files\. For more information, see the *[AWS Cloud9 User Guide](http://docs.aws.amazon.com/cloud9/latest/user-guide/)*\.

You can use the AWS CodeStar console or AWS Cloud9 console to create AWS Cloud9 development environments for projects that store their code in AWS CodeCommit\. For AWS CodeStar projects that store their code in GitHub, you can only use the AWS Cloud9 console\. This topic describes how to use both consoles\.

**Topics**
+ [Create an AWS Cloud9 Environment for a Project](#setting-up-ide-cloud9-create)
+ [Open an AWS Cloud9 Environment for a Project](#setting-up-ide-cloud9-open)
+ [Share an AWS Cloud9 Environment with a Project Team Member](#setting-up-ide-cloud9-share)
+ [Delete an AWS Cloud9 Environment from a Project](#setting-up-ide-cloud9-delete)
+ [Use GitHub with AWS Cloud9](#setting-up-ide-cloud9-github)
+ [Additional Resources](#setting-up-ide-cloud9-more)

## Create an AWS Cloud9 Environment for a Project<a name="setting-up-ide-cloud9-create"></a>

You can create an AWS Cloud9 development environment for an existing or new project in AWS CodeStar, as follows: 

1. Do one of the following:
   + If you have an existing project, open the project in the AWS CodeStar console\. On the side navigation bar, choose **IDE**\. Choose **Create new environment**, and then skip ahead to step 2 in this procedure\.
**Important**  
If the project's source code is stored in GitHub, you won't see **IDE** on the side navigation bar\. However, you can use the AWS Cloud9 console to create a development environment, open the new environment, and then connect it to the existing project's GitHub repository\. To do this, skip the rest of this procedure and see [Use GitHub with AWS Cloud9](#setting-up-ide-cloud9-github)\.  
If the project is in an AWS Region where AWS Cloud9 isn't supported, you won't see **IDE** on the side navigation bar\. However, you can use the AWS Cloud9 console to create a development environment, open the new environment, and then connect it to the existing project's AWS CodeCommit repository\. To do this, skip the rest of this procedure and see [Creating an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html), [Opening an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/open-environment.html), and the [AWS CodeCommit Sample](http://docs.aws.amazon.com/cloud9/latest/user-guide/sample-codecommit.html) in the *AWS Cloud9 User Guide*\. See also the list of supported AWS Regions for [AWS Cloud9](http://docs.aws.amazon.com/general/latest/gr/rande.html#cloud9_region) in the *Amazon Web Services General Reference*\.
   + If you have not yet created a project, follow the steps in [Create a Project](how-to-create-project.md)\. When the create project wizard gets to the **Set up tools** page, for **Pick how you want to edit your code**, choose **Cloud9**\. Choose **Next**, and then skip ahead to step 2 in this procedure\.
**Important**  
If you choose to store the project's source code in GitHub, on the **Set up tools** page, you will see **Connect to your source repository** instead of **Pick how you want to edit your code**, and there are no options here to choose AWS Cloud9\. However, after AWS CodeStar creates the project, you can use the AWS Cloud9 console to create a development environment, open the new environment, and then connect it to the new project's GitHub repository\. To do this, skip the rest of this procedure and see [Use GitHub with AWS Cloud9](#setting-up-ide-cloud9-github)\.  
If the project is in an AWS Region where AWS Cloud9 is not supported, on the **Set up tools** page, you won't see any options to choose AWS Cloud9\. However, you can use the AWS Cloud9 console to create a development environment, open the new environment, and then connect it to the existing project's AWS CodeCommit repository\. To do this, skip the rest of this procedure and see [Creating an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html), [Opening an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/open-environment.html), and the [AWS CodeCommit Sample](http://docs.aws.amazon.com/cloud9/latest/user-guide/sample-codecommit.html) in the *AWS Cloud9 User Guide*\. See also the list of supported AWS Regions for [AWS Cloud9](http://docs.aws.amazon.com/general/latest/gr/rande.html#cloud9_region) in the *Amazon Web Services General Reference*\.

1. To change the default type of Amazon EC2 instance to host the environment, for **Pick an instance type for the IDE \(not your overall project\)**, choose the instance type\.

1. To change the default environment name, add a description for the environment, or both, expand **Environment name and description**, and then change the settings\.
**Note**  
Environment names must be unique per user\.

1.  AWS Cloud9 uses Amazon Virtual Private Cloud \(Amazon VPC\) in your AWS account to communicate with the instance\. Depending on how Amazon VPC is set up in your AWS account, do one of the following\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codestar/latest/userguide/setting-up-ide-cloud9.html)

   For more information, see [Amazon Virtual Private Cloud \(Amazon VPC\) Settings for an AWS Cloud9 EC2 Development Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/vpc-settings.html) in the *AWS Cloud9 User Guide*\.

1. To change the default time period when AWS Cloud9 shuts down the environment after it has not been used, expand **Cost\-saving options**, and then change the setting\.

1. Choose **Next**\.

To open the environment, see [Open an AWS Cloud9 Environment for a Project](#setting-up-ide-cloud9-open)\.

You can create more than one environment for a project by following the preceding steps\. For example, you might want to use one environment to work on one portion of the code, and use another environment to work on the same portion of the code with different settings—or you may want to work on another portion of the code altogether\.

## Open an AWS Cloud9 Environment for a Project<a name="setting-up-ide-cloud9-open"></a>

To open an existing AWS Cloud9 development environment that you created for a project in AWS CodeStar, do the following:

1. With the project open in the AWS CodeStar console, on the side navigation bar, choose **IDE**\.
**Important**  
If the project's source code is stored in GitHub, you won't see **IDE** on the side navigation bar\. However, you can use the AWS Cloud9 console to open an existing environment\. To do this, skip the rest of this procedure and see [Opening an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/open-environment.html) in the *AWS Cloud9 User Guide*\. See also [Use GitHub with AWS Cloud9](#setting-up-ide-cloud9-github)\.

1. For **My Cloud9 environments** or **Shared Cloud9 environments**, choose **Open IDE** for the environment you want to open\.

AWS Cloud9 opens the environment and displays the AWS Cloud9 IDE\. You can use the IDE to begin working with code in the project's AWS CodeCommit repository right away\. For more information, see [The Environment Window](http://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html#tutorial-environment), [The Editor, Tabs, and Panes](http://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html#tutorial-editor), and [The Terminal](http://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html#tutorial-terminal) in the *AWS Cloud9 User Guide*\. See also [Basic Git Commands](http://docs.aws.amazon.com/codecommit/latest/userguide/how-to-basic-git.html) in the *AWS CodeCommit User Guide*\.

## Share an AWS Cloud9 Environment with a Project Team Member<a name="setting-up-ide-cloud9-share"></a>

After you create an AWS Cloud9 development environment for a project in AWS CodeStar, you can invite other users across your AWS account—including project team members—to access that same environment\. This is especially useful for pair programming, where two programmers take turns coding and giving advice about the same code while sitting at the same workstation or through screen sharing\. Environment members can use the shared AWS Cloud9 IDE to see each member's code changes highlighted within the code editor, and to text chat with other members while coding\. 

Adding a team member to a project doesn't automatically allow that member to participate in any related AWS Cloud9 development environments for the project\. To invite a project team member to access an environment for a project, see [About Environment Member Access Roles](http://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html#share-environment-member-roles) and [Invite an IAM User to Your Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html#share-environment-invite-user) in the *AWS Cloud9 User Guide*\. When you invite a project team member to access an environment for a project, the AWS CodeStar console displays the environment to that team member\. The environment is displayed in the **Shared Cloud9 environments** list on the **IDE** tab in the AWS CodeStar console for the project\. To display this list, have the team member open the project in the console, and then choose **IDE** in the side navigation bar\.

**Important**  
If the project's source code is stored in GitHub, you won't see **IDE** on the side navigation bar\. However, you can use the AWS Cloud9 console to invite other users across your AWS account—including project team members—to access an environment\. To do this, see [Use GitHub with AWS Cloud9](#setting-up-ide-cloud9-github) in this guide, and see [About Environment Member Access Roles](http://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html#share-environment-member-roles) and [Invite an IAM User to Your Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html#share-environment-invite-user) in the *AWS Cloud9 User Guide*\.

You can also invite a user who is not a project team member to access an environment\. For example, you might want a user to work on a project's code but have no other access to that project\. To invite this type of user, see [About Environment Member Access Roles](http://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html#share-environment-member-roles) and [Invite an IAM User to Your Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html#share-environment-invite-user) in the *AWS Cloud9 User Guide*\. When you invite a user who is not a project team member to access an environment for a project, that user can use the AWS Cloud9 console to access the environment\. For more information, see [Open an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/open-environment.html) in the *AWS Cloud9 User Guide*\.

## Delete an AWS Cloud9 Environment from a Project<a name="setting-up-ide-cloud9-delete"></a>

When you delete a project from AWS CodeStar and you choose to also delete all related AWS resources for that project, all related AWS Cloud9 development environments that were created with the AWS CodeStar console are also deleted and cannot be recovered\. However, you can delete an existing development environment from a project without deleting the project itself, as follows:

1. With the project open in the AWS CodeStar console, choose **IDE** in the side navigation bar\.
**Important**  
If the project's source code is stored in GitHub, you won't see **IDE** on the side navigation bar\. However, you can use the AWS Cloud9 console to delete a development environment\. To do this, skip the rest of this procedure and see [Deleting an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/delete-environment.html) in the *AWS Cloud9 User Guide*\.

1. Inside of the tile for the environment you want to delete, choose the ellipses \(**…**\)\.

1. Type the development environment's name, and then choose **Delete**\.
**Warning**  
Deleting a development environment cannot be undone\. All uncommitted code changes in the environment will be lost\. 

## Use GitHub with AWS Cloud9<a name="setting-up-ide-cloud9-github"></a>

For AWS CodeStar projects that have their source code stored in GitHub, the AWS CodeStar console doesn't support working with AWS Cloud9 development environments directly\. However, you can use the AWS Cloud9 console to work with source code in GitHub repositories, as follows:

1. Use the AWS Cloud9 console to create an AWS Cloud9 development environment, if one doesn't already exist\. To do this, see [Creating an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html) in the *AWS Cloud9 User Guide*\.

1. Use the AWS Cloud9 console to open the development environment, if AWS Cloud9 doesn't open it automatically\. To do this, see [Opening an Environment](http://docs.aws.amazon.com/cloud9/latest/user-guide/open-environment.html) in the *AWS Cloud9 User Guide*\.

1. When you or AWS Cloud9 open the development environment, the AWS Cloud9 IDE is displayed\. In the IDE, use a terminal session to connect to the GitHub repository \(a process known as *cloning*\)\. If a terminal session isn't running, choose **Window, New Terminal** on the menu bar in the IDE\. For the commands to use to run in the terminal session to clone the GitHub repository, see [Cloning a Repository](https://help.github.com/articles/cloning-a-repository/#platform-linux) on the GitHub Help website\.
**Note**  
To navigate to the main page of the GitHub repository, with the related project open in the AWS CodeStar console, choose **Code** on the console's side navigation bar\.

1. Use the IDE's **Environment** window and editor tabs to view, change, and save code\. For more information, see [The Environment Window](http://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html#tutorial-environment) and [The Editor, Tabs, and Panes](http://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html#tutorial-editor) in the *AWS Cloud9 User Guide*\.

1. Use Git in the IDE's terminal session to push your code changes to the repository and periodically pull code changes from others from the repository\. For more information, see [Pushing to a Remote](https://help.github.com/articles/pushing-to-a-remote/) and [Fetching a remote](https://help.github.com/articles/fetching-a-remote/) on the GitHub Help website\. For additional Git commands, see [Git cheatsheet](https://help.github.com/articles/git-cheatsheet/) on the GitHub Help website\.
**Note**  
To keep Git from asking for your GitHub user name and password every time you push or pull code from the repository, you can use a *credential helper*\. For more information, see [Caching your GitHub password in Git](https://help.github.com/articles/caching-your-github-password-in-git/) on the GitHub Help website\.

## Additional Resources<a name="setting-up-ide-cloud9-more"></a>

For more information about using AWS Cloud9, see the following in the *AWS Cloud9 User Guide*:
+ [Tutorial](http://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial.html)
+ [Working with Environments](http://docs.aws.amazon.com/cloud9/latest/user-guide/environments.html)
+ [Working with the IDE](http://docs.aws.amazon.com/cloud9/latest/user-guide/ide.html)
+ [Samples](http://docs.aws.amazon.com/cloud9/latest/user-guide/samples.html)