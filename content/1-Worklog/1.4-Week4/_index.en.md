---
title: "Week 4 Worklog"
date: 2026-07-10
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

# WEEK 4: MEETING MANAGEMENT AND SERVERLESS ARCHITECTURE

## Tasks to be completed this week

| Day | Tasks | Start date | Completion date | References |
|---|---|---|---|---|
| Monday | - Learn about AWS Lambda.<br>- Learn about events, context, execution roles, memory, timeout, and cold starts.<br>- Learn how Lambda sends logs to Amazon CloudWatch.<br>- Analyze why CampusMeet uses Lambda instead of a continuously running server. | 06/07/2026 | 06/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/) |
| Tuesday | - Learn about Amazon API Gateway HTTP APIs.<br>- Learn about routes, methods, integrations, stages, CORS, and JWT authorizers.<br>- Analyze the API request flow from React to Lambda.<br>- Learn how API Gateway validates Amazon Cognito JWTs. | 07/07/2026 | 07/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/) |
| Wednesday | - Learn about AWS CloudFormation and AWS SAM.<br>- Learn about templates, resources, parameters, and outputs.<br>- Review the structure of the CampusMeet SAM templates.<br>- Learn the validation, build, change set, deployment, and rollback processes. | 08/07/2026 | 08/07/2026 | [AWS SAM Documentation](https://docs.aws.amazon.com/serverless-application-model/)<br>[CampusMeet AWS Deployment Guide](https://github.com/Ngct253/CampusMeet/blob/main/docs/huong-dan-trien-khai-aws.md) |
| Thursday | - Analyze the Meeting, Agenda, Attendee, and Organizer entities.<br>- Define the meeting lifecycle states.<br>- Define validation rules for meeting start and end times.<br>- Define that attendees must belong to the related group.<br>- Define the Organizer as a meeting-specific responsibility rather than a global role. | 09/07/2026 | 09/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |
| Friday | - Design access patterns for Meeting, Agenda, and Attendee data in the `meeting-data` table.<br>- Define APIs for creating, updating, canceling, and listing meetings.<br>- Prepare test cases for time validation, membership, and authorization.<br>- Review the current Meeting interface and mock data.<br>- Complete the Week 4 Worklog. | 10/07/2026 | 10/07/2026 | [CampusMeet DynamoDB Data Model](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md)<br>[CampusMeet Repository](https://github.com/Ngct253/CampusMeet) |

## Week 4 results

- Understood how AWS Lambda processes requests using an event-driven model.
- Understood the role of a Lambda execution role.
- Understood the effects of memory, timeout, and cold starts.
- Understood the role of API Gateway in a serverless architecture.
- Understood how a JWT authorizer validates a token before invoking Lambda.
- Understood the infrastructure management process using AWS SAM and CloudFormation.
- Recognized that the existence of a template does not prove that the system has been successfully deployed.
- Identified the Meeting, Agenda, Attendee, and Organizer entities.
- Defined the expected meeting lifecycle:

> DRAFT  
> → SCHEDULED  
> → IN_PROGRESS  
> → COMPLETED  
>  
> A meeting may also transition to CANCELLED when it is canceled.

- Defined that the meeting end time must be later than the start time.
- Defined that a scheduled meeting must not have a start time in the past.
- Defined that an Attendee must be an active member of the related group.
- Defined the Organizer as the person selected to organize a specific meeting.
- Designed the following access patterns:

> `PK=MEETING#<meetingId>, SK=META`  
> `PK=MEETING#<meetingId>, SK=ATTENDEE#<userId>`  
> `PK=MEETING#<meetingId>, SK=AGENDA#<order>#<agendaId>`

- Defined that the Meeting API must resolve `meetingId` to `groupId` on the backend before performing authorization checks.

## Challenges encountered

- Meeting is a dependency for the Reminder, Minutes, Task, Google Integration, Transcript, and AI modules.
- The contract must be standardized before multiple team members can implement related features in parallel.
- The backend cannot trust `groupId`, `userId`, or roles submitted by the frontend.
- Meeting updates and cancellations must be idempotent.
- The meeting lifecycle must prevent invalid state transitions.
- Some Meeting screens are still using mock data.
- The Meeting API and real DynamoDB repositories are not yet fully implemented.

## Solutions

- Always query the Meeting record on the backend to obtain a trusted `groupId`.
- Apply the authorization boundary provided by the Membership module.
- Use shared DTOs from `@campusmeet/shared`.
- Use conditional writes when updating the Meeting state.
- Use idempotency keys for operations that may be retried.
- Separate Handler, Middleware, Application Service, Domain, and Repository responsibilities.
- Do not place direct DynamoDB queries inside Handlers.
- Write tests proving that a Meeting in Group A cannot be accessed by a user who only belongs to Group B.
- Consider the meeting flow complete only after integration tests, deployment outputs, and CloudWatch Logs confirm that it works.

## Plan for the following week

- Learn about Operational Excellence on AWS.
- Study advanced CloudWatch features, Amazon SNS, and resource tagging.
- Analyze the Google OAuth Authorization Code Flow.
- Design Google Calendar event creation and Google Meet conference-data requests.
- Define Google synchronization states.
- Design retry and idempotency for Google integration.
- Define fallback methods when Google Meet artifacts are unavailable.