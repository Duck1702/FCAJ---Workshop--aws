---
title: " Proposal"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# CAMPUSMEET PROJECT 

## Meeting and Team Work Management System with Google Meet Integration on an AWS Serverless Platform


---

# TABLE OF CONTENTS

1. Background and motivation  
2. Solution overview  
3. Objectives and success criteria  
4. Users and authorization  
5. Project scope  
6. AWS solution architecture  
7. Technical design  
8. Security and privacy  
9. Reliability, monitoring, and cost control  
10. Implementation plan  
11. Activities and deliverables  
12. Team responsibilities  
13. Risks and mitigation strategies  
14. Roadmap from the current repository to the target MVP  
15. Acceptance criteria  
16. Expected outcomes  

---

# 1. BACKGROUND AND MOTIVATION

## 1.1. Background

Student teams, academic project groups, and small project teams often use several disconnected tools to organize their work:

- Google Calendar for schedules.
- Google Meet for online meetings.
- Messaging applications for reminders and quick discussions.
- Separate documents for meeting minutes.
- Task boards or chat messages for post-meeting assignments.

Because these tools are separated, information becomes fragmented. Team members may not know which meeting schedule is official, what decisions were made, who is responsible for an action item, or when a task is due.

The post-meeting process is especially difficult to manage. Decisions and action items may remain only in personal notes or chat messages instead of being converted into trackable tasks with an assignee, priority, due date, and status.

## 1.2. Problems to be solved

| Problem | Consequence |
|---|---|
| Meeting schedules, minutes, and tasks are stored in different tools | Team members spend more time searching and may miss important information |
| Google Calendar events and Google Meet links are created manually | Organizers repeat the same steps for every meeting |
| There is no standardized post-meeting process | Decisions are not consistently converted into actionable work |
| There is no shared dashboard | Teams cannot easily track upcoming meetings, due tasks, overdue tasks, or progress |
| Members may join late or miss a meeting | They have difficulty understanding what has already been discussed |
| Documents and transcripts can be long | Users cannot quickly locate evidence for a conclusion |
| AI-generated content may lack evidence | Minutes and task proposals may be incorrect without citations and human review |
| Meeting data may be sensitive | The system requires consent, authorization, retention, and deletion controls |

## 1.3. Motivation

CampusMeet is proposed as a lightweight and controlled workspace for the complete meeting lifecycle:

```text
Preparation
→ Scheduling
→ Reminders
→ Meeting on Google Meet
→ Live transcription
→ Minutes and decisions
→ Tasks
→ Progress tracking
→ Citation-grounded question answering
```

CampusMeet does not replace Google Meet and does not implement its own video conferencing platform.

Google Meet remains the platform where the online meeting takes place. CampusMeet manages the workflow, information, decisions, documents, transcripts, and tasks before, during, and after the meeting.

---

# 2. SOLUTION OVERVIEW

CampusMeet is an independent web application deployed on AWS using a managed serverless architecture.

The system supports the following capabilities:

1. User registration and sign-in through Amazon Cognito.
2. User profile and notification preference management.
3. Group creation and member management.
4. Group-specific Member and Group Administrator roles.
5. Meeting creation, update, cancellation, and lifecycle management.
6. Agenda, organizer, and attendee management.
7. Google OAuth connection.
8. Google Calendar event creation.
9. Google Meet conference link requests through Google Calendar.
10. Transparent Calendar and Meet synchronization status.
11. Up to three reminder times for each meeting.
12. In-application notifications and optional email delivery.
13. Meeting minutes, decisions, and action items.
14. Conversion of action items into trackable tasks.
15. Personal and group dashboards.
16. Secure document and audio upload through Amazon S3 presigned URLs.
17. Live transcription after explicit consent and capture permission.
18. Transcript segments with timestamps, confidence values, language information, and anonymous `Speaker N` labels.
19. Transcript review and editing.
20. Question answering over documents, transcripts, and minutes with citations.
21. Retrieval over one meeting, selected meetings, or all meetings in the same group.
22. AI-generated draft minutes and task proposals.
23. Human preview, correction, and confirmation before any AI-proposed task becomes an official task.
24. Logs, metrics, alarms, and AWS cost monitoring.

CampusMeet Web is the primary client.

A Google Meet Add-on may be implemented as a side panel. The Add-on loads a CampusMeet HTTPS route and uses the same API Gateway, Lambda backend, authentication, authorization, and data model as the web application.

The Add-on does not have:

- A separate backend.
- A separate database.
- Privileged API endpoints.
- Independent business rules.
- Direct access to AWS data stores.

---

# 3. OBJECTIVES AND SUCCESS CRITERIA

## 3.1. Business objectives

| Objective | Expected result |
|---|---|
| Centralized management | Groups, members, meetings, minutes, and tasks are managed in one system |
| Reduced manual work | CampusMeet assists with Calendar event and Google Meet link creation |
| Standardized post-meeting workflow | Decisions and action items become trackable tasks |
| Improved visibility | Members and administrators can see upcoming meetings and task progress |
| Support for late participants | Users can receive a grounded summary based on the live transcript |
| Group knowledge retrieval | Users can ask questions across meetings in the same group |
| Evidence-based AI | Important AI output includes citations or reports insufficient context |
| Complete AWS project evidence | The project includes IaC, deployment, logs, metrics, alarms, cost control, and cleanup |

## 3.2. Technical objectives

The project aims to:

- Use a managed serverless architecture.
- Avoid continuously running servers in the MVP.
- Avoid EC2, ECS, EKS, RDS, VPC, and NAT Gateway for the baseline MVP.
- Process large files and AI jobs asynchronously.
- Avoid sending large binary files through API Gateway or Lambda request payloads.
- Enforce access by `groupId`, membership, role, ACL, and source approval status.
- Prevent cross-group AI retrieval.
- Deploy AWS infrastructure through Infrastructure as Code.
- Support retries, idempotency, and observable failure states.
- Prioritize Vietnamese transcription quality during benchmarking.
- Keep AI output grounded, reviewable, and reversible.

## 3.3. MVP success criteria

The MVP is considered successful when the following conditions are met:

1. A user can register, confirm an account, sign in, and sign out.
2. A user can create a group and becomes its Group Administrator.
3. A Group Administrator can invite and manage members.
4. A user outside a group cannot access the group’s data.
5. A Group Administrator can create, update, and cancel a meeting.
6. A meeting is stored internally even when Google integration fails.
7. Google synchronization status is shown transparently.
8. A Google Meet link is displayed only when synchronization status is `READY`.
9. The system creates and executes valid reminder schedules.
10. Reminder processing always creates an in-application notification.
11. Email failure does not cause the in-application notification to be lost.
12. Meeting minutes can contain decisions and action items.
13. An action item can be converted into a task.
14. An assignee can update a task through `TODO`, `DOING`, and `DONE`.
15. Dashboards show meetings and tasks requiring attention.
16. Files are uploaded directly to S3 and verified by the backend.
17. Live transcription starts only after consent and capture permission.
18. Transcript segments contain timestamps, confidence, language, and anonymous speaker labels.
19. Authorized users can review and edit transcripts.
20. The AI assistant returns citations or `insufficientContext=true`.
21. RAG never returns content from another group.
22. AI creates only drafts and proposals.
23. Tasks are created only after an authorized person confirms the proposal.
24. CloudWatch logs, metrics, and at least one tested alarm are available.
25. The team has a documented cleanup and cost review procedure.

---

# 4. USERS AND AUTHORIZATION

## 4.1. Target users

CampusMeet is designed for:

- Student study groups.
- University project teams.
- Student research groups.
- Small software or academic project teams.
- Small collaborative teams that need lightweight meeting and task management.

## 4.2. Business roles

CampusMeet has two primary roles within each group.

### Group Member

A Group Member can:

- View groups in which they are an active member.
- View meetings and authorized Google Meet links.
- View documents, transcripts, and minutes for permitted meetings.
- Accept or decline an invitation.
- Update the status of assigned tasks.
- View a personal dashboard.
- Use the AI assistant within the permitted group scope.
- Upload content or start transcription when group policy allows it.

### Group Administrator

A Group Administrator has all Member permissions and can also:

- Invite or remove members.
- Create, update, and cancel meetings.
- Select an organizer and attendees.
- Connect Google when acting as the meeting organizer.
- Create and update meeting minutes.
- Create, assign, and manage tasks.
- View the complete group dashboard.
- Confirm AI-generated task proposals.
- Request group-level progress analysis.
- Manage selected integration and retention settings.

A person may be an Administrator in one group and only a Member in another group.

A meeting organizer is not a global system role. It is a meeting-specific responsibility assigned to a Group Administrator whose Google account is used for Calendar synchronization.

## 4.3. Authorization principles

Amazon Cognito authenticates the identity of the user. It does not replace application-level authorization.

Every group-scoped API must follow this sequence:

```text
Validate JWT
→ Read user identity from trusted token claims
→ Resolve internal group or meeting
→ Verify active membership
→ Verify current role
→ Verify resource-level permission
→ Execute the operation
```

The backend must not trust:

- A `userId` declared by the frontend.
- A role declared by the frontend.
- A `groupId` without verifying the resource relationship.
- A `meetingId` without resolving its internal group.
- A Meet Add-on context without internal mapping and permission checks.

---

# 5. PROJECT SCOPE

## 5.1. Mandatory core MVP scope

### Authentication and profile

- Sign-up.
- Email confirmation.
- Sign-in.
- Sign-out.
- Protected routes.
- User profile.
- Time zone.
- Notification preferences.

### Groups and memberships

- Create a group.
- List a user’s groups.
- View group details.
- Create membership invitations.
- Accept or decline invitations.
- Manage members.
- Prevent deletion or demotion of the last Group Administrator.
- Reject unauthorized cross-group access.
- Record important membership audit events.

### Meeting management

- Create a draft or scheduled meeting.
- Meeting title and description.
- Start and end time.
- Agenda.
- Organizer.
- Attendees.
- Update a meeting.
- Cancel a meeting.
- List meetings.
- Group meeting timeline.
- Meeting lifecycle state.
- Validation of attendees against active membership.
- Validation that a scheduled start time is not in the past.

### Google Calendar and Google Meet integration

- Google OAuth Authorization Code Flow.
- Connect and disconnect Google.
- Create a Google Calendar event.
- Send `conferenceData.createRequest`.
- Use a unique request identifier to prevent duplicate conference creation.
- Store `googleEventId`.
- Track synchronization states:

```text
NOT_REQUESTED
PENDING
READY
FAILED_RETRYABLE
ACTION_REQUIRED
```

- Update or cancel a synchronized Calendar event when the internal meeting changes.
- Display a Meet link only when synchronization is ready.
- Attempt post-meeting artifact synchronization when artifacts are actually available.
- Provide upload or consent-based capture fallback when Google artifacts are unavailable.

Google integration is an enhancement around the internal meeting. Failure in Google integration must not prevent CampusMeet from maintaining the internal meeting record.

### Reminders and notifications

- Up to three reminder times for a meeting.
- One-time Amazon EventBridge Scheduler schedules.
- Reminder Lambda processing.
- In-application notification creation.
- Optional email through Amazon SES.
- Skip reminders for canceled meetings.
- Retry with controlled limits.
- Idempotent reminder processing.
- Record delivery and failure status.

The in-application notification is mandatory. Email is an additional delivery channel and may be restricted by the SES sandbox or email verification rules.

### Minutes and tasks

- Meeting summary.
- Discussion content.
- Decisions.
- Action items.
- Convert an action item into a task.
- Task assignee.
- Optional due date.
- Priority.
- Task state:

```text
TODO
DOING
DONE
```

- Task status history.
- Personal dashboard.
- Group dashboard.
- Upcoming meetings.
- Upcoming tasks.
- Overdue tasks.
- Group completion indicators.

### Operations

- Structured application logs.
- Request identifiers.
- CloudWatch metrics.
- CloudWatch alarms.
- Amazon SNS notification.
- AWS Budgets.
- Infrastructure as Code.
- Smoke tests.
- Rollback notes.
- Resource cleanup checklist.

## 5.2. Mandatory AI MVP scope

### Secure upload

Authorized members can upload documents or audio related to a meeting.

The upload flow must:

1. Validate authentication and membership.
2. Validate upload policy.
3. Create attachment metadata with `PENDING_UPLOAD`.
4. Return a short-lived presigned URL.
5. Upload the binary directly from the browser to S3.
6. Call the complete-upload endpoint.
7. Verify the object through metadata or `HeadObject`.
8. Check MIME type, size, checksum, and expected object key.
9. Set the attachment to `READY` or `REJECTED`.
10. Create no more than one AIJob for the completed source.

Large binary content must not pass through API Gateway or Lambda payloads.

### Live transcription

Live transcription is a mandatory AI MVP capability.

It must:

- Start only after explicit consent and capture permission.
- Run in the background during the meeting session.
- Support the configured language or automatic detection when the provider supports it.
- Prioritize Vietnamese `vi-VN` quality during testing.
- Display partial segments temporarily.
- Persist only stable final segments.
- Produce segment fields such as:

```text
sequence
startMs
endMs
text
confidence
languageCode
speakerLabel
isFinal
version
```

- Use anonymous speaker labels such as `Speaker 1` or `Speaker 2`.
- Never infer a real identity from voice.
- Support idempotent retry by session and segment sequence.
- Report missing or incomplete data when the stream fails.
- Never infer spoken content from agenda or attendee metadata.

The live transcript is the authoritative source for determining what was spoken during the meeting.

Agenda, participant lists, and uploaded documents are supporting context only.

### Consent and recording metadata

The system must maintain consent and recording metadata including:

- User or actor who provided consent.
- Consent time.
- Capture source.
- Description of what is captured.
- Retention information.
- Recording or stream reference.
- Meeting and group relationship.

The system must provide a visible indication that transcription or capture is active and a control for stopping the session.

### Transcript management

The system supports:

- Transcript records.
- Transcript finalization.
- Final transcript segments.
- Timestamp-based navigation.
- Speaker label editing.
- Text correction.
- Language correction when appropriate.
- Optional speaker-to-user mapping performed manually.
- Optimistic version control.
- Audit history.
- Approval before knowledge ingestion.

### AIJob processing

Long-running work uses an `AIJob` with the following lifecycle:

```text
QUEUED
PROCESSING
COMPLETED
FAILED
CANCELLED
```

An asynchronous endpoint returns:

```text
202 Accepted
aiJobId
status
```

The client tracks the job instead of keeping a long HTTP request open.

AIJob may represent:

- File parsing.
- Audio transcription.
- Transcript normalization.
- Knowledge ingestion.
- Draft generation.
- Multi-meeting analysis.
- Reprocessing of an approved source.

### Grounded retrieval and citations

CampusMeet supports three retrieval scopes:

```text
CURRENT_MEETING
SELECTED_MEETINGS
WHOLE_GROUP
```

The retrieval process must:

1. Validate JWT.
2. Verify active group membership.
3. Resolve the authorized `groupId`.
4. Verify that every selected meeting belongs to the same group.
5. Apply mandatory metadata filters for `groupId`.
6. Require approved sources.
7. Apply selected `meetingId` filters when provided.
8. Filter before model generation.
9. Prevent cross-group retrieval.
10. Return `insufficientContext=true` when evidence is insufficient.

The system must not retrieve globally and filter after generation.

A citation should identify:

- Meeting title.
- Meeting date.
- Source type.
- Document or minutes title.
- Anonymous speaker label where relevant.
- Timestamp, segment, page, or chunk reference.
- An internal URI or navigation target that the authorized user can open.

Citations must not expose:

- Raw internal S3 keys.
- Long-lived presigned URLs.
- Secrets.
- Tokens.
- Data from unauthorized meetings.

### AI assistant

The AI assistant can support:

- Question answering for the current meeting.
- Question answering across selected meetings.
- Question answering across a whole group.
- Summary for a participant who joins late.
- Meeting topic summary.
- Decision extraction.
- Action-item extraction.
- Draft minutes.
- Task proposals.
- Group-level progress explanation.

The assistant must remain grounded in authorized source content.

### Draft minutes and task proposals

AI can generate:

- Draft meeting summary.
- Draft discussion topics.
- Draft decisions.
- Draft action items.
- Task proposals with citations.

AI must not invent:

- An assignee.
- A deadline.
- A priority.
- A decision not found in the source.
- A meeting statement not supported by a transcript or approved document.

When required fields are missing, the proposal must return `missingFields`.

The confirmation flow is:

```text
AI generates proposal
→ Backend validates schema
→ Frontend shows preview
→ User reviews and edits
→ Authorized person confirms
→ Backend rechecks membership and role
→ Standard Task API creates the task idempotently
→ Audit event is recorded
```

The AI model does not write directly to DynamoDB and does not bypass the normal application service.

### Group progress analysis

AI may explain metrics calculated by the backend, such as:

- Number of completed tasks.
- Number of active tasks.
- Number of overdue tasks.
- Upcoming meetings.
- Task completion trends.
- Work requiring attention.

AI must not:

- Score individual members.
- Rank members.
- Infer personality or attitude.
- Infer ability from transcript data.
- Change task state.
- Compare data across unrelated groups.

## 5.3. Should-have scope

The following capabilities may be implemented after mandatory flows are stable:

- Invitations through email or shareable links.
- Extended audit history.
- Calendar month or week view.
- Google Meet Add-on side panel.
- Google Meet recording or transcript synchronization when available.
- AI assistance for agenda preparation.
- Form completion through a preview-and-confirm flow.
- Document Picture-in-Picture as a progressive enhancement.

These items must not delay the primary CampusMeet Web application.

## 5.4. Out of scope for the baseline MVP

CampusMeet does not implement:

- Its own video calling platform.
- Its own audio calling platform.
- WebRTC infrastructure.
- TURN servers.
- Real-time user-to-user chat.
- A full embedded copy of the Google Meet interface.
- Recording without explicit consent.
- Automatic real-name speaker identification.
- Voice biometrics.
- Long-video understanding.
- Individual employee or student scoring.
- Personal performance evaluation from transcripts.
- Cross-group RAG.
- Unconfirmed AI mutations.
- Arbitrary model-selected tools.
- Public Google Marketplace release within the baseline eight-week plan.
- EC2, RDS, NAT Gateway, or Kubernetes-based infrastructure for the MVP.

---

# 6. AWS SOLUTION ARCHITECTURE

## 6.1. Architecture diagram

![CampusMeet AWS Deployment Architecture](/FCAJ---Workshop--aws/images/2-Proposal/campusmeet-aws-architecture-Target%20MVP%20Architecture.drawio.png)
## 6.2. High-level architecture

```text
CampusMeet Web / Meet Add-on
        │
        ├── HTTPS → Amazon CloudFront
        │               │
        │               └── OAC → Private S3 Frontend Bucket
        │
        ├── Amazon Cognito User Pool
        │
        └── JWT → Amazon API Gateway HTTP API
                         │
                         ▼
                    API Lambda
                         │
        ┌────────────────┼────────────────────┐
        │                │                    │
        ▼                ▼                    ▼
   DynamoDB       S3 User Content      Google Adapters
   5 tables       Presigned Upload     Calendar / Meet
        │
        ├── EventBridge Scheduler
        │           │
        │           ▼
        │      Reminder Lambda
        │           ├── In-app Notification
        │           └── Amazon SES
        │
        └── AWS Step Functions
                    │
                    ▼
              AI Worker Lambda
                    ├── Amazon Transcribe
                    ├── Amazon Bedrock
                    ├── Bedrock Knowledge Bases
                    └── Amazon S3 Vectors

Amazon CloudWatch
        └── CloudWatch Alarms → Amazon SNS

AWS Budgets
        └── Cost alerts

GitHub Actions
        └── AWS SAM / AWS CloudFormation
```

## 6.3. External systems

External systems are represented compactly because the main proposal focuses on AWS deployment.

External components include:

- Users and web browsers.
- Google OAuth.
- Google Calendar API.
- Google Meet REST API.
- Google Meet Add-ons SDK.
- Email recipients.
- Operational alert recipients.
- An alternative STT provider only if benchmarking requires it.

CampusMeet must not depend on Google Meet artifacts for its core workflow.

If a Google recording or transcript does not exist due to account edition, administrator policy, language support, consent, or permissions, CampusMeet uses its own approved upload or capture flow.

## 6.4. AWS services and responsibilities

| AWS service | Responsibility |
|---|---|
| Amazon CloudFront | Delivers frontend assets through HTTPS and caches static content |
| Amazon S3 frontend bucket | Stores the built React application as a private origin |
| Origin Access Control | Allows CloudFront to access S3 without making the bucket public |
| Amazon Cognito User Pool | Provides registration, sign-in, account confirmation, and JWT issuance |
| Amazon API Gateway HTTP API | Provides the public HTTPS API endpoint and JWT authorization |
| AWS Lambda API | Executes application services, authorization checks, and business workflows |
| Amazon DynamoDB | Stores business data using five domain-oriented physical tables |
| Amazon S3 user-content bucket | Stores documents, recordings, audio, and normalized AI sources |
| Amazon EventBridge Scheduler | Creates one-time meeting reminder schedules |
| Reminder Lambda | Creates in-app notifications and attempts optional email delivery |
| Amazon SES | Sends optional reminder email |
| AWS Step Functions | Coordinates long-running AI and ingestion workflows |
| AI Worker Lambda | Performs parsing, normalization, transcription, ingestion, and generation steps |
| Amazon Transcribe | Initial speech-to-text provider, including `vi-VN` testing |
| Amazon Bedrock | Generates grounded answers, draft minutes, and proposals |
| Bedrock Knowledge Bases | Manages retrieval over approved knowledge sources |
| Amazon S3 Vectors | Stores vector data and retrieval metadata |
| AWS Systems Manager Parameter Store | Stores non-public configuration and suitable secure parameters |
| AWS Secrets Manager | Stores secrets or external OAuth credentials when required |
| Amazon CloudWatch | Provides logs, metrics, dashboards, and alarms |
| Amazon SNS | Delivers operational alerts |
| AWS Budgets | Monitors spending and sends budget alerts |
| AWS SAM and CloudFormation | Define and deploy infrastructure as code |
| GitHub Actions | Runs quality gates and controlled deployment workflows |

## 6.5. Frontend architecture

The CampusMeet frontend uses:

- React 19.
- TypeScript.
- Vite.
- React Router.
- TanStack Query.
- AWS Amplify for Cognito integration.

Frontend responsibilities include:

- Rendering the application interface.
- Managing client-side state.
- Obtaining JWTs from Cognito.
- Calling API Gateway with bearer tokens.
- Uploading files directly to S3 using presigned URLs.
- Displaying live transcript state.
- Displaying citations.
- Showing AI proposal previews.
- Requesting user confirmation before mutation.

The frontend must not:

- Access DynamoDB directly.
- Store AWS access keys.
- Store Google refresh tokens.
- Invoke Amazon Bedrock with long-lived credentials.
- Treat route protection as the final authorization boundary.
- Contain hidden administrative API privileges.

## 6.6. Backend architecture

The backend uses Node.js 22 and TypeScript on AWS Lambda.

The internal dependency direction is:

```text
Handler
→ Middleware
→ Application Service
→ Domain Port
→ Repository or Integration Adapter
```

### Handler responsibilities

- Parse HTTP transport data.
- Read route and method.
- Invoke middleware.
- Call an application service.
- Return a standardized HTTP response.

Handlers should not directly query DynamoDB or contain large business workflows.

### Middleware responsibilities

- Parse and validate authentication context.
- Produce a trusted request context.
- Apply common error boundaries.
- Support membership and role checks.
- Attach request identifiers.
- Reject unauthorized requests.

### Application service responsibilities

- Coordinate a use case.
- Enforce business rules.
- Define transaction boundaries.
- Handle idempotency.
- Call repository ports.
- Call integration ports.
- Produce domain results.

### Repository responsibilities

- Implement DynamoDB access patterns.
- Use `Query`, `GetItem`, conditional writes, transactions, and pagination.
- Avoid `Scan` on normal request paths.
- Store and retrieve domain aggregates.
- Keep AWS SDK details outside the domain layer.

### Integration responsibilities

- Google Calendar and Meet APIs.
- EventBridge Scheduler.
- Amazon SES.
- Amazon S3.
- AWS Step Functions.
- Amazon Transcribe.
- Amazon Bedrock.
- Knowledge Base ingestion.
- Secret and parameter access.

---

# 7. TECHNICAL DESIGN

## 7.1. DynamoDB data model v2

CampusMeet uses five physical DynamoDB tables:

| Physical table | Data scope |
|---|---|
| `identity` | User profile, preference, Google integration reference, OAuth state, and notification |
| `collaboration` | Group, membership, invitation, and group audit events |
| `meeting-data` | Meeting, attendee, agenda, minutes, reminder, attachment, recording, consent, live session, transcript, and segment |
| `task-data` | Task, task history, and dashboard-oriented indexes |
| `ai-work` | AIJob, KnowledgeSource, conversation, message, citation, proposal, and idempotency data |

Physical names follow this pattern:

```text
campusmeet-<env>-identity
campusmeet-<env>-collaboration
campusmeet-<env>-meeting-data
campusmeet-<env>-task-data
campusmeet-<env>-ai-work
```

DynamoDB is designed around access patterns and aggregates, not around one table per logical entity.

General rules include:

- Composite `PK` and `SK`.
- Clear entity prefixes.
- Sparse GSIs.
- `Query` instead of `Scan`.
- Conditional writes for state and uniqueness.
- Transactions for atomic multi-item changes.
- UTC timestamps using a fixed ISO 8601 format.
- TTL only for temporary data.
- Binary and normalized documents stored in S3.
- Vector data stored through Knowledge Bases and S3 Vectors.
- Every group-scoped item retains `groupId` for authorization and audit.

Example item relationships:

```text
PK=MEETING#mtg_123
SK=META

PK=MEETING#mtg_123
SK=ATTENDEE#usr_456

PK=MEETING#mtg_123
SK=TRANSCRIPT#tr_001

PK=MEETING#mtg_123
SK=SEGMENT#000001
```

## 7.2. Legacy DynamoDB tables

The development AWS account previously contained 17 legacy tables created before the data model review.

These legacy tables:

- Are not the current physical source of truth.
- Must not be used as the basis for new repositories.
- Must not be deleted immediately.
- Require read-only audit.
- Require backup or export when they contain data.
- Can be cleaned up only after the five-table v2 model is deployed, verified, and smoke-tested.

The migration process is:

```text
Audit legacy tables
→ Identify data and dependencies
→ Backup or export when required
→ Deploy five v2 tables
→ Implement repositories
→ Run integration and smoke tests
→ Verify no code uses legacy tables
→ Review migration evidence
→ Clean up approved legacy resources
```

## 7.3. Main API groups

### Core API

```text
GET    /health
GET    /groups
POST   /groups
POST   /groups/:groupId/invitations
GET    /memberships
PATCH  /memberships
GET    /meetings
POST   /meetings
PATCH  /meetings
DELETE /meetings
GET    /minutes
POST   /minutes
GET    /tasks
POST   /tasks
PATCH  /tasks
GET    /dashboard
GET    /notifications
PATCH  /notifications
POST   /integrations/google
DELETE /integrations/google
```

### Google artifact and meeting source API

```text
POST /meetings/:meetingId/google-artifacts/sync
POST /meetings/:meetingId/attachments
GET  /meetings/:meetingId/attachments
POST /meetings/:meetingId/recordings
GET  /meetings/:meetingId/recordings
POST /meetings/:meetingId/transcripts
GET  /meetings/:meetingId/transcripts
PATCH /meetings/:meetingId/transcripts
```

### Live transcription API

```text
POST /meetings/:meetingId/live-transcription
GET  /meetings/:meetingId/live-transcription/:sessionId
POST /meetings/:meetingId/live-transcription/:sessionId/segments
POST /meetings/:meetingId/live-transcription/:sessionId/stop
```

### AI API

```text
POST /meetings/:meetingId/ai/chat
POST /groups/:groupId/ai/search
POST /meetings/:meetingId/ai/minutes-draft
POST /meetings/:meetingId/ai/task-proposals
POST /ai/task-proposals/:id/confirm
POST /groups/:groupId/ai/progress-analysis
GET  /ai/jobs/:aiJobId
```

The Google Meet Add-on uses the same endpoints and has no separate privileged API.

## 7.4. Standard API response

Successful response:

```json
{
  "success": true,
  "data": {},
  "requestId": "request-id"
}
```

Failure response:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "A safe user-facing message",
    "details": {}
  },
  "requestId": "request-id"
}
```

The API must not return:

- Stack traces.
- Access tokens.
- Refresh tokens.
- AWS credentials.
- Raw secrets.
- Sensitive internal S3 keys.
- Unfiltered provider errors.
- Internal authorization details that could help an attacker.

## 7.5. Secure upload and AIJob flow

```text
Browser
→ Request presigned upload
→ API verifies membership and upload policy
→ API creates PENDING_UPLOAD attachment
→ API returns short-lived presigned URL
→ Browser uploads directly to S3
→ Browser calls complete-upload
→ Backend verifies object metadata
→ Attachment becomes READY or REJECTED
→ Exactly one AIJob is created
→ Step Functions executes processing
→ Client tracks AIJob status
```

## 7.6. Live transcription flow

```text
User provides consent
→ User selects or permits capture source
→ CampusMeet creates LiveTranscriptionSession
→ Streaming STT begins
→ Partial segments are displayed temporarily
→ Final segments are submitted
→ Backend verifies session and sequence
→ Final segments are stored in meeting-data
→ Transcript is reviewed and corrected
→ Transcript is approved
→ Approved source is ingested
```

If live streaming fails, dependent features must report incomplete source data. They must not silently produce authoritative minutes from agenda or participant metadata.

## 7.7. RAG flow

```text
User submits a question
→ Validate JWT
→ Verify active membership
→ Resolve authorized group
→ Verify selected meeting set
→ Apply groupId and approved-source filter
→ Apply optional meetingId filter
→ Retrieve authorized chunks
→ Generate grounded answer with Bedrock
→ Normalize citations
→ Return GroundedAnswer
```

Authorized source types may include:

- `READY` documents.
- Approved transcripts.
- Approved meeting minutes.
- Final live segments for current-meeting summarization when permitted.

## 7.8. Google synchronization flow

```text
Group Administrator connects Google
→ Backend completes OAuth callback
→ Token or secret reference is stored securely
→ CampusMeet creates internal meeting
→ Google Calendar API is called
→ Conference data is requested
→ googleEventId and sync state are stored
→ Limited polling or retry is applied
→ READY: authorized members can view the Meet link
→ Artifact unavailable: approved upload or capture fallback
```

A new Google event must not be created only because a previous request remains `PENDING`.

---

# 8. SECURITY AND PRIVACY

## 8.1. Authentication

- Amazon Cognito User Pool authenticates users.
- API Gateway validates JWTs.
- Backend reads identity from trusted claims.
- Frontend protected routes improve user experience but are not the final security boundary.

## 8.2. Application authorization

The backend must:

- Verify membership for every group-scoped resource.
- Verify current role for every mutation.
- Resolve the group from internal resources.
- Check meeting ownership and group relationship.
- Check that attendees and assignees are active members.
- Verify permissions again during proposal confirmation.
- Prevent cross-group document, transcript, and vector retrieval.

## 8.3. IAM

The AWS environment follows least privilege:

- Each Lambda receives only the permissions it requires.
- Application Lambdas do not receive broad administrator permissions.
- Developers use individual AWS profiles.
- Human users use MFA.
- The root account is not used for routine deployment.
- Long-lived AWS credentials are not committed to Git.
- GitHub Actions should use temporary credentials, preferably through OpenID Connect where supported.

## 8.4. S3 controls

The frontend and user-content buckets must use:

- S3 Block Public Access.
- Encryption at rest.
- HTTPS in transit.
- Controlled object key patterns.
- Short-lived presigned URLs.
- MIME type allowlists.
- File size limits.
- Checksum verification.
- Lifecycle policies.
- Retention and deletion controls.

Recordings, transcripts, and user documents must not be publicly accessible.

## 8.5. Google OAuth security

Google integration uses:

- Authorization Code Flow.
- Minimum required scopes.
- Server-side token handling.
- Secure token references or encrypted storage.
- No refresh token exposure to the browser.
- No token values in logs.
- A disconnect operation.
- Clear disclosure of requested Google permissions.

## 8.6. Consent and media privacy

Before capture starts, CampusMeet must show:

- The purpose of capture.
- The capture source.
- The data being stored.
- The retention period or policy.
- The scope of AI processing.
- A clear consent action.

During capture, the system should show:

- An active transcription indicator.
- Current session status.
- A stop control.
- Failure or reconnection status.

CampusMeet must not imply that browser microphone capture always records every Google Meet participant correctly.

## 8.7. AI safety

AI safety controls include:

- Treating documents and transcripts as untrusted input.
- Preventing source text from becoming system or tool instructions.
- Server-side tool allowlists.
- Backend schema validation.
- Citation requirements.
- Human confirmation.
- Idempotency.
- Audit events.
- Rate limits.
- Token and time quotas.
- Insufficient-context responses.
- No direct AI access to DynamoDB mutation APIs.

## 8.8. Logging privacy

Logs must not contain:

- Raw audio.
- Full transcript content unless explicitly required and protected.
- Complete prompts containing sensitive data.
- Google access or refresh tokens.
- JWTs.
- Active presigned URLs.
- Secrets.
- Passwords.
- Unnecessary personal data.

Logs should use identifiers, status, timing, count, and safe error codes.

---

# 9. RELIABILITY, MONITORING, AND COST CONTROL

## 9.1. Reliability controls

Important operations require:

- Idempotency keys.
- Conditional writes.
- Optimistic version control.
- Controlled retries.
- Exponential backoff where appropriate.
- Timeouts.
- Explicit failure states.
- Failure records or dead-letter handling for background processing.
- Smoke tests after deployment.
- Rollback procedures.

Examples:

- Retrying group creation must not create duplicate groups.
- Retrying Google synchronization must not create duplicate events.
- Retrying a reminder must not send duplicate notifications.
- Retrying complete-upload must not create multiple AIJobs.
- Retrying a final transcript segment must not create duplicates.
- Retrying proposal confirmation must not create multiple tasks.

## 9.2. CloudWatch observability

Amazon CloudWatch collects:

- API Gateway access logs.
- Lambda invocation count.
- Lambda errors.
- Lambda duration.
- Lambda throttles.
- API latency.
- API 4xx and 5xx rates.
- Authorization failures.
- Google synchronization states.
- Reminder sent, skipped, and failed metrics.
- Email delivery failures.
- AIJob queue, completion, and failure metrics.
- Transcription duration and failure.
- Retrieval with no results.
- Missing citation count.
- Step Functions failure.
- Dead-letter message count where applicable.
- AI token or cost metadata by environment.

## 9.3. Alarms

The team should configure alarms for:

- API 5xx rate.
- Lambda error rate.
- Lambda throttling.
- Reminder processing failure.
- Google synchronization failure.
- AIJob failure or timeout.
- Transcription failure.
- Unexpected empty retrieval rate.
- Missing citation rate.
- Dead-letter queue messages.
- Cost or forecast exceeding a threshold.

CloudWatch Alarms send notifications through Amazon SNS.

## 9.4. Cost model

CampusMeet uses consumption-based services and avoids continuously running infrastructure.

| Service | Main cost factor | Cost control |
|---|---|---|
| CloudFront | Requests and data transfer | Cache static assets and compress content |
| S3 | Storage, requests, and transfer | Lifecycle, retention, and cleanup |
| Cognito | Monthly active users | Small controlled demo user base |
| API Gateway | Request count | Avoid unnecessary polling |
| Lambda | Invocations, duration, and memory | Short timeouts and asynchronous processing |
| DynamoDB | Read/write requests and storage | On-demand capacity, Query/GSI, no Scan |
| EventBridge Scheduler | Schedules and invocations | Delete obsolete schedules |
| SES | Email count | Use email only as an optional channel |
| CloudWatch | Log ingestion and retention | Set log retention and avoid large payload logs |
| Step Functions | State transitions | Keep workflows compact |
| Transcribe | Audio duration | Limit session and demo duration |
| Bedrock | Input and output tokens | Token limits, quotas, and controlled model settings |
| Knowledge Bases and S3 Vectors | Ingestion, storage, and queries | Ingest only approved sources and remove stale sources |
| Secrets Manager | Number of secrets and API calls | Use SSM SecureString where appropriate |
| SNS | Notification count | Send only meaningful operational alerts |

A fixed monthly amount should not be claimed without explicit assumptions.

The actual cost depends on:

- AWS Region.
- Selected Bedrock models.
- Embedding model.
- Number of transcription minutes.
- Input and output tokens.
- Number and size of documents.
- Audio storage.
- Ingestion frequency.
- Retrieval frequency.
- Retention period.
- Number of environments.

Before a demonstration deployment, the team must:

1. Define expected usage.
2. Estimate cost with AWS Pricing Calculator.
3. Configure AWS Budgets.
4. Configure transcription and Bedrock quotas.
5. Record minutes, tokens, and estimated cost by AIJob.
6. Review Cost Explorer after the demo.
7. Remove unused resources.

---

# 10. IMPLEMENTATION PLAN

## 10.1. Development approach

CampusMeet is implemented through vertical slices instead of completing the entire frontend before the backend.

Each vertical slice includes:

```text
Shared contract
→ Domain rules
→ Application service
→ Repository or integration adapter
→ API
→ Frontend
→ Tests
→ Infrastructure changes where required
→ Smoke test
→ Evidence
```

When a dependency is not yet implemented, a fake repository or provider may be used in tests or local development.

Fake implementations must not be hard-coded into production handlers.

## 10.2. Eight-week roadmap

| Week | Focus | Deliverables | Exit criteria |
|---|---|---|---|
| 1 | Analysis and alignment | SRS, wireframes, architecture, data model, IAM and cost plan | Team approves the baseline |
| 2 | Platform foundation | Monorepo, CI, IaC skeleton, Cognito, frontend hosting target, API skeleton, UI shell | Sign-in and health-check evidence |
| 3 | Groups and memberships | Group, membership, invitation, and authorization boundary | Cross-group access returns 403 |
| 4 | Internal meetings | Meeting, agenda, attendee, organizer, lifecycle, and timeline | Internal meeting flow works end to end |
| 5 | Google integration | OAuth, Calendar event, conference request, sync state, retry, and artifact spike | Real integration or explicit fallback evidence |
| 6 | Post-meeting and source foundation | Reminder, notification, minutes, task, dashboard, presigned upload, consent, and recording metadata | Minutes → Task → Dashboard and direct S3 upload |
| 7 | AI vertical slice | Live STT, transcript editor, meeting chat, draft minutes/tasks, KnowledgeSource, and citations | One meeting works end to end with citations |
| 8 | Completion | Multi-meeting RAG, ACL tests, progress analysis, prompt-injection tests, alarms, cost, retention, and cleanup | No cross-group leakage and complete demo evidence |

## 10.3. Recommended merge order

1. Data model v2 and infrastructure.
2. Shared error, pagination, and idempotency contracts.
3. Group and membership authorization boundary.
4. Meeting boundary.
5. Minutes, tasks, dashboard, and notifications.
6. Google integration.
7. Upload, consent, live transcript, and AIJob.
8. KnowledgeSource, RAG, and citations.
9. Reminder and notification integration.
10. Security, observability, cost, and cleanup rehearsal.

---

# 11. ACTIVITIES AND DELIVERABLES

| Phase | Activity | Deliverable |
|---|---|---|
| Analysis | Define the problem, users, use cases, scope, and constraints | SRS and proposal |
| Design | Design AWS architecture, API contracts, and data access patterns | Draw.io diagrams, API contract, DynamoDB model |
| Foundation | Configure monorepo, CI, authentication, and IaC | Executable repository and quality gates |
| M1 core | Build group, invitation, membership, and authorization | Group vertical slice |
| M2 core | Build meeting, agenda, attendee, and lifecycle | Meeting vertical slice |
| M3 core | Build minutes, tasks, dashboard, and notification core | Post-meeting workflow |
| M4 integration | Build OAuth, Calendar/Meet adapter, artifacts, reminders, and SES | Integration vertical slice and fallback |
| M5 AI | Build upload, consent, live transcript, AIJob, RAG, citations, and proposals | Citation-grounded AI vertical slice |
| Testing | Perform unit, integration, security, end-to-end, and operational testing | Test report and evidence |
| Deployment | Review CloudFormation changes, deploy, and smoke-test | AWS development environment |
| Operations | Configure logs, metrics, alarms, cost review, and cleanup | Dashboard, alarms, and runbooks |
| Reporting | Produce worklogs, workshop pages, and presentation material | Bilingual workshop website |

---

# 12. TEAM RESPONSIBILITIES

| Member | Main outcome | Responsibility |
|---|---|---|
| M1 | Groups and authorization | Group, membership, invitation, role, and authorization helper |
| M2 | Meetings | Meeting, agenda, attendee, organizer, lifecycle, and timeline |
| M3 | Post-meeting workflow | Minutes, action items, tasks, dashboard, and progress snapshots |
| M4 | Google integration and reminders | OAuth, Calendar/Meet integration, artifact synchronization, Scheduler, notification, and SES |
| M5 | Upload, transcript, and AI | Attachment, recording consent, live STT, AIJob, KnowledgeSource, RAG, citations, and proposals |

Shared contracts, router changes, IAM, and Infrastructure as Code require cross-review.

Ownership identifies responsibility for an outcome. It does not grant exclusive ownership of a file.

Each member should provide:

- Clear commits or pull requests.
- Unit or integration tests.
- At least one error or boundary case.
- CloudWatch or runtime evidence.
- Worklog documentation.
- Cleanup confirmation for created resources.

---

# 13. RISKS AND MITIGATION STRATEGIES

| Risk | Impact | Mitigation |
|---|---|---|
| Incorrect OAuth or redirect URI configuration | High | Test early and keep internal CRUD independent |
| Google Meet link remains pending | Medium | Display state, use limited retry, and never fabricate a link |
| Google artifact does not exist | High | Provide approved upload or capture fallback |
| SES sandbox limits email | Medium | Keep in-app notifications mandatory |
| Scope expansion | High | Maintain MoSCoW priorities and weekly gates |
| Cross-group data leakage | High | Central authorization and explicit 403 tests |
| Duplicate event or reminder | High | Use idempotency and conditional writes |
| Capture does not contain complete meeting audio | High | State capture limitations clearly |
| Vietnamese STT is inaccurate | High | Benchmark, confidence display, editor, and human review |
| Speaker labels are inaccurate | Medium | Use only anonymous `Speaker N` labels |
| Prompt injection | High | Treat sources as untrusted, use allowlists, schemas, confirmation, and audit |
| RAG retrieves another group’s data | High | Filter by `groupId` before retrieval and test cross-group access |
| AI hallucination | High | Require citations, draft state, and insufficient-context behavior |
| Incorrect task creation | High | Use missing fields, preview, correction, and confirmation |
| AI cost grows unexpectedly | Medium | Apply token/minute quotas, Budgets, and cost metadata |
| Legacy data is deleted accidentally | High | Audit, backup, and review before cleanup |
| Templates exist but application does not work | High | Require integration tests, smoke tests, and logs |
| Documentation is delayed | Medium | Capture evidence after every sprint |

---

# 14. ROADMAP FROM THE CURRENT REPOSITORY TO THE TARGET MVP

## 14.1. Current implementation state

At the time of this proposal:

| Area | Current state |
|---|---|
| Cognito authentication | Foundation exists and was previously integration-tested; the temporary test stack was cleaned up |
| React frontend | Project structure exists, but many business screens still use mock data |
| `GET /health` | Implemented and returns `200` |
| Protected identity endpoint | Basic protected integration exists |
| Business API handlers | Many remain skeletons and return `501 Not Implemented` |
| Group-level authorization | Not yet complete |
| DynamoDB repositories | Not yet complete across business vertical slices |
| DynamoDB v2 | Five-table physical design has been approved |
| Legacy DynamoDB tables | Seventeen tables require audit and backup before cleanup |
| Secure upload and live transcript | Contracts and target architecture are defined but not fully implemented |
| AIJob and Bedrock RAG | Target design, not yet proven operational |
| Full application stack | Not production-ready |
| CI | Basic quality gates exist |
| CD | Deployment, smoke test, and rollback still require completion |

A service appearing in a template or diagram must not be described as operational without code, deployment output, integration tests, and logs.

## 14.2. Deployment stages

### Stage 1 – Data foundation

- Freeze the legacy schema.
- Audit the seventeen legacy tables.
- Back up or export required data.
- Preview the CloudFormation change set.
- Deploy the five v2 tables.
- Verify keys, GSIs, TTL, tags, and outputs.
- Keep the legacy tables until migration is proven.

### Stage 2 – Core application

- Group and membership.
- Invitation.
- Authorization helper.
- Meeting.
- Agenda and attendees.
- Minutes.
- Tasks.
- Dashboard.
- Notifications.
- Real DynamoDB repositories.
- Cross-group authorization tests.

### Stage 3 – External integrations

- Google OAuth.
- Calendar event creation.
- Conference request.
- Synchronization lifecycle.
- Reminder scheduling.
- SES fallback.
- Google artifact synchronization when available.

### Stage 4 – AI source pipeline

- Private S3 user-content bucket.
- Presigned upload.
- Complete-upload verification.
- Consent.
- Recording metadata.
- Live transcription session.
- Transcript segments.
- Transcript editor.
- AIJob processing.

### Stage 5 – Grounded AI

- Normalized knowledge sources.
- Bedrock Knowledge Base.
- S3 Vectors.
- Current-meeting chat.
- Selected-meeting RAG.
- Whole-group RAG.
- Citations.
- Draft minutes.
- Task proposals.
- Group progress analysis.

### Stage 6 – Release candidate

- Security tests.
- Cross-group RAG tests.
- Idempotency tests.
- Prompt-injection tests.
- CloudWatch dashboard.
- Alarms and SNS.
- Cost review.
- Retention policies.
- Cleanup rehearsal.
- Complete demonstration evidence.

---

# 15. ACCEPTANCE CRITERIA

The project is accepted only when:

1. Lint, type checking, tests, and builds pass.
2. AWS infrastructure can be reproduced through SAM or CloudFormation.
3. No credentials, tokens, or secrets are committed to the repository.
4. The following core workflow is complete:

```text
Sign in
→ Create group
→ Invite member
→ Create meeting
→ Synchronize Google
→ Send reminder
→ Create minutes
→ Create task
→ View dashboard
```

5. The following AI vertical slice is complete:

```text
Upload or consent
→ Live transcription
→ Transcript review
→ Chat or late-join summary
→ Draft minutes
→ Task proposal
→ Citation
→ Human confirmation
→ Official task
```

6. RAG supports current meeting, selected meetings, and whole-group scope.
7. Tests prove that Group A cannot retrieve sources from Group B.
8. The system does not automatically identify or evaluate individual speakers.
9. AI does not mutate application data without confirmation.
10. Email failure does not remove the in-application notification.
11. At least one CloudWatch and SNS alarm is tested.
12. Logs contain no token, secret, active presigned URL, or sensitive raw content.
13. The team records transcription minutes, model token usage, or estimated AI cost.
14. Retention and cleanup procedures are documented.
15. Every team member provides implementation and operational evidence.
16. Documentation and diagrams correctly distinguish:

```text
Implemented
Incomplete
Proposed
```

---

# 16. EXPECTED OUTCOMES

After completion of the MVP, CampusMeet provides a unified workflow for team meetings:

- Members know which meetings are upcoming.
- Organizers reduce repeated Calendar and Meet setup work.
- Members receive reminders at the correct time.
- Meeting content is recorded as a reviewable transcript.
- Late participants can receive a grounded summary of what has occurred.
- Decisions and action items include evidence.
- Action items become tasks through a controlled confirmation flow.
- Group Administrators can track group progress.
- Users can ask questions across multiple meetings without cross-group data leakage.
- The system provides logs, metrics, alarms, cost controls, retention, and cleanup.
- The infrastructure can be redeployed from source-controlled templates.

CampusMeet demonstrates how AWS serverless services, controlled Google integration, and grounded AI can be combined to solve a practical team collaboration problem.

The core value of CampusMeet is not replacing Google Meet. Its value is transforming every meeting into a structured workflow with preparation, evidence, decisions, responsibilities, and measurable follow-up.