# Security in AWS CodeStar<a name="security"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from a data center and network architecture that is built to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify the effectiveness of our security as part of the [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/) \. To learn about the compliance programs that apply to AWS CodeStar, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

This documentation helps you understand how to apply the shared responsibility model when using AWS CodeStar\. The following topics show you how to configure AWS CodeStar to meet your security and compliance objectives\. You also learn how to use other AWS services that help you to monitor and secure your AWS CodeStar resources\. 

When you create custom policies and use permission boundaries in AWS CodeStar, ensure least\-privilege access by granting only the permissions required to perform a task and scoping down the permissions to targeted resources\. To prevent members of other projects from accessing resources in your project, grant organization members separate permissions for each AWS CodeStar project\. As a best practice, create a project account for each member and then assign role\-based access to that account\.

For example, you can use a service such as AWS Control Tower with AWS Organizations to provision accounts for each developer role under a DevOps group\. Then you can assign permissions to those accounts\. The overall permissions apply to the account but the user has limited access to resources outside the project\.

For more information about managing least\-privilege access to AWS resources using a *multi\-account strategy*, refer to [AWS multi\-account strategy for your landing zone](https://docs.aws.amazon.com/controltower/latest/userguide/aws-multi-account-landing-zone.html#guidelines-for-multi-account-setup) in the *AWS Control Tower User Guide*\.

 

**Topics**
+ [Data Protection in AWS CodeStar](data-protection.md)
+ [Identity and Access Management for AWS CodeStar](security-iam.md)
+ [Logging AWS CodeStar API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Compliance Validation for AWS CodeStar](codestar-compliance.md)
+ [Resilience in AWS CodeStar](disaster-recovery-resiliency.md)
+ [Infrastructure Security in AWS CodeStar](infrastructure-security.md)