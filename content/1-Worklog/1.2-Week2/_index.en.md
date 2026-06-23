---
title: "Week 2 Worklog"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# Week 2: Completing the Proposal, Website Structure, and Module 2

## Tasks to be completed this week

| Day       | Task                                                                                                                                                                                                                                                                                                                                                                | Start date | Completion date | Reference                             |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------- |
| Monday    | - Finalize the project idea: AI-Powered Personal Finance Budget Alert System on AWS.<br>- Define the main system scope: transaction management, budget management, budget overrun alerts, and AI analysis.<br>- Identify the AWS services planned for the system, including Cognito, API Gateway, Lambda, DynamoDB, SNS, CloudWatch, Bedrock, IAM, and AWS Budgets. | 22/06/2026 | 26/06/2026      | AWS Documentation, FCAJ Requirement   |
| Tuesday   | - Complete the Proposal content in both Vietnamese and English.<br>- Add the system architecture overview image to the Proposal section.<br>- Fix menu, header, and sidebar issues on the report website.<br>- Standardize the bilingual structure for Vietnamese and English sections.                                                                             | 23/06/2026 | 26/06/2026      | Hugo Documentation, GitHub Repository |
| Wednesday | - Create README.md for the repository to describe the project on GitHub.<br>- Review the AWS cost control plan before deploying resources.<br>- Complete the learning content in Module 2.<br>- Take notes on important knowledge from Module 2.                                                                                                                    | 24/06/2026 | 26/06/2026      | GitHub, Module 2                      |
| Thursday  | - Prepare technical documentation for the AWS project.<br>- Write initial documents such as requirements, architecture, data model, API endpoints, cost control, and cleanup checklist.                                                                                                                                                                             | 25/06/2026 | 26/06/2026      | AWS Documentation, Project Repository |
| Friday    | - Prepare for the AWS implementation phase in the following week.<br>- Define the implementation order of AWS services: AWS Budget, IAM, Cognito, DynamoDB, Lambda, API Gateway, SNS, CloudWatch, and Bedrock.<br>- Summarize the results of Week 2, complete the Week 2 Worklog, and prepare the plan for Week 3.                                                  | 26/06/2026 | 26/06/2026      | AWS Console, GitHub Repository        |

## Results achieved in Week 2

* Finalized the official project idea and scope.
* Identified the AWS services that will be used in the system.
* Completed the Proposal in both Vietnamese and English.
* Added the architecture overview image to the Proposal section.
* Fixed interface and sidebar issues on the report website.
* Further standardized the bilingual structure of the website.
* Created README.md to describe the project on GitHub.
* Completed the learning content in Module 2.
* Prepared initial technical documentation for the AWS project.
* Built an AWS implementation plan for the following week.

## Evidence for Week 2

### 1. Updated Proposal

![Proposal](/FCAJ---Workshop--aws/images/1-Worklog/week2/proposal.png)

### 2. Hugo Website Running Locally

![Hugo Local Server](/FCAJ---Workshop--aws/images/1-Worklog/week2/hugo_local.png)

### 3. Updated GitHub Repository

![GitHub Repository](/FCAJ---Workshop--aws/images/1-Worklog/week2/github_repo.png)

### 4. Completed Module 2

![Module 2](/FCAJ---Workshop--aws/images/1-Worklog/week2/module-2.png)

## Difficulties encountered

* The old Hugo theme caused errors when running with the newer Hugo version.
* The English sidebar was not fully displayed because some `_index.en.md` files were missing.
* Image paths needed to be checked carefully to ensure that evidence images were displayed correctly.
* AWS cost control needed to be considered carefully before deploying real AWS resources.

## Solutions

* Fixed the Hugo theme template files to make the website run more stably.
* Created additional `_index.en.md` files for the missing English sections.
* Checked the image folder structure inside `static/images`.
* Prepared the cost control document and cleanup checklist before creating AWS resources.

## Plan for next week

* Start deploying the foundational AWS services.
* Configure IAM user and required access permissions.
* Create Amazon Cognito for user management.
* Design and create DynamoDB tables.
* Prepare Lambda functions for transactions and budgets.
* Continue recording implementation evidence for the Workshop section.
