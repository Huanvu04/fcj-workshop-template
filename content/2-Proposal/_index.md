---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# TASK MANAGEMENT SYSTEM

## AWS Serverless Architecture for Secure Task Management System

### 1. Executive Summary
The Task Management System is designed as a secure, scalable, and operationally efficient web application for creating, assigning, tracking, and updating tasks in real-time. The system leverages an AWS Serverless architecture to focus on application functionality rather than server management, OS patching, or manual infrastructure provisioning.

The frontend is distributed via Amazon CloudFront from a private Amazon S3 bucket, Amazon Route 53 handles DNS, AWS Certificate Manager provides TLS certificates, and AWS WAF protects the public edge layer. Users are authenticated via Amazon Cognito and send GraphQL queries, mutations, and subscriptions to AWS AppSync with JWT. Business logic runs on AWS Lambda, task data is stored in Amazon DynamoDB with point-in-time recovery enabled, and logs, metrics, and alarms are centralized in Amazon CloudWatch. The deployment process is automated from GitHub through AWS CodePipeline, AWS CodeBuild, and AWS CloudFormation.

### 2. Problem Statement

#### Current Issues
* Many small teams still manage work using fragmented messaging, spreadsheets, or notes.
* This approach makes it difficult to determine task ownership, current status, priority levels, and change history.
* As the number of tasks increases, manual updates are prone to data duplication, missed deadlines, and reduced progress tracking capabilities.
* Furthermore, traditional web deployment can create unnecessary operational overhead for an internship or student project: server management, manual deployment, database backups, SSL configuration, and error monitoring.

#### Solution
The system provides a centralized task management application based on managed AWS services. Users log in via Cognito and then interact with tasks through the AppSync GraphQL API. Queries and mutations support viewing task lists, creating tasks, updating tasks, assigning work, and completing tasks. GraphQL subscriptions support real-time updates so that members see changes without needing to reload the page.

#### Benefits and ROI
The system improves progress visibility by centralizing work data, statuses, owners, and updates in one place. Serverless services result in a leaner operational model and better cost control since most resources are billed based on usage. Automated deployment helps shorten release times, reduce configuration errors, and makes the project easier to maintain after the internship phase.

### 3. Solution Architecture
The architecture is divided into serverless layers including frontend distribution, authentication, API, backend, data, operational monitoring, and CI/CD. Website requests are resolved by Route 53 and distributed via CloudFront from a private S3 bucket. Login requests go through Cognito. Authenticated application requests use AppSync GraphQL with JWT, after which AppSync invokes Lambda for business logic and DynamoDB for task data persistence. CloudWatch collects logs, metrics, and alarms for the entire application.

![Task Management System Architecture](/images/proposal/task_management_architecture.png)

#### AWS Services Used
* **Amazon Route 53**: Manages DNS records for the application domain.
* **Amazon CloudFront**: Provides global frontend distribution and connects users to the S3 private origin.
* **Amazon S3**: Stores static website assets in a private bucket.
* **AWS WAF**: Protects the CloudFront distribution against common web attacks.
* **AWS Certificate Manager**: Provisions and manages TLS certificates for HTTPS.
* **Amazon Cognito**: Handles registration, login, JWT token issuance, and user authentication.
* **AWS AppSync**: Provides the GraphQL API for queries, mutations, and subscriptions.
* **AWS Lambda**: Executes task management business logic without requiring server management.
* **Amazon DynamoDB**: Stores task data, users, statuses, and assignments; with point-in-time recovery enabled.
* **Amazon CloudWatch**: Collects logs, metrics, and alarms for monitoring and troubleshooting.
* **AWS CodePipeline**: Automates deployment based on source code changes.
* **AWS CodeBuild**: Builds and tests application artifacts.
* **AWS CloudFormation**: Initializes and updates infrastructure as code.
* **GitHub**: Stores source code and triggers the CI/CD pipeline.

#### Component Design
* **Frontend Distribution**: Static frontend files are uploaded to an S3 private bucket and distributed via CloudFront. Route 53 maps the custom domain to the CloudFront distribution, and ACM enables HTTPS.
* **Edge Security**: AWS WAF is attached to CloudFront to filter malicious or abnormal traffic before requests reach the application.
* **Authentication**: Cognito manages users and returns a JWT token after login. The frontend attaches this token to GraphQL requests.
* **API Layer**: AppSync validates JWT and provides GraphQL operations for creating tasks, updating tasks, filtering lists, and receiving real-time events.
* **Backend Logic**: Lambda implements business rules such as checking task operation permissions, updating statuses, and normalizing responses.
* **Data Layer**: DynamoDB stores task data and enables point-in-time recovery to protect against accidental writes or deletions.
* **Operations**: CloudWatch receives logs and metrics from AppSync, Lambda, DynamoDB, and the deployment pipeline. Alarms can notify when errors or unusual usage occur.
* **Deployment**: Developers push code to GitHub. CodePipeline and CodeBuild package the application, then CloudFormation updates AWS resources and deploys frontend/backend changes.

### 4. Technical Implementation

#### Implementation Phases
1. Requirement Analysis and Architectural Design: Identify user roles, task lifecycle, GraphQL operations, security boundaries, and responsibilities of each AWS service.
2. Infrastructure as Code Setup: Create CloudFormation templates for S3, CloudFront, WAF, ACM, Cognito, AppSync, Lambda, DynamoDB, CloudWatch, and CI/CD.
3. Frontend Development: Build the interface, authentication screens, task list, task details, filters, and real-time updates.
4. Backend and API Development: Define the GraphQL schema, connect resolvers with Lambda and DynamoDB, and set up the deployment pipeline.
5. Monitoring and Security Hardening: Configure CloudWatch logs, metrics, alarms, IAM least privilege, DynamoDB PITR, and WAF rules.
6. Testing and Deployment: Test login, CRUD operations, subscription updates, CI/CD deployment, rollbacks, and basic error scenarios.

#### Technical Requirements
* Static web frontend capable of being built and deployed to S3.
* Cognito user pool for authentication and JWT-based authorization.
* AppSync GraphQL schema covering task queries, mutations, and subscriptions.
* Lambda runtime for backend logic and DynamoDB integration.
* DynamoDB table design for access patterns related to tasks, users, projects, statuses, priorities, and assignments.
* CloudFormation templates for repeatable infrastructure deployment.
* CI/CD pipeline integrated with GitHub for automatic builds and deployments.
* CloudWatch dashboards or alarms for error tracking and operational observability.

### 5. Roadmap and Milestones
* **Pre-deployment**: Research AWS Serverless services, compare architectural approaches, and finalize project scope.
* **Month 1**: Design the solution architecture, define the data model, create initial CloudFormation templates, and configure basic frontend hosting.
* **Month 2**: Deploy Cognito authentication, AppSync GraphQL API, Lambda functions, and DynamoDB persistence.
* **Month 3**: Complete CI/CD, monitoring, WAF configuration, integration testing, documentation, and the final presentation.
* **Post-deployment**: Improve UI/UX, add grouping by team/project, enhance audit logging, and optimize costs based on actual usage.

### 6. Budget Estimation
The project is designed for small-scale workloads during the internship phase, so most usage is expected to fall within low-cost or free tier limits. Final estimates should be recalculated using the AWS Pricing Calculator before deployment, as AWS costs depend on region, traffic, and selected plans.

#### Estimated Infrastructure Costs
* **Route 53**: Approximately 0.50 USD/month for a public hosted zone, plus DNS query charges if applicable.
* **CloudFront and S3**: Expected to be very low for static assets and small traffic; CloudFront also offers a free plan with monthly usage allowances.
* **Cognito**: Expected 0 USD/month for internal user groups falling within the free tier based on monthly active users.
* **AppSync**: Expected 0 USD/month while within free-tier usage limits for queries/mutations and real-time operations.
* **Lambda**: Expected 0 USD/month with low invocation counts falling within the Lambda free tier.
* **DynamoDB**: Expected 0 USD/month for small tables within the free-tier storage/provisioned capacity, or low pay-per-request costs if using on-demand mode.
* **CloudWatch**: Expected 0 USD/month if logs, metrics, and alarms are within the free tier.
* **WAF**: The primary recurring security cost if enabled separately; estimated at 10-15 USD/month for a Web ACL with a small rule set and low request volume.
* **CodePipeline, CodeBuild, and CloudFormation**: Expected to be low for small pipelines and limited build frequency.

**Total Estimate**: Approximately 1-3 USD/month for core serverless application traffic when WAF is excluded, or 12-18 USD/month when WAF is enabled.

### 7. Risk Assessment

#### Risk Matrix
* Misconfiguration of authentication or authorization: High impact, medium probability.
* GraphQL resolver or Lambda errors: Medium impact, medium probability.
* Inappropriate DynamoDB access patterns: Medium impact, medium probability.
* Accidental data deletion or incorrect updates: High impact, low probability.
* Costs arising from WAF, logs, or sudden traffic spikes: Medium impact, low probability.
* CI/CD deployment failure: Medium impact, medium probability.

#### Mitigation Strategies
* Apply IAM least privilege and check Cognito JWT authorization in AppSync.
* Use structured logging and CloudWatch alarms for Lambda and AppSync errors.
* Design DynamoDB keys according to actual access patterns before deployment.
* Enable DynamoDB point-in-time recovery and restrict manual direct data entry in the production environment.
* Configure AWS Budgets and review CloudWatch log retention.
* Keep infrastructure changes in CloudFormation and test the pipeline before acceptance.

#### Contingency Plan
* Roll back failed deployments via CloudFormation and CodePipeline.
* Restore DynamoDB records using point-in-time recovery if data is modified unintentionally.
* Temporarily disable non-essential features like subscriptions or certain WAF rules if they cause cost or availability issues during testing.
* Use manual deployment only as a short-term measure while fixing the CI/CD pipeline.

### 8. Expected Outcomes

#### Technical Improvements
The final system provides a serverless task management application with secure authentication, real-time GraphQL updates, automated deployment, centralized monitoring, and recoverable task data storage.

#### Long-term Value
The architecture can be reused as a foundation for group collaboration tools, issue tracking, or internal workflow applications in the future. The project also demonstrates practical AWS skills in frontend hosting, identity, serverless backend, NoSQL data modeling, monitoring, security, and CI/CD automation.