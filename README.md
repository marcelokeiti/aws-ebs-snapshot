# aws-ebs-snapshot
Lambda functions aimed to manage EBS snapshots

## Getting Started

These instructions will guide you to deploy the solution on Amazon Web Services

### Prerequisites

* AWS Account: This guide will assume that you have an AWS account configured on your local machine. For more information [see](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

### Installing

#### 1. Create IAM Role: AWSManageEBSSnapshotForLambdaRole

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:*"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateSnapshot",
                "ec2:DeleteSnapshot",
                "ec2:CreateTags",
                "ec2:ModifySnapshotAttribute",
                "ec2:ResetSnapshotAttribute"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

#### 2. Create Lambda Functions

```
Name: create-ebs-snapshot
Runtime: Python 2.7
Role: AWSManageEBSSnapshotForLambdaRole
```
```
Name: tag-ebs-snapshot
Runtime: Python 2.7
Role: AWSManageEBSSnapshotForLambdaRole
```
```
Name: delete-ebs-snapshot
Runtime: Python 2.7
Role: AWSManageEBSSnapshotForLambdaRole
```

#### 3. Configure Cloud Watch

```
Name: create-ebs-snapshot
Schedule:
   Cron Expression Example: 0 2 * * ? *
Target: Lambda Function create-ebs-snapshot
```
```
Name: tag-ebs-snapshot
Event Pattern:
   Service Name: EC2
   Event Type: EBS Snapshot Notification
Target: Lambda Function tag-ebs-snapshot
```
```
Name: delete-ebs-snapshot
Schedule:
   Cron Expression Example: 15 2 * * ? *
Target: Lambda Function delete-ebs-snapshot
```
