---
title: "Week 2 Worklog"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

# WEEK 2: BUILDING THE AWS FOUNDATION AND CAMPUSMEET PROJECT STRUCTURE

## Tasks to be completed this week

| Day | Tasks | Start date | Completion date | References |
|---|---|---|---|---|
| Monday | - Learn about IAM Users, IAM Groups, IAM Roles, and IAM Policies.<br>- Learn the principle of least privilege.<br>- Practice verifying AWS identity using the AWS CLI.<br>- Agree that access keys must not be shared among team members. | 22/06/2026 | 22/06/2026 | [AWS IAM](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| Tuesday | - Learn about VPCs, subnets, route tables, Internet Gateways, NAT Gateways, and Security Groups.<br>- Analyze why the CampusMeet MVP does not require a custom VPC or NAT Gateway.<br>- Learn about Amazon EC2 and the cost of continuously running resources. | 23/06/2026 | 23/06/2026 | [Amazon VPC and EC2](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| Wednesday | - Learn about Amazon S3, buckets, objects, Block Public Access, encryption, and lifecycle policies.<br>- Learn about Amazon RDS and Amazon DynamoDB.<br>- Compare relational databases and NoSQL databases based on access patterns. | 24/06/2026 | 24/06/2026 | [Explore AWS Services](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| Thursday | - Build the CampusMeet monorepo structure, including `apps`, `services`, `packages`, `infra`, `scripts`, and `docs`.<br>- Select React and TypeScript for the frontend and Node.js with TypeScript for the backend.<br>- Prepare shared types and the API contract. | 25/06/2026 | 25/06/2026 | [CampusMeet Repository](https://github.com/Ngct253/CampusMeet) |
| Friday | - Review the Cognito authentication foundation.<br>- Check protected routes, the API client, and the `/health` endpoint.<br>- Learn about GitHub Actions, AWS SAM, and AWS CloudFormation.<br>- Complete the Week 2 Worklog. | 26/06/2026 | 26/06/2026 | [CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md)<br>[AWS SAM](https://docs.aws.amazon.com/serverless-application-model/) |

## Week 2 results

- Understood the roles of IAM Users, Groups, Roles, and Policies.
- Understood the principle of least privilege and the importance of not sharing credentials.
- Learned the basic components of Amazon VPC.
- Understood the differences between Amazon EC2 and a serverless architecture.
- Understood how Amazon S3 stores and protects data.
- Distinguished between Amazon RDS and Amazon DynamoDB at a basic level.
- Built the monorepo structure for CampusMeet.
- Defined the boundaries among the frontend, backend, shared package, and infrastructure.
- Reviewed the Cognito authentication foundation and the API health check.
- Established an initial foundation for continuous integration and Infrastructure as Code.

## Challenges encountered

- Multiple team members may modify the same shared or infrastructure files.
- It is easy to confuse the existence of a template with the successful deployment of actual AWS resources.
- The team needs to standardize folder names, data types, and API contracts.
- Credentials must be carefully managed when working on personal computers.
- The DynamoDB repositories and real business APIs are not yet fully implemented.

## Solutions

- Agree on the repository structure before implementing individual features.
- Use shared contracts instead of copying interfaces between the frontend and backend.
- Confirm that a feature is operational only when source code, tests, deployment outputs, and logs are available.
- Each team member should use a separate AWS profile.
- Do not commit secrets or access keys stored in `.env` files.
- All infrastructure changes must be reviewed before deployment.

## Plan for the following week

- Analyze Group, Membership, and Invitation features.
- Define the Member and Administrator roles.
- Design the authorization boundary.
- Study DynamoDB, CloudFront, and CloudWatch in greater depth.
- Design the `collaboration` table and its access patterns.
- Prepare tests for rejecting unauthorized cross-group access.