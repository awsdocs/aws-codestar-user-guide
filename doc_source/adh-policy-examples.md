# AWS CodeStar Policy Examples<a name="adh-policy-examples"></a>

AWS CodeStar creates several policies on your behalf for creating and managing project resources\. If you want to customize your CodeStar project, it can help to understand the contents of typical policies\.

**Topics**
+ [AWS Managed Policies](#adh-managed-policies)
+ [Customer Managed Policies](#adh-customer-policies)
+ [Inline Policies](#adh-inline-policies)

## AWS Managed Policies<a name="adh-managed-policies"></a>

AWS provides two managed \(predefined\) policies for AWS CodeStar, the service role and the `AWSCodeStarFullAccess`\. Creating a CodeStar project for the first time triggers the creation of the CodeStar service role policy\. The `AWSCodeStarFullAccess` policy must be attached to your IAM user before you can create your first CodeStar project\.

### AWSCodeStarFullAccess<a name="adh-policy-fullaccess"></a>

 The `AWSCodeStarFullAccess` policy allows full access to AWS CodeStar\. For more information about this policy, see [AWS CodeStar Access Permissions Reference](access-permissions.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CodeStarEC2",
      "Effect": "Allow",
      "Action": [
        "codestar:*",
        "ec2:DescribeKeyPairs",
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodeStarCF",
      "Effect": "Allow",
      "Action": [
        "cloudformation:DescribeStack*",
        "cloudformation:GetTemplateSummary"
      ],
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/awscodestar-*"
      ]
    }
  ]
}
```

### AWS CodeStar Service Role Policy<a name="adh-policy-service-role"></a>

The `aws-codestar-service-role` policy is attached to the service role that allows AWS CodeStar to perform actions with other services\. For more information, see [AWS CodeStar Service Role Policy and Permissions](access-permissions-service-role.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ProjectStack",
      "Effect": "Allow",
      "Action": [
        "cloudformation:*Stack*",
        "cloudformation:*ChangeSet*",
        "cloudformation:GetTemplate"
      ],
      "Resource": [
        "arn:aws:cloudformation:*:*:stack/awscodestar-*",
        "arn:aws:cloudformation:*:*:stack/awseb-*",
        "arn:aws:cloudformation:*:*:stack/aws-cloud9-*",
        "arn:aws:cloudformation:*:aws:transform/CodeStar*"
      ]
    },
    {
      "Sid": "ProjectStackTemplate",
      "Effect": "Allow",
      "Action": [
        "cloudformation:GetTemplateSummary",
        "cloudformation:DescribeChangeSet"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ProjectQuickstarts",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::awscodestar-*/*"
      ]
    },
    {
      "Sid": "ProjectS3Buckets",
      "Effect": "Allow",
      "Action": [
        "s3:*"
      ],
      "Resource": [
        "arn:aws:s3:::aws-codestar-*",
        "arn:aws:s3:::aws-codestar-*/*",
        "arn:aws:s3:::elasticbeanstalk-*",
        "arn:aws:s3:::elasticbeanstalk-*/*"
      ]
    },
    {
      "Sid": "ProjectServices",
      "Effect": "Allow",
      "Action": [
        "codestar:*Project",
        "codestar:*Resource*",
        "codestar:List*",
        "codestar:Describe*",
        "codestar:Get*",
        "codestar:AssociateTeamMember",
        "codecommit:*",
        "codepipeline:*",
        "codedeploy:*",
        "codebuild:*",
        "ec2:RunInstances",
        "autoscaling:*",
        "cloudwatch:Put*",
        "ec2:*",
        "elasticbeanstalk:*",
        "elasticloadbalancing:*",
        "iam:ListRoles",
        "logs:*",
        "sns:*",
        "cloud9:CreateEnvironmentEC2",
        "cloud9:DeleteEnvironmentEC2",
        "cloud9:DescribeEnvironment*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ProjectWorkerRoles",
      "Effect": "Allow",
      "Action": [
        "iam:AttachRolePolicy",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:GetRole",
        "iam:PassRole",
        "iam:PutRolePolicy",
        "iam:SetDefaultPolicyVersion",
        "iam:CreatePolicy",
        "iam:DeletePolicy",
        "iam:AddRoleToInstanceProfile",
        "iam:CreateInstanceProfile",
        "iam:DeleteInstanceProfile",
        "iam:RemoveRoleFromInstanceProfile"
      ],
      "Resource": [
        "arn:aws:iam::*:role/CodeStarWorker*",
        "arn:aws:iam::*:policy/CodeStarWorker*",
        "arn:aws:iam::*:instance-profile/awscodestar-*"
      ]
    },
    {
      "Sid": "ProjectTeamMembers",
      "Effect": "Allow",
      "Action": [
        "iam:AttachUserPolicy",
        "iam:DetachUserPolicy"
      ],
      "Resource": "*",
      "Condition": {
        "ArnEquals": {
          "iam:PolicyArn": [
            "arn:aws:iam::*:policy/CodeStar_*"
          ]
        }
      }
    },
    {
      "Sid": "ProjectRoles",
      "Effect": "Allow",
      "Action": [
        "iam:CreatePolicy",
        "iam:DeletePolicy",
        "iam:CreatePolicyVersion",
        "iam:DeletePolicyVersion",
        "iam:ListEntitiesForPolicy",
        "iam:ListPolicyVersions"
      ],
      "Resource": [
        "arn:aws:iam::*:policy/CodeStar_*"
      ]
    },
    {
      "Sid": "InspectServiceRole",
      "Effect": "Allow",
      "Action": [
        "iam:ListAttachedRolePolicies"
      ],
      "Resource": [
        "arn:aws:iam::*:role/aws-codestar-service-role"
      ]
    },
    {    
      "Sid": "IAMLinkRole",
      "Effect": "Allow",
      "Condition": {
        "StringLike": {
          "iam:AWSServiceName": "cloud9.amazonaws.com"
        }
      },
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": "arn:aws:iam::*:role/aws-service-role/cloud9.amazonaws.com/AWSServiceRoleForCloud9*"
    }
  ]
}
```

## Customer Managed Policies<a name="adh-customer-policies"></a>

AWS CodeStar creates several customer managed IAM policies on your behalf when you create a project\. These policies manage access levels for project team members\. CodeStar creates a permissions boundary policy as well\.

### AWS CodeStar Owner Role Policy<a name="adh-policy-owner"></a>

CodeStar applies the `CodeStar_project-id_Owner` policy to project team members with the owner access level\. For more information, see [AWS CodeStar Owner Role Policy ](access-permissions-proj.md#access-permissions-proj-owner)\.

```
...
{
  "Effect": "Allow",
  "Action": [
    ...
    "codestar:*",
    ...
  ],
  "Resource": [
    "arn:aws:codestar:us-east-2:111111111111:project/project-id",
    "arn:aws:iam::account-id:policy/CodeStar_project-id_Owner"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:DescribeUserProfile",
    "codestar:ListProjects",
    "codestar:ListUserProfiles",
    "codestar:VerifyServiceRole",
    ...
  ],
  "Resource": [
    "*"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:*UserProfile",
    ...
  ],
  "Resource": [
    "arn:aws:iam::account-id:user/user-name"
  ]
}
...
```

### AWS CodeStar Contributor Role Policy<a name="adh-policy-contributor"></a>

CodeStar applies the `CodeStar_project-id_Contributor` policy to project team members with the contributor access level\. Users with contributor access can contribute to the project and change the project dashboard, but cannot add or remove team members\. For more information, see [AWS CodeStar Contributor Role Policy ](access-permissions-proj.md#access-permissions-proj-contributor)\.

```
...
{
  "Effect": "Allow",
  "Action": [
    ...
    "codestar:Describe*",
    "codestar:Get*",
    "codestar:List*",
    "codestar:PutExtendedAccess",
    ...
  ],
  "Resource": [
    "arn:aws:codestar:us-east-2:111111111111:project/project-id",
    "arn:aws:iam::account-id:policy/CodeStar_project-id_Contributor"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:DescribeUserProfile",
    "codestar:ListProjects",
    "codestar:ListUserProfiles",
    "codestar:VerifyServiceRole",
    ...
  ],
  "Resource": [
    "*"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:*UserProfile",
    ...
  ],
  "Resource": [
    "arn:aws:iam::account-id:user/user-name"
  ]
}
...
```

### AWS CodeStar Viewer Role Policy<a name="adh-policy-viewer"></a>

CodeStar applies the `CodeStar_project-id_Viewer` policy to project team members with the viewer access level\. Users with viewer access can view a project in CodeStar, but not change its resources or add or remove team members\. For more information, see [AWS CodeStar Viewer Role Policy ](access-permissions-proj.md#access-permissions-proj-viewer)\.

```
...
{
  "Effect": "Allow",
  "Action": [
    ...
    "codestar:Describe*",
    "codestar:Get*",
    "codestar:List*",
    ...
  ],
  "Resource": [
    "arn:aws:codestar:us-east-2:111111111111:project/project-id",
    "arn:aws:iam::account-id:policy/CodeStar_project-id_Viewer"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:DescribeUserProfile",
    "codestar:ListProjects",
    "codestar:ListUserProfiles",
    "codestar:VerifyServiceRole",
    ...
  ],
  "Resource": [
    "*"
  ]
},
{
  "Effect": "Allow",
  "Action": [
    "codestar:*UserProfile",
    ...
  ],
  "Resource": [
    "arn:aws:iam::account-id:user/user-name"
  ]
}
...
```

### AWS CodeStar Permissions Boundary Policy<a name="permissions-boundary-policy"></a>

If you create a CodeStar project after December 6, 2018 PDT, CodeStar creates a permissions boundary policy for your project\. This policy prevents the escalation of privileges to resources outside the project\. It is a dynamic policy that updates as the project evolves\. The exact contents of the policy depend on the type of project you are creating\. The following policy is an example\. For more information, see [IAM Permissions Boundary](access-permissions-proj.md#access-permissions-proj-pb-worker)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::*/AWSLogs/*/Config/*"
      ]
    },
    {
      "Sid": "2",
      "Effect": "Allow",
      "Action": [
        "*"
      ],
      "Resource": [
        "arn:aws:codestar:us-east-1:account-id:project/project-id",
        "arn:aws:cloudformation:us-east-1:account-id:stack/awscodestar-project-id-lambda/eefbbf20-c1d9-11e8-8a3a-500c28b4e461",
        "arn:aws:cloudformation:us-east-1:account-id:stack/awscodestar-project-id/4b80b3f0-c1d9-11e8-8517-500c28b236fd",
        "arn:aws:codebuild:us-east-1:account-id:project/project-id",
        "arn:aws:codecommit:us-east-1:account-id:project-id",
        "arn:aws:codepipeline:us-east-1:account-id:project-id-Pipeline",
        "arn:aws:execute-api:us-east-1:account-id:7rlst5mrgi",
        "arn:aws:iam::account-id:role/CodeStarWorker-project-id-CloudFormation",
        "arn:aws:iam::account-id:role/CodeStarWorker-project-id-CloudWatchEventRule",
        "arn:aws:iam::account-id:role/CodeStarWorker-project-id-CodeBuild",
        "arn:aws:iam::account-id:role/CodeStarWorker-project-id-CodePipeline",
        "arn:aws:iam::account-id:role/CodeStarWorker-project-id-Lambda",
        "arn:aws:lambda:us-east-1:account-id:function:awscodestar-project-id-lambda-GetHelloWorld-KFKTXYNH9573",
        "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-app",
        "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe"
      ]
    },
    {
      "Sid": "3",
      "Effect": "Allow",
      "Action": [
        "apigateway:GET",
        "config:Describe*",
        "config:Get*",
        "config:List*",
        "config:Put*",
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:DescribeLogGroups",
        "logs:PutLogEvents"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

## Inline Policies<a name="adh-inline-policies"></a>

 AWS CodeStar creates several inline IAM policies when you create a project\. These policies are applied to the worker roles that use other AWS services to create project resources\.

### AWS CloudFormation Worker Role Policy<a name="adh-policy-cfn-worker-new"></a>

For AWS CodeStar projects created after December 6, 2018 PDT, CodeStar creates an inline policy for a worker role that creates AWS CloudFormation resources for your CodeStar project\. The exact contents of the policy depend on the type of resources needed for your project\. The following policy is an example\. For more information, see [IAM Policies for Worker Roles](access-permissions-proj.md#access-permissions-proj-worker)\.

```
{
{
    "Statement": [
        {
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe",
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "acm:*",
                "apigateway:*",
                "application-autoscaling:*",
                "appsync:*",
                "athena:*",
                "autoscaling:*",
                "autoscaling-plans:*",
                "batch:*",
                "cloudfront:*",
                "cloudtrail:*",
                "cloudwatch:*",
                "codedeploy:*",
                "codestar:*",
                "cognito-idp:*",
                "cognito-identity:*",
                "cognito-sync:*",
                "config:*",
                "datapipeline:*",
                "dax:*",
                "discovery:*",
                "dms:*",
                "ds:*",
                "dynamodb:*",
                "ec2:*",
                "ecr:*",
                "ecs:*",
                "eks:*",
                "elasticache:*",
                "elasticbeanstalk:*",
                "elasticfilesystem:*",
                "elasticloadbalancing:*",
                "elasticmapreduce:*",
                "es:*",
                "events:*",
                "firehose:*",
                "gamelift:*",
                "glue:*",
                "guardduty:*",
                "inspector:*",
                "iot:*",
                "kinesis:*",
                "kinesisanalytics:*",
                "lambda:*",
                "logs:*",
                "mq:*",
                "opsworks:*",
                "rds:*",
                "redshift:*",
                "route53:*",
                "s3:*",
                "sagemaker:*",
                "sdb:*",
                "servicecatalog:*",
                "ses:*",
                "sns:*",
                "sqs:*",
                "states:*",
                "waf:*",
                "waf-regional:*",
                "workspaces:*",
                "ssm:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::account-id:role/CodeStar-project-id*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "cloudformation:CreateChangeSet"
            ],
            "Resource": [
                "arn:aws:cloudformation:us-east-1:aws:transform/Serverless-2016-10-31",
                "arn:aws:cloudformation:us-east-1:aws:transform/CodeStarGamma"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:GetRole",
                "iam:DeleteRole",
                "iam:DeleteRolePolicy",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:DeleteUser",
                "iam:DeleteUserPolicy",
                "iam:AttachUserPolicy",
                "iam:DetachUserPolicy"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Condition": {
                "StringEquals": {
                    "iam:PermissionsBoundary": "arn:aws:iam::account-id:policy/CodeStar_project-id_PermissionsBoundary"
                }
            },
            "Action": [
                "iam:CreateRole",
                "iam:CreateUser",
                "iam:PutUserPermissionsBoundary",
                "iam:PutRolePermissionsBoundary"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "kms:CreateKey",
                "kms:CreateAlias",
                "kms:DeleteAlias",
                "kms:DisableKey",
                "kms:EnableKey",
                "kms:UpdateAlias",
                "kms:TagResource",
                "kms:UntagResource"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

### Toolchain Worker Role Policy \(After December 6, 2018 PDT\)<a name="adh-policy-toolchain-worker-new"></a>

For AWS CodeStar projects created after December 6, 2018 PDT, CodeStar creates an inline policy for a worker role that creates resources for your project in other AWS services\. The exact contents of the policy depend on the type of project you are creating\. The following policy is an example\. For more information, see [IAM Policies for Worker Roles](access-permissions-proj.md#access-permissions-proj-worker)\.

```
{
  "Statement": [
    {
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:GetBucketVersioning",
        "s3:PutObject*",
        "codecommit:CancelUploadArchive",
        "codecommit:GetBranch",
        "codecommit:GetCommit",
        "codecommit:GetUploadArchiveStatus",
        "codecommit:GitPull",
        "codecommit:UploadArchive",
        "codebuild:StartBuild",
        "codebuild:BatchGetBuilds",
        "codebuild:StopBuild",
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "cloudformation:DescribeStacks",
        "cloudformation:DescribeChangeSet",
        "cloudformation:CreateChangeSet",
        "cloudformation:DeleteChangeSet",
        "cloudformation:ExecuteChangeSet",
        "codepipeline:StartPipelineExecution",
        "lambda:ListFunctions",
        "lambda:InvokeFunction",
        "sns:Publish"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "kms:GenerateDataKey*",
        "kms:Encrypt",
        "kms:Decrypt"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

### AWS CodeBuild Worker Role Policy \(Before December 6, 2018 PDT\)<a name="adh-policy-acb-worker-existing"></a>

If your CodeStar project was created before December 6, 2018 PDT, CodeStar created an inline policy for an AWS CodeBuild worker role\. The following policy statement is an example\.

```
{
    "Statement": [
        {
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe",
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe/*",
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-app",
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-app/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "codecommit:GitPull"
            ],
            "Resource": [
                "arn:aws:codecommit:us-east-1:account-id:project-id"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "kms:GenerateDataKey*",
                "kms:Encrypt",
                "kms:Decrypt"
            ],
            "Resource": [
                "arn:aws:kms:us-east-1:account-id:alias/aws/s3"
            ],
            "Effect": "Allow"
        }
    ]
}
```

### AWS CodePipeline Worker Role Policy \(Before December 6, 2018 PDT\)<a name="adh-policy-acp-worker-existing"></a>

If your CodeStar project was created before December 6, 2018 PDT, CodeStar created an inline policy for an AWS CodePipeline worker role\. The following policy statement is an example\.

```
{
    "Statement": [
        {
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:GetBucketVersioning",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe",
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "codecommit:CancelUploadArchive",
                "codecommit:GetBranch",
                "codecommit:GetCommit",
                "codecommit:GetUploadArchiveStatus",
                "codecommit:UploadArchive"
            ],
            "Resource": [
                "arn:aws:codecommit:us-east-1:account-id:project-id"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "codebuild:StartBuild",
                "codebuild:BatchGetBuilds",
                "codebuild:StopBuild"
            ],
            "Resource": [
                "arn:aws:codebuild:us-east-1:account-id:project/project-id"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "cloudformation:DescribeStacks",
                "cloudformation:DescribeChangeSet",
                "cloudformation:CreateChangeSet",
                "cloudformation:DeleteChangeSet",
                "cloudformation:ExecuteChangeSet"
            ],
            "Resource": [
                "arn:aws:cloudformation:us-east-1:account-id:stack/awscodestar-project-id-lambda/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::account-id:role/CodeStarWorker-project-id-CloudFormation"
            ],
            "Effect": "Allow"
        }
    ]
}
```

### AWS CloudFormation Worker Role Policy \(Before December 6, 2018 PDT\)<a name="adh-policy-cfn-worker-existing"></a>

If your CodeStar project was created before December 6, 2018 PDT, CodeStar created an inline policy for an AWS CloudFormation worker role\. The following policy statement is an example\.

```
{
    "Statement": [
        {
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe",
                "arn:aws:s3:::aws-codestar-us-east-1-account-id-project-id-pipe/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "codestar:SyncResources",
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:AddPermission",
                "lambda:UpdateFunction",
                "lambda:UpdateFunctionCode",
                "lambda:GetFunction",
                "lambda:GetFunctionConfiguration",
                "lambda:UpdateFunctionConfiguration",
                "lambda:RemovePermission",
                "lambda:listTags",
                "lambda:TagResource",
                "lambda:UntagResource",
                "apigateway:*",
                "dynamodb:CreateTable",
                "dynamodb:DeleteTable",
                "dynamodb:DescribeTable",
                "kinesis:CreateStream",
                "kinesis:DeleteStream",
                "kinesis:DescribeStream",
                "sns:CreateTopic",
                "sns:DeleteTopic",
                "sns:ListTopics",
                "sns:GetTopicAttributes",
                "sns:SetTopicAttributes",
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "config:DescribeConfigRules",
                "config:PutConfigRule",
                "config:DeleteConfigRule",
                "ec2:*",
                "autoscaling:*",
                "elasticloadbalancing:*",
                "elasticbeanstalk:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::account-id:role/CodeStarWorker-project-id-Lambda"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "cloudformation:CreateChangeSet"
            ],
            "Resource": [
                "arn:aws:cloudformation:us-east-1:aws:transform/Serverless-2016-10-31",
                "arn:aws:cloudformation:us-east-1:aws:transform/CodeStar"
            ],
            "Effect": "Allow"
        }
    ]
}
```

### CloudWatch Events Worker Role Policy \(Before December 6, 2018 PDT\)<a name="adh-policy-cwe-worker-existing"></a>

If your CodeStar project was created before December 6, 2018 PDT, CodeStar created an inline policy for an CloudWatch Events worker role\. The following policy statement is an example\.

```
{
    "Statement": [
        {
            "Action": [
                "codepipeline:StartPipelineExecution"
            ],
            "Resource": [
                "arn:aws:codepipeline:us-east-1:account-id:project-id-Pipeline"
            ],
            "Effect": "Allow"
        }
    ]
}
```