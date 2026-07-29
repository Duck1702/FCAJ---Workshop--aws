---
title: "Week 3 Worklog"
date: 2026-07-03
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

# WEEK 3: GROUPS, MEMBERSHIPS, AND AUTHORIZATION

## Tasks to be completed this week

| Day | Tasks | Start date | Completion date | References |
|---|---|---|---|---|
| Monday | - Learn about Amazon DynamoDB, including partition keys, sort keys, Global Secondary Indexes, Query, Scan, and on-demand capacity.<br>- Learn how to design data based on access patterns.<br>- Review and compare the current CampusMeet data model. | 29/06/2026 | 29/06/2026 | [DynamoDB](https://cloudjourney.awsstudygroup.com/vi/)<br>[CampusMeet DynamoDB v2](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md) |
| Tuesday | - Learn about Amazon CloudFront and content delivery from Amazon S3.<br>- Learn about Origin Access Control and private S3 origins.<br>- Analyze the target frontend architecture of CampusMeet. | 30/06/2026 | 30/06/2026 | [Amazon CloudFront](https://cloudjourney.awsstudygroup.com/vi/1-explore/)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
| Wednesday | - Learn about CloudWatch Logs, Metrics, Dashboards, and Alarms.<br>- Analyze the metrics that should be monitored for the API, Lambda functions, and authorization processes.<br>- Define the requirement that logs must not contain tokens or secrets. | 01/07/2026 | 01/07/2026 | [Amazon CloudWatch](https://cloudjourney.awsstudygroup.com/vi/1-explore/) |
| Thursday | - Analyze the Group, Membership, and Invitation entities.<br>- Define the Member and Group Administrator roles.<br>- Design the group creation flow in which the creator becomes an Administrator.<br>- Design the invitation acceptance flow.<br>- Identify cases that must return a `403 Forbidden` response. | 02/07/2026 | 02/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |
| Friday | - Design a shared authorization boundary.<br>- Design access patterns for the `collaboration` table.<br>- Define the transaction used to create a Group, Membership, and Audit Event.<br>- Prepare tests for cross-group access and protection of the final Administrator.<br>- Complete the Week 3 Worklog. | 03/07/2026 | 03/07/2026 | [CampusMeet Team Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-trien-khai-nhom.md) |

## Week 3 results

- Understood how DynamoDB organizes data using partition keys and sort keys.
- Distinguished between Query and Scan operations.
- Understood the role of Global Secondary Indexes.
- Understood the benefits of on-demand capacity for systems with unpredictable traffic.
- Understood how CloudFront delivers frontend content from Amazon S3.
- Understood the role of Origin Access Control in protecting a private S3 frontend bucket.
- Learned the basic components of Amazon CloudWatch.
- Identified the Group, Membership, Invitation, and Audit Event entities.
- Defined the Member and Group Administrator roles.
- Defined that the group creator automatically becomes a Group Administrator.
- Designed the shared authorization flow:

> Valid JWT  
> → Read the user identity from trusted token claims  
> → Resolve the related group  
> → Verify active membership  
> → Verify the current role  
> → Verify permission for the requested resource  
> → Perform the business operation

- Designed the group creation transaction containing the Group, Creator Membership, and Audit Event.
- Identified test cases for unauthorized cross-group access.
- Defined the rule that the final Group Administrator cannot be removed or demoted.

## Challenges encountered

- DynamoDB must be designed around access patterns instead of using one table for every entity.
- Membership is a dependency for many other CampusMeet modules.
- Frontend authorization alone is not sufficient to protect application data.
- Without a transaction, the system could create a Group without creating the Administrator Membership.
- Retried requests may create duplicate data when idempotency is not implemented.
- Removing the final Administrator could leave a group without anyone able to manage it.
- The Group and Membership handlers still require complete application services and real DynamoDB repositories.

## Solutions

- Use the `collaboration` table for Groups, Memberships, Invitations, and Audit Events.
- Use a GSI to query the groups associated with a user.
- Do not use Scan for normal business request flows.
- Use a DynamoDB transaction when creating the initial Group and Membership.
- Use conditional writes and idempotency for mutations that may be retried.
- Build a shared authorization boundary for other modules.
- Read `userId` from the JWT instead of trusting a `userId` or role sent by the frontend.
- Verify active membership before reading or modifying group data.
- Write tests confirming that users outside the group receive `403 Forbidden`.
- Mark the module as complete only after the real repository and integration tests are operational.

## Plan for the following week

- Learn about AWS Lambda and Amazon API Gateway.
- Learn about AWS SAM and AWS CloudFormation.
- Analyze Meeting, Agenda, Attendee, and Organizer entities.
- Define the meeting lifecycle.
- Design APIs for creating, updating, canceling, and listing meetings.
- Design access patterns for meeting data.
- Apply the shared authorization boundary to the Meeting API.