# Logging AWS CodeStar API Calls with AWS CloudTrail<a name="cloudtrail"></a>

AWS CodeStar is integrated with CloudTrail, a service that captures API calls made by or on behalf of AWS CodeStar in your AWS account and delivers the log files to an Amazon S3 bucket you specify\. CloudTrail captures API calls from the AWS CodeStar console, the AWS CLI, the AWS SDKs, and the AWS CodeStar HTTP API\. Using the information collected by CloudTrail, you can determine which request was made to AWS CodeStar, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## AWS CodeStar Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, calls made to AWS CodeStar actions are tracked in log files\. AWS CodeStar records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All of the AWS CodeStar actions are logged\. These actions are documented in the [AWS CodeStar API Reference](http://docs.aws.amazon.com/codestar/latest/APIReference)\.

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the `userIdentity` field in the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, Amazon S3 server\-side encryption \(SSE\) is used to encrypt your log files\.

You can have CloudTrail publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate AWS CodeStar log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding AWS CodeStar Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public calls\.

The following example shows a CloudTrail log entry that demonstrates a `CreateProject` operation being called in AWS CodeStar:

```
{
  "eventVersion": "1.05",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAJLIN2OF3UBEXAMPLE:role-name",
    "arn": "arn:aws:sts::account-ID:assumed-role/role-name/role-session-name",
    "accountId": "account-ID",
    "accessKeyId": "ASIAJ44LFQS5XEXAMPLE",
    "sessionContext": {
      "attributes": {
        "mfaAuthenticated": "false",
        "creationDate": "2017-06-04T23:56:57Z"
      },
      "sessionIssuer": {
        "type": "Role",
        "principalId": "AROAJLIN2OF3UBEXAMPLE",
        "arn": "arn:aws:iam::account-ID:role/service-role/role-name",
        "accountId": "account-ID",
        "userName": "role-name"
      }
    },
    "invokedBy": "codestar.amazonaws.com"
  },
  "eventTime": "2017-06-04T23:56:57Z",
  "eventSource": "codestar.amazonaws.com",
  "eventName": "CreateProject",
  "awsRegion": "region-ID",
  "sourceIPAddress": "codestar.amazonaws.com",
  "userAgent": "codestar.amazonaws.com",
  "requestParameters": {
    "clientRequestToken": "arn:aws:cloudformation:region-ID:account-ID:stack/stack-name/additional-ID",
    "id": "project-ID",
    "stackId": "arn:aws:cloudformation:region-ID:account-ID:stack/stack-name/additional-ID",
    "description": "AWS CodeStar created project",
    "name": "project-name",
    "projectTemplateId": "arn:aws:codestar:region-ID::project-template/project-template-name"
  },
  "responseElements": {
    "projectTemplateId": "arn:aws:codestar:region-ID::project-template/project-template-name",
    "arn": "arn:aws:codestar:us-east-1:account-ID:project/project-ID",
    "clientRequestToken": "arn:aws:cloudformation:region-ID:account-ID:stack/stack-name/additional-ID",
    "id": "project-ID"
  },
  "requestID": "7d7556d0-4981-11e7-a3bc-dd5daEXAMPLE",
  "eventID": "6b0d6e28-7a1e-4a73-981b-c8fdbEXAMPLE",
  "eventType": "AwsApiCall",
  "recipientAccountId": "account-ID"
}
```