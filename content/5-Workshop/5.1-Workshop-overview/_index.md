---
title : "Workshop Overview"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

### Goal
In this workshop, you will examine the core components of the **Task Management System** deployed on AWS:

* Frontend static assets are uploaded to Amazon S3.
* Users are managed by Amazon Cognito User Pool.
* The main API uses AWS AppSync GraphQL API with Cognito as the primary auth mode.
* Backend logic runs using AWS Lambda.
* Data for boards, tasks, users, notifications, and activity logs are stored in Amazon DynamoDB.
* DynamoDB has Point-in-Time Recovery (PITR) enabled to protect data from accidental write/delete operations.
* Amazon CloudWatch collects logs from Lambda functions and operational components.

### High-level Architecture
![Task Management System Architecture](images/workshop/tongquan.png)


### Main request flow
1. Developer builds the frontend and uploads static files to the S3 bucket `taskmanager-frontend-dev-*`.
2. Users log in via Cognito User Pool `taskmanager-users-dev`.
3. The frontend sends GraphQL queries/mutations/subscriptions to the AppSync API `TaskManagerAPI-dev`.
4. AppSync invokes Lambda functions such as `userManager-dev`, `boardManager-dev`, `taskProcessor-dev`, and `streamProcessor-dev`.
5. Lambda reads/writes data to DynamoDB tables `TaskManager-Boards-dev`, `TaskManager-Tasks-dev`, `TaskManager-Users-dev`, `TaskManager-Notifications-dev`, and `TaskManager-ActivityLogs-dev`.
6. CloudWatch records logs to support troubleshooting and operational observability.

### Expected Outcomes
After completing this workshop, you will be able to explain each AWS service in the TaskManager system, know how to inspect deployed resources, verify DynamoDB data, check PITR, and read operational logs in CloudWatch.