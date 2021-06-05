# Security in AWS CodeStar<a name="security"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from a data center and network architecture that is built to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify the effectiveness of our security as part of the [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/) \. To learn about the compliance programs that apply to AWS CodeStar, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

This documentation helps you understand how to apply the shared responsibility model when using AWS CodeStar\. The following topics show you how to configure AWS CodeStar to meet your security and compliance objectives\. You also learn how to use other AWS services that help you to monitor and secure your AWS CodeStar resources\. 

When you manage AWS CodeStar projects in your organization, use multiple accounts so that organization members have separate permissions for each project where they are a project member\. As a best practice to isolate resources for AWS CodeStar projects, create an account for each project member and assign role\-based access to that account\. 

Under your main organization accounts, set up accounts that map to your directory listings or align with your project members\. For example, use a service such as AWS Control Tower with AWS Organizations to provision accounts for each developer role under a DevOps group, and then you can assign custom permission sets to those accounts\. The overall permissions are applied to the account while limiting access to all resources outside the project\. For more information about managing least\-privilege access to AWS resources using a *multi\-account strategy*, refer to [AWS multi\-account strategy for your landing zone](https://docs.aws.amazon.com/controltower/latest/userguide/aws-multi-account-landing-zone.html#guidelines-for-multi-account-setup) in the *AWS Control Tower User Guide*\.

 

**Topics**
+ [Data Protection in AWS CodeStar](data-protection.md)
+ [Identity and Access Management for AWS CodeStar](security-iam.md)
+ [Logging AWS CodeStar API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Compliance Validation for AWS CodeStar](codestar-compliance.md)
+ [Resilience in AWS CodeStar](disaster-recovery-resiliency.md)
+ [Infrastructure Security in AWS CodeStar](infrastructure-security.md)