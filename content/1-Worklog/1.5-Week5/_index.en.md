---
title: "Week 5 Worklog"
date: 2026-07-17
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

# WEEK 5: GOOGLE INTEGRATION AND OPERATIONAL OPTIMIZATION

## Tasks to be completed this week

| Day | Tasks | Start date | Completion date | References |
|---|---|---|---|---|
| Monday | - Learn about Operational Excellence on AWS.<br>- Learn about runbooks, automation, deployment reviews, and smoke tests.<br>- Learn about rollback procedures and resource cleanup.<br>- Compare these practices with the CampusMeet deployment guide. | 13/07/2026 | 13/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[CampusMeet AWS Deployment Guide](https://github.com/Ngct253/CampusMeet/blob/main/docs/huong-dan-trien-khai-aws.md) |
| Tuesday | - Study advanced CloudWatch Metrics and Alarms.<br>- Learn about Amazon SNS and operational alert delivery.<br>- Identify metrics that should be monitored, such as API errors, Lambda errors, execution duration, and Google synchronization failures.<br>- Learn how to configure CloudWatch Logs retention. | 14/07/2026 | 14/07/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[Amazon SNS Documentation](https://docs.aws.amazon.com/sns/) |
| Wednesday | - Learn about resource tagging and classification by Project, Environment, and Owner.<br>- Learn about AWS Systems Manager Parameter Store.<br>- Distinguish public configuration from server-side secrets.<br>- Define that Google access tokens and refresh tokens must not be stored in the frontend. | 15/07/2026 | 15/07/2026 | [AWS Systems Manager Documentation](https://docs.aws.amazon.com/systems-manager/)<br>[The First Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| Thursday | - Analyze the Google OAuth Authorization Code Flow.<br>- Design the Google account connection and disconnection flows.<br>- Design the Google Calendar event creation flow.<br>- Learn about `conferenceData.createRequest` and request IDs.<br>- Define `googleEventId` and Google synchronization states. | 16/07/2026 | 16/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet API Contract](https://github.com/Ngct253/CampusMeet/blob/main/docs/api-contract.md) |
| Friday | - Design retry and idempotency for Google integration.<br>- Analyze the synchronization of recordings or transcripts after a meeting.<br>- Define fallback methods using secure uploads or consent-based capture.<br>- Define that the Google Meet Add-on uses the same API and database as CampusMeet Web.<br>- Update the architecture documentation and complete the Week 5 Worklog. | 17/07/2026 | 17/07/2026 | [CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture)<br>[CampusMeet Repository](https://github.com/Ngct253/CampusMeet) |

## Week 5 results

- Understood the principles of Operational Excellence on AWS.
- Understood the roles of runbooks, deployment reviews, smoke tests, rollback procedures, and resource cleanup.
- Understood how CloudWatch Alarms work with Amazon SNS to deliver operational alerts.
- Learned how tags can be used to classify and manage AWS resources.
- Understood the role of Parameter Store in managing server-side configuration.
- Defined that CampusMeet does not build or duplicate Google Meet.
- Defined that Google Meet is the platform where video meetings take place, while CampusMeet manages the surrounding meeting workflow.
- Designed the Google OAuth flow:

> The user chooses to connect Google  
> → The backend creates an OAuth state  
> → The user grants permission  
> → Google redirects to the backend callback  
> → The backend validates the OAuth state  
> → The token or secret reference is stored on the server  
> → The frontend receives only the connection status

- Designed the Google Calendar event creation and conference-data request flow.
- Defined the following synchronization states:

> NOT_REQUESTED  
> PENDING  
> READY  
> FAILED_RETRYABLE  
> ACTION_REQUIRED

- Defined that the Google Meet link is displayed only when the synchronization state is `READY`.
- Recognized that Google Meet artifacts are not always available.
- Defined secure upload or consent-based capture as fallback methods.
- Defined that the Google Meet Add-on does not have a separate backend or database.

## Challenges encountered

- Google integration depends on account configuration, OAuth permissions, and Google Workspace policies.
- A Google Meet link may not be immediately available after a Calendar event is created.
- A recording or transcript may not exist.
- Incorrect retry handling may create duplicate Google Calendar events.
- Google access tokens and refresh tokens are sensitive data.
- The existence of a contract or adapter skeleton does not prove that the integration is operational.
- Real testing depends on having a suitable Google account and the required permissions.

## Solutions

- Always create and store the internal Meeting record before calling the Google API.
- Use a unique request ID to prevent duplicate conference creation.
- Store `googleEventId` so that the existing Calendar event can be updated.
- Do not create a new event only because the previous request remains `PENDING`.
- Store explicit synchronization states so that the frontend can display the current status.
- Store tokens on the server and never return refresh tokens to the browser.
- Do not write Google tokens to CloudWatch Logs.
- Use secure uploads or consent-based capture as fallback methods.
- Limit retry attempts and use exponential backoff where appropriate.
- Mark the integration as complete only after real tests and operational logs confirm that it works.

## Plan for the following week

- Study the Security section of the First Cloud Journey.
- Learn about IAM Permission Boundaries and IAM Policy Conditions.
- Learn about AWS Security Hub, AWS WAF, AWS KMS, and AWS Secrets Manager.
- Design the Reminder and Notification modules.
- Analyze Meeting Minutes, Action Items, Tasks, and Dashboards.
- Design secure S3 presigned uploads.
- Define Consent, Recording metadata, and AIJob processing.
- Review the DynamoDB v2 model and the target AWS architecture.