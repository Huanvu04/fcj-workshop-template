---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# DEPLOYING TASK MANAGEMENT SYSTEM ON AWS SERVERLESS

### Overview

This workshop provides guidance on deploying and verifying a **Task Management System** based on a serverless architecture on AWS. The system allows users to log in, manage boards, create tasks, update task statuses, and record operational activities through services managed by AWS.

The architecture utilizes Amazon S3 for storing static frontend, Amazon Cognito for user authentication, AWS AppSync to provide a GraphQL API, AWS Lambda for business logic processing, Amazon DynamoDB for data storage, IAM for access control, and Amazon CloudWatch for log monitoring.

### Contents

1. [Workshop overview](5.1-Workshop-overview/)
2. [Environment preparation and IAM permissions](5.2-Prerequiste/)
3. [Deploying frontend with Amazon S3](5.3-S3-frontend/)
4. [Configuring authentication, API, and backend](5.4-Backend-config/)
5. [Checking DynamoDB, indexes, and PITR](5.5-DynamoDB-check/)
6. [Cleaning up resources](5.6-Cleanup/)