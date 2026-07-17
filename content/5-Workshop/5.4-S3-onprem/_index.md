---
title : "Configure authentication, API, and backend"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Overview
This section validates the main layers behind the TaskManager frontend:

* **Amazon Cognito**: manages users and issues login tokens.
* **AWS AppSync**: provides the GraphQL API for the frontend.
* **AWS Lambda**: runs business logic for users, boards, tasks, and stream processing.
* **Amazon CloudWatch**: stores logs for backend troubleshooting.

### Authentication and API flow
1. The user signs in to the frontend.
2. Cognito authenticates the user and returns a JWT token.
3. The frontend sends GraphQL requests to AppSync with the token.
4. AppSync validates the token and invokes Lambda resolvers.
5. Lambda reads and writes data in DynamoDB.
6. CloudWatch stores logs for troubleshooting.

### Content
* Validate the Cognito User Pool
* Validate the AppSync GraphQL API
* Validate Lambda functions
* Validate CloudWatch logs

