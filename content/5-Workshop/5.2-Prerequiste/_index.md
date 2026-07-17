---
title : "Environment preparation and IAM"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

### Goal
This section validates the AWS account, Region, IAM permissions, and prerequisites before reviewing the TaskManager system.

### Region
The workshop uses **Asia Pacific (Singapore) - ap-southeast-1** for the main services: Cognito, AppSync, Lambda, DynamoDB, S3, and CloudWatch.

### Validate IAM
1. Open the AWS Management Console.
2. Search for **IAM**.
3. Review the IAM dashboard to confirm the account has basic security controls.

![Image](images/workshop/iam.png)
The dashboard shows that MFA is enabled for the root user and there are no active root access keys. These are important security recommendations for an AWS account.

### Required permissions
The IAM user or role used in this workshop needs access to the following services:

* Amazon S3: upload frontend assets.
* Amazon Cognito: review User Pool and App Client settings.
* AWS AppSync: review GraphQL API, endpoint, and authorization mode.
* AWS Lambda: review function list and runtime configuration.
* Amazon DynamoDB: review tables, indexes, capacity mode, and PITR.
* Amazon CloudWatch: review log groups and log streams.
* IAM: review roles and policies related to Lambda, AppSync, and deployment.

### Resource naming convention
The workshop resources use the `dev` suffix for the development environment:

* `taskmanager-frontend-dev-*`
* `taskmanager-users-dev`
* `TaskManagerAPI-dev`
* `userManager-dev`
* `boardManager-dev`
* `taskProcessor-dev`
* `streamProcessor-dev`
* `TaskManager-Boards-dev`
* `TaskManager-Tasks-dev`
* `TaskManager-Users-dev`

### Prerequisite checklist
* Sign in to the correct AWS account.
* Select the `ap-southeast-1` Region.
* Confirm the IAM user or role can view the required resources.
* Prepare the frontend build output, including `index.html`, JavaScript bundle, CSS bundle, and static SVG assets.