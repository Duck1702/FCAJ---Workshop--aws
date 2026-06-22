---

title: "2. Proposal"
date: 2026-06-15
weight: 2
chapter: false
--------------

# Project Proposal

## 1. Project Name

**Serverless Personal Finance API & Monitoring System on AWS**

Vietnamese name: **Hệ thống API quản lý tài chính cá nhân và giám sát trên AWS**

## 2. Project Overview

This project aims to build a personal finance management API using a serverless architecture on Amazon Web Services. The system allows users to manage income and expense transactions, categorize transactions, track personal budgets, and receive alerts when their total spending exceeds a predefined limit.

The project follows a cloud-native approach by using AWS services such as Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon Cognito, Amazon CloudWatch, and Amazon SNS. The goal is to build a scalable, low-cost, easy-to-deploy, and easy-to-monitor system based on a practical use case.

## 3. Reason for Choosing the Topic

Personal finance management is a common need in daily life. However, many users still have difficulty tracking expenses, categorizing transactions, and controlling their budgets. In addition, when building a traditional backend system, developers usually need to manage servers, configure environments, monitor errors, and optimize operational costs.

Therefore, this project uses a serverless architecture on AWS to reduce infrastructure management effort, optimize cost, and improve scalability. Through this project, students can practice important AWS services, understand how to design a cloud-native system, and create a workshop document based on a real deployment process.

## 4. Problem Statement

The project focuses on solving the following problems:

* Users need a system to store and manage personal financial transactions.
* Transaction data must be stored securely, queried efficiently, and scaled easily.
* APIs need an authentication mechanism to protect user data.
* The system needs logs and metrics to monitor its operation.
* Users need alerts when spending exceeds the budget limit.
* Operational costs should be minimized by using a serverless model.
* A detailed workshop document is needed so that others can reproduce the project.

## 5. Project Objectives

The main objectives of the project are:

* Build a REST API for personal finance transaction management.
* Allow users to create, view, update, and delete transactions.
* Store transaction data using Amazon DynamoDB.
* Authenticate users using Amazon Cognito.
* Handle business logic using AWS Lambda.
* Expose APIs through Amazon API Gateway.
* Record logs, monitor metrics, and create alerts using Amazon CloudWatch.
* Send email alerts using Amazon SNS when spending exceeds the budget.
* Create bilingual workshop documentation with step-by-step deployment instructions.
* Test the APIs and clean up AWS resources after completion to avoid unnecessary costs.

## 6. Project Scope

Within the scope of this project, the system focuses on the following main features:

* User registration and login.
* Create income or expense transactions.
* View transaction lists.
* Update transaction information.
* Delete transactions.
* Store transaction data per user.
* Set a budget limit.
* Send alerts when total spending exceeds the budget.
* Monitor system logs and metrics.

The project does not focus on building a complete user interface. Instead, it prioritizes backend implementation, AWS architecture, monitoring, testing, and workshop documentation.

## 7. AWS Services Used

The AWS services planned for this project include:

* **Amazon API Gateway:** creates REST APIs for clients or Postman to send requests to the backend.
* **AWS Lambda:** handles business logic such as creating, updating, deleting, and querying transactions.
* **Amazon DynamoDB:** stores personal finance transaction data.
* **Amazon Cognito:** manages user registration, login, and authentication.
* **Amazon CloudWatch:** monitors logs, metrics, errors, and system status.
* **Amazon SNS:** sends email alerts when spending exceeds the budget.
* **AWS IAM:** grants Lambda functions the required permissions to access AWS resources securely.

## 8. Solution Architecture

The main workflow of the system is as follows:

Users register and log in through Amazon Cognito. After successful authentication, users receive a token to call the APIs. Requests are sent to Amazon API Gateway. API Gateway verifies authentication and forwards the requests to AWS Lambda. Lambda handles the business logic and reads or writes data to Amazon DynamoDB. During processing, logs and metrics are recorded in Amazon CloudWatch. When total spending exceeds the budget limit, Lambda sends a notification to Amazon SNS, which then sends an email alert to the user.

General architecture:

![System architecture overview](/FCAJ---Workshop--aws/images/2-Proposal/architecture-overview_en.png)

Main workflow:
1. Users authenticate through Amazon Cognito.
2. Users call APIs through Amazon API Gateway.
3. API Gateway forwards requests to AWS Lambda.
4. Lambda processes business logic and stores data in DynamoDB.
5. CloudWatch is used for logging and monitoring.
6. When spending exceeds the budget, SNS sends an email alert.

## 9. Expected Data Model

Expected DynamoDB table: **Transactions**

Main attributes:

```text
userId
transactionId
type
category
amount
note
createdAt
updatedAt
```

Description:

* **userId:** user identifier.
* **transactionId:** transaction identifier.
* **type:** transaction type, either income or expense.
* **category:** transaction category, such as food, transport, salary, or shopping.
* **amount:** transaction amount.
* **note:** transaction note.
* **createdAt:** transaction creation time.
* **updatedAt:** transaction update time.

## 10. Implementation Plan

The project is planned for 12 weeks:

| Week    | Planned tasks                                                                 |
| ------- | ----------------------------------------------------------------------------- |
| Week 1  | Study the requirements, select the topic, and prepare the workshop repository |
| Week 2  | Write the proposal and identify the AWS services to be used                   |
| Week 3  | Design the system architecture and data model                                 |
| Week 4  | Create the DynamoDB table and configure IAM roles                             |
| Week 5  | Build the Lambda function for creating transactions                           |
| Week 6  | Build the remaining APIs and integrate API Gateway                            |
| Week 7  | Configure Amazon Cognito for user authentication                              |
| Week 8  | Test APIs using Postman                                                       |
| Week 9  | Configure CloudWatch Logs, Metrics, and Alarms                                |
| Week 10 | Configure Amazon SNS for sending alerts                                       |
| Week 11 | Complete workshop documentation and add screenshots                           |
| Week 12 | Review, optimize cost, write self-evaluation, and clean up resources          |

## 11. Testing and Evaluation

The project will be tested using Postman and AWS Console. Some expected test cases include:

* Call the API to create a transaction successfully.
* Call the API to retrieve the transaction list.
* Call the API to update a transaction.
* Call the API to delete a transaction.
* Call the API without an authentication token.
* Call the API with invalid input data.
* Verify that data is stored in DynamoDB.
* Check Lambda logs in CloudWatch.
* Verify alerts when spending exceeds the budget.
* Verify that email alerts are sent through Amazon SNS.

## 12. Expected Outcomes

After completion, the project is expected to deliver:

* A working serverless REST API system on AWS.
* Users can manage personal finance transactions.
* Data is stored in Amazon DynamoDB.
* APIs are protected by Amazon Cognito.
* The system has logs, metrics, and alarms through Amazon CloudWatch.
* Email alerts are sent using Amazon SNS.
* Bilingual workshop documentation with step-by-step deployment instructions.
* A cleanup section to remove AWS resources after completion.

## 13. Risks and Mitigation

Some potential risks during implementation include:

* Incorrect IAM permissions may prevent Lambda from accessing DynamoDB.
* API Gateway may not be integrated correctly with Lambda.
* Cognito tokens may be invalid during API testing.
* CloudWatch Alarms may not be triggered under the expected conditions.
* Input data may not follow the expected format.
* Costs may be incurred if resources are not deleted after the demo.

Mitigation plans:

* Use CloudWatch Logs to debug Lambda errors.
* Apply the principle of least privilege when configuring IAM permissions.
* Test each API separately using Postman.
* Verify Cognito Authorizer configuration in API Gateway.
* Create an AWS Budget or monitor the Billing Dashboard.
* Clean up all resources after the project is completed.

## 14. Estimated Cost

The project mainly uses serverless services, so the expected cost is low. With demo-scale usage and a small number of requests, services such as AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon CloudWatch, and Amazon SNS can be used within the free tier or with very low cost.

After the demo is completed, all unnecessary resources will be deleted to avoid unexpected charges.

## 15. Conclusion

The “Serverless Personal Finance API & Monitoring System on AWS” project helps students practice building a complete serverless backend system on AWS. The project does not only focus on creating APIs, but also emphasizes security, monitoring, alerts, testing, cost optimization, and resource cleanup. This is a practical use case that matches the learning objectives and requirements of the First Cloud AI Journey program.
