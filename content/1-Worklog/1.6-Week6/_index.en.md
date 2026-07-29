---
title: "Week 6 Worklog"
date: 2026-07-24
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

# WEEK 6: SECURITY, POST-MEETING WORKFLOWS, AND AI DATA SOURCES

## Tasks to be completed this week

| Day | Tasks | Start date | Completion date | References |
|---|---|---|---|---|
| Monday | - Learn about IAM Permission Boundaries.<br>- Learn about IAM Policy Conditions and the explicit deny principle.<br>- Analyze least-privilege permissions for the API Lambda, Reminder Lambda, and AI Worker.<br>- Review access permissions for DynamoDB, Amazon S3, and integrated services. | 20/07/2026 | 20/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[AWS IAM Documentation](https://docs.aws.amazon.com/iam/) |
| Tuesday | - Learn about AWS Security Hub, AWS WAF, and AWS KMS.<br>- Learn about AWS Secrets Manager and Parameter Store.<br>- Review Amazon Cognito and JWT authentication.<br>- Learn about GuardDuty, Macie, and Amazon S3 security principles. | 21/07/2026 | 21/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[AWS Security Documentation](https://docs.aws.amazon.com/security/) |
| Wednesday | - Design the Reminder workflow using Amazon EventBridge Scheduler.<br>- Design the Reminder Lambda and in-app notification flow.<br>- Define Amazon SES as an additional email notification channel.<br>- Design retry, idempotency, and conditions for skipping canceled Meetings.<br>- Analyze Meeting Minutes, Decisions, and Action Items. | 22/07/2026 | 22/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
| Thursday | - Analyze Tasks and Dashboards.<br>- Define Task states as `TODO`, `DOING`, and `DONE`.<br>- Design Task History and optimistic versioning.<br>- Define the Personal Dashboard and Group Dashboard.<br>- Review the DynamoDB v2 model consisting of five physical tables. | 23/07/2026 | 23/07/2026 | [CampusMeet DynamoDB Data Model](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md)<br>[CampusMeet Team Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-trien-khai-nhom.md) |
| Friday | - Design S3 presigned uploads and complete-upload verification.<br>- Define Attachment status, MIME type, file size, and checksum validation.<br>- Analyze Consent, Recording metadata, and asynchronous AIJob processing.<br>- Review the target AWS deployment architecture.<br>- Update the Vietnamese and English Proposals and complete the Week 6 Worklog. | 24/07/2026 | 24/07/2026 | [CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |

## Week 6 results

- Understood the role of IAM Permission Boundaries.
- Understood how IAM Policy Conditions are used.
- Understood the explicit deny principle and the principle of least privilege.
- Analyzed the permissions required for the API Lambda, Reminder Lambda, and AI Worker.
- Understood the general roles of Security Hub, WAF, KMS, GuardDuty, and Macie.
- Understood how Secrets Manager and Parameter Store protect server-side configuration.
- Reviewed how Cognito issues JWTs and how API Gateway validates tokens.
- Understood Amazon S3 security principles, including Block Public Access, encryption, lifecycle policies, and presigned URLs.
- Designed the Reminder workflow:

> The Administrator configures a reminder time  
> → The backend validates the scheduled time  
> → EventBridge Scheduler creates a one-time schedule  
> → The Reminder Lambda is invoked  
> → The system verifies that the Meeting has not been canceled  
> → An in-app notification is created  
> → The system attempts to send an email through Amazon SES  
> → The processing status is recorded

- Defined in-app notifications as mandatory and email notifications as an additional channel.
- Designed Meeting Minutes containing Summary, Discussion, Decision, and Action Item sections.
- Defined that an Action Item can be converted into a Task after confirmation.
- Defined that a Task contains Title, Description, Assignee, Due Date, Priority, Status, Source Meeting, and Version.
- Defined the following Task states:

> `TODO`  
> `DOING`  
> `DONE`

- Defined the Personal Dashboard and Group Dashboard.
- Finalized the DynamoDB v2 model with five physical tables:

> `identity`  
> `collaboration`  
> `meeting-data`  
> `task-data`  
> `ai-work`

- Designed the secure upload workflow:

> The frontend requests an upload  
> → The API validates the JWT and group membership  
> → An Attachment with the `PENDING_UPLOAD` state is created  
> → A short-lived presigned URL is returned  
> → The browser uploads the file directly to Amazon S3  
> → The frontend calls the complete-upload API  
> → The backend uses `HeadObject` to verify the uploaded object  
> → MIME type, file size, checksum, and object key are validated  
> → The Attachment changes to `READY` or `REJECTED`  
> → An AIJob is created when appropriate

- Defined that binary files must not be transferred through API Gateway or Lambda payloads.
- Defined that Consent is required before recording or live transcription begins.
- Defined the following AIJob states:

> `QUEUED`  
> `PROCESSING`  
> `COMPLETED`  
> `FAILED`  
> `CANCELLED`

- Reviewed and improved the description of the target AWS deployment architecture.
- Updated the CampusMeet Proposal in both Vietnamese and English.

## Challenges encountered

- Week 6 included several modules that depend on Group, Membership, and Meeting.
- The Reminder workflow must prevent duplicate notifications when Lambda executions are retried.
- Uploaded files, recordings, and transcripts contain sensitive data.
- IAM Policies must provide enough permissions for the system to operate without granting excessive access.
- Large files are not suitable for transfer through API Gateway.
- AIJob processing must be asynchronous and have clearly defined states.
- Consent must be recorded before capturing audio data.
- The previous model contains 17 legacy DynamoDB tables that cannot be deleted immediately.
- The documentation must clearly distinguish between implemented components, incomplete components, and the target architecture.
- Reminder, Minutes, Task, Dashboard, Upload, and AIJob still require complete application services, repositories, and integration tests.

## Solutions

- Divide the features into small vertical slices.
- Merge shared contracts before implementing large frontend and backend changes.
- Verify membership and role permissions in every group-related API.
- Use EventBridge Scheduler for reminders scheduled at specific times.
- Use idempotency and conditional writes to prevent duplicate notifications.
- Use presigned URLs so that browsers upload files directly to Amazon S3.
- Use `HeadObject` to verify files after upload.
- Apply a MIME type allowlist, file-size limits, and checksum validation.
- Do not store binary files in DynamoDB.
- Do not write tokens, secrets, complete transcripts, or active presigned URLs to logs.
- Use AIJob and AWS Step Functions for long-running tasks.
- Audit and back up the legacy DynamoDB tables before cleanup.
- Mark a feature as complete only when source code, tests, deployment outputs, and CloudWatch Logs confirm that it works.

## Plan for the following week

- Conduct a Vietnamese live-transcription technical spike.
- Complete the Attachment and complete-upload workflow.
- Implement Consent and Recording metadata.
- Build the LiveTranscriptionSession workflow.
- Store final transcript segments by sequence.
- Build the transcript editing interface.
- Prepare AWS Step Functions and the AI Worker.
- Perform ingestion for one Meeting.
- Prepare the chatbot, RAG, and citation features.
- Test prevention of unauthorized cross-group data retrieval.