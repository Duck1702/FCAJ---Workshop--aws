---

title: "2. Proposal"
date: 2026-06-15
weight: 2
chapter: false
------------

## 1. Project Name

**AI-Powered Personal Finance Budget Alert System on AWS**

Vietnamese name: **Hệ thống quản lý tài chính cá nhân, phân tích AI và cảnh báo vượt ngân sách trên AWS**

## 2. Project Overview

This project aims to build a personal finance management system using a serverless architecture on Amazon Web Services. The system allows users to manage income and expense transactions, set monthly budgets, receive email alerts when spending exceeds the budget, and use AI to suggest transaction categories and generate monthly spending insights.

The project uses AWS services such as Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon SNS, Amazon CloudWatch, Amazon Bedrock, and AWS IAM. The goal is to build a practical cloud-native system that is low-cost, scalable, easy to monitor, and enhanced with AI capabilities to support better personal financial decisions.

## 3. Project Purpose

The main purpose of this project is to help users monitor and control their personal finances more proactively. Instead of only recording transactions, the system helps users detect overspending and provides AI-generated insights to better understand their spending behavior.

From a technical perspective, this project provides hands-on experience in building a complete serverless system on AWS, including user authentication, REST API development, Lambda-based business logic, NoSQL data storage, email notification, CloudWatch monitoring, and AI integration using Amazon Bedrock.

## 4. Target Users

The system is designed for the following user groups:

* Students who want to track monthly spending.
* Employees who want to manage personal income and expenses.
* Personal users who want to receive alerts when spending exceeds a budget.
* Users who want simple AI-generated insights about their spending behavior.
* AWS learners who want to build a serverless application with authentication, API, database, notification, monitoring, and AI features.

## 5. Problem Statement

In real life, many users do not track their spending regularly, do not categorize transactions clearly, and do not know when their spending exceeds the monthly budget. This can lead to poor personal financial control.

In addition, traditional systems often require servers, relational databases, and fixed operational costs. For a learning project, a serverless architecture helps reduce cost, minimize infrastructure management, and still provide scalability.

This project addresses the following problems:

* Managing personal income and expense transactions.
* Setting and tracking monthly budgets.
* Sending alerts when spending exceeds the budget.
* Suggesting transaction categories using AI.
* Generating spending insights using AI.
* Recording logs, monitoring errors, and controlling deployment costs.

## 6. Project Objectives

The objectives of the project are:

* Build a REST API for personal finance management.
* Allow users to create, view, update, and delete transactions.
* Allow users to set monthly budgets.
* Automatically calculate total spending and compare it with the budget.
* Send email alerts when total spending exceeds the budget.
* Use AI to suggest transaction categories based on transaction descriptions.
* Use AI to generate monthly spending insights.
* Authenticate users using Amazon Cognito.
* Store data using Amazon DynamoDB.
* Handle business logic using AWS Lambda.
* Expose APIs through Amazon API Gateway.
* Record logs and monitor the system using Amazon CloudWatch.
* Apply IAM permissions based on the principle of least privilege.
* Control the total project cost within a maximum budget of 200 USD during the implementation period.
* Create bilingual workshop documentation with step-by-step deployment instructions.

## 7. Project Scope

Within the project scope, the system focuses on the following features:

* User registration and login.
* Create income or expense transactions.
* View transaction lists.
* Update transaction information.
* Delete transactions.
* Set monthly budgets.
* Check total spending against the budget.
* Send email alerts when spending exceeds the budget.
* Suggest transaction categories using AI.
* Generate spending insights using AI.
* Record logs and monitor the system.
* Test APIs using Postman.

The project does not focus on building a full user interface. Functional testing and demonstration can be performed using Postman, AWS Console, and real email notifications.

## 8. AWS Services Used

The planned AWS services include:

* **Amazon Cognito:** manages user registration, login, and authentication.
* **Amazon API Gateway:** creates REST API endpoints for users or Postman to call the system.
* **AWS Lambda:** handles business logic for transactions, budgets, alerts, and AI features.
* **Amazon DynamoDB:** stores transactions, budgets, and alert history.
* **Amazon SNS:** sends email alerts when spending exceeds the budget.
* **Amazon CloudWatch:** records logs, monitors metrics, and supports error tracking.
* **Amazon Bedrock:** provides AI capabilities for category suggestions and spending insights.
* **AWS IAM:** manages permissions between Lambda and other AWS services.
* **AWS Budgets:** monitors project cost and sends cost alerts.

## 9. Solution Architecture

The figure below illustrates the overall system architecture:

![System architecture overview](/FCAJ---Workshop--aws/images/2-Proposal/architecture-overview_eng.png)

The main workflow of the system is as follows:

1. Users sign up or sign in through Amazon Cognito.
2. After successful authentication, users receive a JWT token.
3. Users send requests to Amazon API Gateway with the authentication token.
4. API Gateway forwards valid requests to AWS Lambda.
5. Lambda handles transaction, budget, and AI-related business logic.
6. Lambda reads and writes data to Amazon DynamoDB.
7. When users provide transaction descriptions, Lambda can call Amazon Bedrock to suggest categories.
8. When users request spending analysis, Lambda summarizes data and calls Amazon Bedrock to generate insights.
9. If total spending exceeds the budget, Lambda sends a notification to Amazon SNS.
10. Amazon SNS sends an email alert to the user.
11. Amazon CloudWatch records logs, metrics, and supports error monitoring.
12. AWS IAM Role grants Lambda the required permissions to access DynamoDB, SNS, CloudWatch, and Bedrock.

## 10. Expected Data Model

The system is expected to use three main DynamoDB tables.

### Transactions Table

Stores user income and expense transactions.

```text
userId
transactionId
type
category
amount
description
createdAt
updatedAt
```

Description:

* **userId:** user identifier.
* **transactionId:** transaction identifier.
* **type:** transaction type, either income or expense.
* **category:** transaction category.
* **amount:** transaction amount.
* **description:** transaction description.
* **createdAt:** transaction creation time.
* **updatedAt:** transaction update time.

### Budgets Table

Stores monthly budget information.

```text
userId
month
budgetLimit
currentExpense
updatedAt
```

Description:

* **userId:** user identifier.
* **month:** the month to which the budget applies.
* **budgetLimit:** monthly budget limit.
* **currentExpense:** current total expense.
* **updatedAt:** last update time.

### AlertEvents Table

Stores budget alert history.

```text
alertId
userId
month
budgetLimit
currentExpense
status
createdAt
```

Description:

* **alertId:** alert identifier.
* **userId:** user identifier.
* **month:** the month in which the alert occurs.
* **budgetLimit:** configured budget limit.
* **currentExpense:** total spending at the alert time.
* **status:** alert sending status, such as SENT or FAILED.
* **createdAt:** alert creation time.

## 11. Expected APIs

The main APIs of the system include:

| Method | Endpoint           | Function                              |
| ------ | ------------------ | ------------------------------------- |
| POST   | /transactions      | Create a new transaction              |
| GET    | /transactions      | Get transaction list                  |
| GET    | /transactions/{id} | Get transaction details               |
| PUT    | /transactions/{id} | Update a transaction                  |
| DELETE | /transactions/{id} | Delete a transaction                  |
| POST   | /budgets           | Set a monthly budget                  |
| GET    | /budgets           | Get the current budget                |
| POST   | /budgets/check     | Check budget status                   |
| POST   | /ai/categorize     | Suggest transaction category using AI |
| GET    | /ai/insights       | Generate spending insights using AI   |

## 12. AI Features

The project uses AI for two main features.

### 12.1. AI Category Suggestion

When users enter a transaction description, the system can use AI to suggest a suitable category.

Examples:

```text
Description: "chicken rice lunch"
Suggested category: Food

Description: "motorbike fuel"
Suggested category: Transport

Description: "June salary"
Suggested category: Salary
```

### 12.2. AI Spending Insights

Based on transaction and budget data, the system generates short spending insights for users.

Example:

```text
This month, your highest spending categories are Food and Shopping.
Your current spending has exceeded the budget by 12%.
You should reduce non-essential expenses in the next week.
```

## 13. Testing and Evaluation

The system will be tested using Postman and AWS Console. Expected test cases include:

* Sign in and obtain a token from Amazon Cognito.
* Call APIs with a valid token.
* Call APIs without a token.
* Create an income or expense transaction.
* Retrieve the transaction list.
* Update a transaction.
* Delete a transaction.
* Set a monthly budget.
* Create multiple expense transactions to exceed the budget.
* Verify the email alert sent through Amazon SNS.
* Call the AI category suggestion API.
* Call the AI spending insight API.
* Verify data stored in DynamoDB.
* Check logs in CloudWatch.
* Check error responses when input data is invalid.

## 14. Expected Outcomes

After completion, the project is expected to deliver:

* A working serverless REST API system on AWS.
* Users can manage personal income and expense transactions.
* Users can set budgets and receive alerts when spending exceeds the budget.
* The system can suggest transaction categories using AI.
* The system can generate monthly spending insights using AI.
* Data is stored in DynamoDB.
* APIs are protected by Amazon Cognito.
* Email alerts are sent through Amazon SNS.
* CloudWatch records logs and supports system monitoring.
* Bilingual workshop documentation with step-by-step deployment instructions.
* Screenshots of AWS services, API testing results, logs, and email alerts.
* A cleanup section to remove AWS resources after completion.

## 15. Risks and Mitigation

Some potential risks during implementation include:

* Incorrect IAM permissions may prevent Lambda from accessing DynamoDB, SNS, or Bedrock.
* API Gateway may not be integrated correctly with Lambda.
* Cognito tokens may be invalid during API testing.
* Spending calculation logic may be inaccurate.
* SNS email subscription may not be confirmed.
* AI cost may increase if Amazon Bedrock is called too frequently.
* CloudWatch Logs may incur cost if logs are retained for too long.
* Costs may be incurred if resources are not cleaned up after the demo.

Mitigation plans:

* Use CloudWatch Logs to debug Lambda errors.
* Configure IAM permissions based on the principle of least privilege.
* Test each API using Postman before testing the full workflow.
* Confirm SNS email subscription before testing alerts.
* Limit the number of AI calls during demo and testing.
* Set CloudWatch log retention to 7 or 14 days.
* Create AWS Budgets to monitor cost.
* Clean up all resources after completing the project.

## 16. Estimated Cost

The project mainly uses serverless and pay-as-you-go services, so the expected cost is low. The maximum target cost for the entire implementation period of around three months is **200 USD**.

Cost control plan:

* Use Lambda, API Gateway, and DynamoDB at demo scale.
* Call Amazon Bedrock only when category suggestion or insight generation is needed.
* Do not use RDS, ECS, NAT Gateway, or Load Balancer.
* Set an AWS Budget of around 60 USD per month.
* Configure cost alerts at 50%, 80%, and 100%.
* Clean up all resources after completion.

## 17. Implementation Roadmap

The project is planned for 12 weeks:

| Week    | Implementation tasks                                            | Expected result                                              |
| ------- | --------------------------------------------------------------- | ------------------------------------------------------------ |
| Week 1  | Analyze requirements and define project scope                   | Project scope and objectives are completed                   |
| Week 2  | Design AWS architecture, APIs, and data model                   | Architecture diagram, API list, and data model are completed |
| Week 3  | Create code repository and project structure                    | Initial source code structure is prepared                    |
| Week 4  | Configure Amazon Cognito                                        | Users can sign up, sign in, and obtain tokens                |
| Week 5  | Create DynamoDB tables and IAM Role                             | Transactions, Budgets, and AlertEvents tables are created    |
| Week 6  | Build Lambda functions and APIs for transactions                | Transaction APIs work                                        |
| Week 7  | Build Lambda functions and APIs for budgets                     | Budget APIs work                                             |
| Week 8  | Integrate SNS and budget alert logic                            | Email alerts are sent successfully                           |
| Week 9  | Integrate Amazon Bedrock for AI features                        | AI category suggestion and spending insights work            |
| Week 10 | Configure CloudWatch Logs, Metrics, and Alarms                  | Logs and monitoring metrics are available                    |
| Week 11 | Test the full system using Postman                              | Test cases, results, and screenshots are completed           |
| Week 12 | Complete workshop documentation, cleanup, and cost optimization | Report is completed and unused resources are deleted         |

## 18. Conclusion

The “AI-Powered Personal Finance Budget Alert System on AWS” project is a practical use case for learning AWS serverless services. The project does not only build financial management APIs, but also combines user authentication, data storage, email alerts, system monitoring, and AI-powered spending analysis. This implementation is suitable for the learning objectives, project requirements, and cost constraints within a three-month implementation period.
