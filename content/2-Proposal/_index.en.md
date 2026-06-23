---

title: "2. Proposal"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

# 2. Proposal

## 1. Project Name

**Vietnamese title:** Hệ thống quản lý tài chính cá nhân, phân tích AI và cảnh báo vượt ngân sách trên AWS

**English title:** AI-Powered Personal Finance Budget Alert System on AWS

---

## 2. Project Overview

This project aims to build a personal finance management system on Amazon Web Services using a serverless architecture. The system allows users to register, log in, manage income and expense transactions, set monthly personal budgets, receive alerts when spending exceeds the configured budget, and use AI to suggest transaction categories as well as generate monthly spending insights.

The system includes a simple web frontend for users to interact with the main features, a REST API backend to handle business logic, a NoSQL database for data storage, a user authentication service, an email notification service, a logging and monitoring system, and an AI service to support spending analysis.

The project is implemented using AWS services such as Amazon S3, Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon SNS, Amazon CloudWatch, Amazon Bedrock, AWS IAM, and AWS Budgets. The goal is to build a practical, low-cost, scalable, cloud-native system with monitoring capabilities and AI-powered support to help users make better financial decisions.

---

## 3. Project Purpose

The main purpose of this project is to help users track and control their personal finances more proactively. Instead of only recording income and expense transactions, the system also helps users identify when their spending exceeds the configured monthly budget so that they can adjust their spending behavior in time.

In addition, the system uses AI to support users in managing their finances. AI can suggest suitable categories based on transaction descriptions and generate short monthly spending insights. This helps users better understand their spending habits.

From a technical perspective, this project provides hands-on practice in deploying a complete serverless system on AWS, including a web frontend, user authentication, REST API, business logic processing with Lambda, data storage with DynamoDB, email notifications with SNS, system monitoring with CloudWatch, AI integration with Amazon Bedrock, and cost control with AWS Budgets.

---

## 4. Target Users

The system is designed for the following user groups:

* Students who want to track monthly expenses.
* Working individuals who want to manage personal income and expenses.
* Individual users who want to receive alerts when spending exceeds their budget.
* Users who want simple AI-generated insights about their spending behavior.
* AWS learners who want to practice building a cloud-native application with frontend, authentication, API, database, notification, monitoring, and AI features.

---

## 5. Problem Statement

In practice, many users do not track their spending regularly, do not categorize transactions clearly, and may not realize when their spending has exceeded their monthly budget. This can lead to poor personal financial control, especially for students or users who are new to managing personal finance.

In addition, traditional systems often require servers, relational databases, and fixed operational costs. For a learning project, using a serverless architecture helps reduce cost, minimize infrastructure management, and still maintain scalability when the number of users increases.

This project addresses the following problems:

* Managing personal income and expense transactions.
* Setting and tracking monthly budgets.
* Sending alerts when total spending exceeds the budget.
* Suggesting transaction categories using AI.
* Generating spending insights using AI.
* Providing a simple web interface for users.
* Logging, monitoring, and supporting system debugging.
* Controlling AWS deployment costs.
* Cleaning up AWS resources after project completion.

---

## 6. Project Objectives

The project aims to achieve the following objectives:

* Build a simple web frontend for users to access the main features.
* Deploy the frontend as a static website on Amazon S3.
* Build REST APIs for the personal finance management system.
* Allow users to register and log in using Amazon Cognito.
* Allow users to create, view, update, and delete transactions.
* Allow users to set monthly budgets.
* Automatically calculate total expenses and compare them with the monthly budget.
* Send email alerts when total spending exceeds the budget.
* Use AI to suggest transaction categories from transaction descriptions.
* Use AI to generate monthly spending insights.
* Store data using Amazon DynamoDB.
* Process business logic using AWS Lambda.
* Expose APIs using Amazon API Gateway.
* Log and monitor the system using Amazon CloudWatch.
* Manage permissions using AWS IAM based on the least privilege principle.
* Keep the total project cost within a maximum budget of 200 USD for the entire implementation period.
* Write bilingual workshop documentation with step-by-step deployment instructions.
* Evaluate the system based on the six pillars of the AWS Well-Architected Framework.

---

## 7. Project Scope

Within the scope of this project, the system focuses on the following core features:

* User registration and login.
* A simple web interface for users.
* Creating income or expense transactions.
* Viewing transaction lists.
* Updating transaction information.
* Deleting transactions.
* Setting monthly budgets.
* Checking total expenses against the budget.
* Sending email alerts when the budget is exceeded.
* AI-based transaction category suggestion.
* AI-generated spending insights.
* System logging and monitoring.
* API testing with Postman.
* Basic user flow testing on the web frontend.
* Recording implementation evidence and testing results.

The frontend does not focus on complex UI design. However, it must support the basic user flows, including registration, login, adding transactions, viewing transaction lists, setting budgets, checking budget alerts, and viewing AI-generated spending insights.

---

## 8. AWS Services Used

The following AWS services are planned for this project:

| AWS Service        | Role in the System                                                          |
| ------------------ | --------------------------------------------------------------------------- |
| Amazon S3          | Hosts and deploys the static web frontend for users.                        |
| Amazon Cognito     | Manages user registration, login, and authentication.                       |
| Amazon API Gateway | Provides REST API endpoints for the frontend or Postman to call the system. |
| AWS Lambda         | Handles business logic for transactions, budgets, alerts, and AI features.  |
| Amazon DynamoDB    | Stores transactions, budgets, and alert history.                            |
| Amazon SNS         | Sends email alerts when spending exceeds the budget.                        |
| Amazon CloudWatch  | Records logs, tracks metrics, and supports error monitoring.                |
| Amazon Bedrock     | Supports AI-powered transaction category suggestion and spending insights.  |
| AWS IAM            | Manages access permissions between Lambda and other AWS services.           |
| AWS Budgets        | Tracks cost and sends alerts when spending reaches configured thresholds.   |

Services such as Amazon RDS, NAT Gateway, Amazon ECS, Load Balancer, and Amazon EC2 are not used in the main scope of this project in order to reduce cost and implementation complexity.

---

## 9. Solution Architecture

The following diagram shows the overall architecture of the system:

![System architecture overview](/FCAJ---Workshop--aws/images/2-Proposal/architecture-overview_eng.png)

Main system workflow:

1. The user accesses the web frontend deployed on Amazon S3.
2. The user registers or logs in through Amazon Cognito.
3. After successful authentication, the user receives a JWT token.
4. The frontend sends requests to Amazon API Gateway with the authentication token.
5. API Gateway validates the request and forwards valid requests to AWS Lambda.
6. Lambda handles transaction, budget, alert, and AI-related business logic.
7. Lambda reads and writes data to Amazon DynamoDB.
8. When the user enters a transaction description, Lambda can call Amazon Bedrock to suggest a transaction category.
9. When the user requests spending analysis, Lambda aggregates transaction data and calls Amazon Bedrock to generate insights.
10. If total spending exceeds the configured budget, Lambda sends a notification to Amazon SNS.
11. Amazon SNS sends an email alert to the user.
12. Amazon CloudWatch records logs, metrics, and supports error monitoring.
13. AWS IAM roles grant Lambda the required permissions to access DynamoDB, SNS, CloudWatch, and Bedrock.
14. AWS Budgets monitors project cost during implementation.

---

## 10. Expected Data Model

The system is expected to use three main DynamoDB tables: Transactions, Budgets, and AlertEvents.

### 10.1. Transactions Table

The `Transactions` table stores user income and expense transactions.

**Key schema:**

| Component     | Attribute     |
| ------------- | ------------- |
| Partition key | userId        |
| Sort key      | transactionId |

**Main attributes:**

| Attribute     | Description                                 |
| ------------- | ------------------------------------------- |
| userId        | User identifier.                            |
| transactionId | Transaction identifier.                     |
| type          | Transaction type, either income or expense. |
| category      | Transaction category.                       |
| amount        | Transaction amount.                         |
| description   | Transaction description.                    |
| createdAt     | Transaction creation time.                  |
| updatedAt     | Last update time.                           |

This design allows the system to query transactions by user without scanning the entire table.

### 10.2. Budgets Table

The `Budgets` table stores monthly budgets for each user.

**Key schema:**

| Component     | Attribute |
| ------------- | --------- |
| Partition key | userId    |
| Sort key      | month     |

**Main attributes:**

| Attribute      | Description                          |
| -------------- | ------------------------------------ |
| userId         | User identifier.                     |
| month          | Budget month, for example 2026-06.   |
| budgetLimit    | Monthly budget limit.                |
| currentExpense | Current total expense for the month. |
| updatedAt      | Last update time.                    |

This design allows each user to set a separate budget for each month.

### 10.3. AlertEvents Table

The `AlertEvents` table stores the history of budget overrun alerts.

**Key schema:**

| Component     | Attribute |
| ------------- | --------- |
| Partition key | userId    |
| Sort key      | alertId   |

**Main attributes:**

| Attribute      | Description                                    |
| -------------- | ---------------------------------------------- |
| alertId        | Alert identifier.                              |
| userId         | User identifier.                               |
| month          | Month when the alert was generated.            |
| budgetLimit    | Configured monthly budget.                     |
| currentExpense | Total expense at the time of alert generation. |
| status         | Alert sending status, such as SENT or FAILED.  |
| createdAt      | Alert creation time.                           |

This table helps record alert history for checking and debugging.

---

## 11. Expected APIs

The main APIs of the system include:

| Method | Endpoint           | Function                                |
| ------ | ------------------ | --------------------------------------- |
| POST   | /transactions      | Create a new transaction                |
| GET    | /transactions      | Get the transaction list                |
| GET    | /transactions/{id} | Get transaction details                 |
| PUT    | /transactions/{id} | Update a transaction                    |
| DELETE | /transactions/{id} | Delete a transaction                    |
| POST   | /budgets           | Set a monthly budget                    |
| GET    | /budgets           | Get the current budget                  |
| POST   | /budgets/check     | Check budget overrun                    |
| POST   | /ai/categorize     | Suggest a transaction category using AI |
| GET    | /ai/insights       | Generate spending insights using AI     |

All main APIs must be protected using Amazon Cognito Authorizer. Users must log in and send a valid JWT token when calling the APIs.

---

## 12. User Interface

The web frontend is built at a simple level to support demonstration and end-to-end business flow testing. The frontend can be developed using React or adapted from existing UI components, then modified to integrate with the new AWS serverless backend.

Expected screens include:

* User registration screen.
* Login screen.
* Transaction list screen.
* Add transaction form.
* Update transaction form.
* Monthly budget setup screen.
* Budget alert checking screen.
* AI spending insights screen.

The frontend will call API Gateway endpoints and send the JWT token received from Amazon Cognito. The frontend must not store AWS Access Key, Secret Key, or sensitive information in the source code.

---

## 13. AI Features

The project uses AI for two main features.

### 13.1. AI Transaction Category Suggestion

When the user enters a transaction description, the system can use AI to suggest a suitable category.

Examples:

| Transaction Description   | Suggested Category |
| ------------------------- | ------------------ |
| chicken rice for lunch    | Food               |
| motorcycle fuel           | Transport          |
| June salary               | Salary             |
| buying AWS learning books | Education          |
| mobile phone bill         | Utilities          |

This feature helps users save time when entering transactions and supports more consistent data categorization.

### 13.2. AI Spending Insights

Based on transaction and budget data, the system generates short spending insights for the user.

Examples:

* This month, most of your spending is on Food and Shopping.
* Your current spending has exceeded the budget by 12%.
* You should reduce unnecessary expenses in the following week.
* Your food spending ratio is higher than other categories.

Within the scope of this project, AI only supports simple analysis. The system does not train a custom model, does not perform fine-tuning, and does not use batch inference in order to avoid unnecessary costs.

---

## 14. AWS Well-Architected Framework Alignment

The project is designed based on the principles of the AWS Well-Architected Framework to ensure that the system is not only functional, but also operable, secure, scalable, cost-controlled, and efficient.

### 14.1. Operational Excellence

The system uses Amazon CloudWatch to record Lambda logs, track API errors, and support debugging. Deployment, testing, and cleanup steps are documented in the workshop so that the system can be reviewed and reproduced.

### 14.2. Security

The system uses Amazon Cognito to authenticate users. API Gateway validates JWT tokens before forwarding requests to Lambda. AWS IAM roles are configured based on the least privilege principle. The source code does not contain AWS Access Key, Secret Key, or hard-coded tokens.

### 14.3. Reliability

Lambda functions must handle errors clearly when DynamoDB, SNS, or Bedrock fails. APIs should return appropriate status codes such as 400, 401, 404, or 500. If AI or SNS fails, the system must record logs for debugging and should not affect other core features.

### 14.4. Performance Efficiency

The system uses a serverless architecture to scale automatically based on request volume. DynamoDB is designed with suitable partition keys and sort keys to query by user and avoid full table scans. Lambda timeout and memory should be configured appropriately for demo-scale usage.

### 14.5. Cost Optimization

The project uses AWS Budgets to monitor cost and send alerts when spending reaches configured thresholds. The system prioritizes serverless services such as Lambda, API Gateway, DynamoDB, and S3 static hosting. The project does not use NAT Gateway, RDS, ECS, Load Balancer, or Bedrock Provisioned Throughput unless absolutely necessary.

### 14.6. Sustainability

The system minimizes always-running resources by using Lambda and static frontend hosting on S3. Demo resources will be cleaned up after project completion. CloudWatch log retention should be configured properly to avoid storing logs longer than necessary.

---

## 15. Testing and Evaluation

The system will be tested using the web frontend, Postman, and AWS Console.

### 15.1. Authentication Testing

* Register a user using Amazon Cognito.
* Log in and obtain a JWT token.
* Call APIs with a valid token.
* Call APIs without a token.
* Call APIs with an invalid token.

### 15.2. Transaction Testing

* Create an income transaction.
* Create an expense transaction.
* Get the transaction list.
* Get transaction details.
* Update a transaction.
* Delete a transaction.
* Check data stored in DynamoDB.

### 15.3. Budget and Alert Testing

* Set a monthly budget.
* Create multiple expense transactions to exceed the budget.
* Call the budget checking API.
* Check the email alert sent by Amazon SNS.
* Check alert data in the AlertEvents table.

### 15.4. AI Testing

* Call the AI category suggestion API.
* Call the AI spending insights API.
* Test cases where Bedrock fails or does not respond.
* Limit the number of AI calls during the demo to control cost.

### 15.5. Frontend Testing

* Test the registration and login flow.
* Test adding, updating, and deleting transactions from the web interface.
* Test setting a monthly budget from the web interface.
* Test error messages when APIs fail.
* Test displaying AI insights on the interface.

### 15.6. Operational Testing

* Check logs in CloudWatch.
* Check Lambda error logs.
* Check API responses when input data is invalid.
* Check AWS Budget and cost alerts.
* Check cleanup after completing the demo.

---

## 16. Expected Outcomes

After completion, the project is expected to achieve the following outcomes:

* A simple web frontend for users to access the system.
* The frontend is deployed using Amazon S3 Static Website Hosting.
* A serverless REST API system running on AWS.
* Users can register and log in using Amazon Cognito.
* Users can manage personal income and expense transactions.
* Users can set monthly budgets and receive alerts when spending exceeds the budget.
* The system provides AI-powered transaction category suggestions.
* The system provides AI-generated monthly spending insights.
* Data is stored in DynamoDB.
* APIs are protected by Amazon Cognito.
* Email alerts are sent through Amazon SNS.
* CloudWatch records logs and supports system monitoring.
* IAM roles are configured based on the least privilege principle.
* Bilingual workshop documentation is available with step-by-step deployment instructions.
* Evidence screenshots are available for AWS services, API testing results, web interface, logs, and email alerts.
* A cleanup section is available to remove AWS resources after completion.
* The system is evaluated based on the six pillars of the AWS Well-Architected Framework.

---

## 17. Risks and Mitigation

### 17.1. Technical Risks

Possible risks during implementation include:

* Incorrect IAM configuration may prevent Lambda from accessing DynamoDB, SNS, or Bedrock.
* API Gateway may not be integrated correctly with Lambda.
* Cognito token may be invalid during API testing.
* The frontend may call the wrong API endpoint or miss the token.
* Expense calculation logic may be incorrect.
* SNS email subscription may not be confirmed.
* Bedrock may return errors or fail to respond.
* CloudWatch Logs may generate cost if logs are retained for too long.
* Costs may be incurred if resources are not cleaned up after the demo.

### 17.2. Mitigation Plan

* Use CloudWatch Logs to debug Lambda errors.
* Configure IAM permissions based on the least privilege principle.
* Test each API with Postman before integrating with the frontend.
* Confirm SNS email subscription before testing alerts.
* Validate JWT tokens before calling API Gateway.
* Limit the number of AI calls during the demo.
* Configure CloudWatch log retention to 7 or 14 days.
* Create AWS Budgets to monitor cost.
* Clean up all resources after project completion.
* Do not use Bedrock Provisioned Throughput.
* Do not use NAT Gateway unless it is strictly required.

---

## 18. Cost Estimation

The project mainly uses serverless and pay-as-you-go services, so the expected cost is low. The maximum target cost for the entire implementation period of about three months is 200 USD.

Cost control plan:

* Use Lambda, API Gateway, and DynamoDB at demo scale.
* Deploy the frontend using Amazon S3 Static Website Hosting.
* Call Amazon Bedrock only when category suggestion or spending insight generation is needed.
* Do not use RDS, ECS, NAT Gateway, or Load Balancer.
* Do not use Bedrock Provisioned Throughput, batch inference, or fine-tuning.
* Set AWS Budget to about 60 USD per month.
* Configure cost alerts at 50%, 80%, and 100%.
* Configure CloudWatch log retention properly.
* Clean up all resources after completion.

**Note:** AWS Budgets only sends cost alerts. It does not automatically stop resources. Therefore, the project must include a clear cleanup checklist.

---

## 19. Implementation Roadmap

The project is planned to be implemented over 12 weeks as follows:

| Week    | Implementation Content                                                      | Expected Result                                                                                      |
| ------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Week 1  | Analyze requirements and define the project scope                           | Scope and project objectives are completed                                                           |
| Week 2  | Design AWS architecture, APIs, and data model                               | Architecture diagram, API list, and data model are available                                         |
| Week 3  | Create code repository, project structure, and initial technical documents  | Source code structure and documents such as requirements, architecture, and data model are available |
| Week 4  | Configure Amazon Cognito                                                    | Users can register, log in, and obtain tokens                                                        |
| Week 5  | Create DynamoDB tables and IAM roles                                        | Transactions, Budgets, and AlertEvents tables are created with Lambda IAM role                       |
| Week 6  | Build Lambda functions and APIs for transactions                            | Transaction APIs work correctly                                                                      |
| Week 7  | Build Lambda functions and APIs for budgets                                 | Budget APIs work correctly                                                                           |
| Week 8  | Integrate SNS and budget overrun alerts                                     | Email alerts are sent successfully                                                                   |
| Week 9  | Integrate Amazon Bedrock for AI features                                    | AI category suggestion and spending insights work                                                    |
| Week 10 | Build the web frontend and integrate it with API Gateway                    | Users can use the main features through the web interface                                            |
| Week 11 | Configure CloudWatch and test the system using Postman and frontend         | Test cases, test results, logs, and evidence screenshots are available                               |
| Week 12 | Complete workshop documentation, demo video, cleanup, and cost optimization | Report, demo video, and resource cleanup are completed                                               |

---

## 20. Resource Cleanup Plan

After completing the demo and report, AWS resources must be reviewed and cleaned up to avoid unnecessary costs.

Resources to check:

* Amazon S3 bucket used for the frontend.
* Amazon Cognito User Pool.
* API Gateway REST API.
* AWS Lambda functions.
* DynamoDB tables.
* SNS topics and subscriptions.
* CloudWatch log groups.
* IAM roles and policies created for the project.
* AWS Budgets if no longer needed.

Resources not used in the project, such as NAT Gateway, RDS, ECS, Load Balancer, EC2, or Bedrock Provisioned Throughput, should not be created unless they are absolutely required.

---

## 21. Conclusion

The project “AI-Powered Personal Finance Budget Alert System on AWS” is a practical use case for building an application on AWS using a serverless architecture. The project not only provides finance management APIs but also includes a simple web frontend for users, authentication with Cognito, data storage with DynamoDB, email alerts with SNS, monitoring with CloudWatch, and AI-powered spending analysis using Amazon Bedrock.

This project is suitable for the learning objectives and requirements of building an application on a cloud platform. In addition to business features, the project also focuses on cost control, security, operational excellence, reliability, performance efficiency, sustainability, and resource cleanup based on the AWS Well-Architected Framework.
