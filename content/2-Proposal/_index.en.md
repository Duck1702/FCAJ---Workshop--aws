---
title: " Proposal"
date: 2026-06-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## 1. Project Name

CampusMeet — AI-Assisted Meeting and Collaboration Platform on AWS

## 2. Project Overview

CampusMeet is a web application for university teams to organize members, meetings, minutes, tasks, reminders, and meeting knowledge. Consent-based live transcription and approved artifacts support cited answers, draft minutes, and task proposals. AI never silently changes official data.

The source contains a React/Vite frontend, TypeScript Lambda API structure, shared contracts, tests, AWS SAM templates, a five-table DynamoDB foundation, API contract, SRS, and AWS architecture diagram. This proposal turns that baseline into a staged AWS plan; it does not claim target components are operational.

## 3. Problem Statement

Teams distribute meeting information across chat, calendars, documents, and notes. Decisions are lost, late participants lack context, reminders are inconsistent, and multi-meeting search is slow. AI is unsafe when answers lack citations, cross group boundaries, infer speaker identities, or create tasks without approval.

## 4. Objectives and Target Users

CampusMeet serves project teams, student clubs, group administrators, meeting organizers, members, minute takers, and operators. Objectives are to deliver authenticated collaboration workflows; consent-based transcription; editable Speaker N segments; cited answers, minutes, and task proposals; group-isolated retrieval; compact Google Calendar integration; and a low-operations, monitored, cost-controlled serverless deployment.

## 5. Functional Scope

Scope includes authentication; groups, invitations, and roles; meeting lifecycle, agendas, attendees, and Google sync status; reminders and notifications; minutes, tasks, and dashboards; secure upload; live transcription; transcript approval; asynchronous AI jobs; grounded chat; draft minutes; cited task proposals; and group progress explanation. Official mutations require authorization, preview, confirmation, and idempotency.

### 5.1. MVP and Future Scope

The MVP covers core collaboration, one-time reminders, optional email, uploads, AIJob tracking, consent-based vi-VN-priority transcription, transcript review, cited RAG over current/selected/all group meetings, draft minutes, confirmed task proposals, monitoring, cost alerts, and cleanup guidance.

Future scope includes recurring meetings, Slack or Discord, richer Google Meet artifact sync, reranking, and a Meet side panel. A custom video-call stack, real speaker identity inference, individual evaluation, and autonomous AI mutation are excluded.

## 6. Current Implementation Status

| Status | Repository evidence |
| --- | --- |
| **Implemented** | React/Vite app shell and routes; Cognito UI integration and auth tests; shared contracts; API utilities; working /health and JWT-based /me; CI/tests; SAM target templates; five encrypted, on-demand DynamoDB tables with GSIs, TTL, and optional PITR. |
| **Incomplete** | Feature pages mostly use mocks; group/meeting repositories throw NotImplementedError; most endpoints are skeletons; authorization is unfinished; SAM retains TODOs for CORS, IAM scope, domain, alarms, and deployment role. |
| **Proposed** | AWS deployment, user-content S3, upload validation, Transcribe, AIJob workers, transcript persistence, Bedrock RAG/citations, Calendar, Scheduler/SES, SQS DLQ, Parameter Store, dashboards, and budget alarms. |

Implemented means present in source or IaC, not deployed in an AWS account.

## 7. Target AWS Deployment Architecture

![CampusMeet target AWS architecture](/FCAJ---Workshop--aws/images/2-Proposal/campusmeet-aws-architecture-Target%20MVP%20Architecture.drawio.png)

Amazon CloudFront serves the React build from a private Amazon S3 frontend bucket through Origin Access Control. Amazon Cognito issues JWTs validated by Amazon API Gateway HTTP API, which invokes least-privilege AWS Lambda. Amazon DynamoDB on-demand stores domain records; a separate private S3 user-content bucket stores uploads and artifacts. Amazon Transcribe supplies live segments and Amazon Bedrock supplies grounded generation and multi-meeting RAG.

Amazon EventBridge Scheduler invokes reminders, Amazon SES sends optional email to external recipients, and an Amazon SQS dead-letter queue isolates terminal asynchronous failures. AWS Systems Manager Parameter Store holds configuration and secure references. Amazon CloudWatch, Amazon SNS, and AWS Budgets provide observability and cost alerts. GitHub Actions and AWS SAM provide validation and delivery. Google Calendar API remains a compact external system. The MVP needs no EC2, ECS/EKS, RDS, ALB, VPC, or NAT Gateway.

Runtime flow: CloudFront loads the web app; Cognito authenticates; the HTTP API and Lambda enforce identity, group role, and record scope; Lambda queries DynamoDB or issues a narrow presigned S3 URL; long speech/AI work becomes an AIJob; Scheduler invokes Reminder Lambda; CloudWatch alarms notify SNS.

## 8. Frontend Architecture

The existing apps/web uses React, Vite, TypeScript, public and protected routes, an app shell, feature services, and an API client. Static output targets the private frontend bucket. Runtime configuration contains only public API/Cognito values; credentials and secrets never enter the bundle. Current screens cover auth, dashboard, groups, meetings, tasks, notifications, and settings, but mocks must be replaced with authenticated calls and clear loading, empty, unauthorized, and retry states.

## 9. Backend and API Architecture

The services/api TypeScript structure includes handlers, DTOs, domain ports, adapters, consistent responses, request IDs, and structured logging. API Gateway is the public boundary; Lambda is stateless and durable data belongs in DynamoDB or S3. The contract covers health/profile, groups/membership, meetings, minutes, tasks, dashboard, notifications, integrations, uploads, transcripts, AI jobs, and AI interactions. Every group-owned endpoint derives trusted identity from JWT claims, validates membership and meeting scope, uses opaque pagination, and makes create/retry operations idempotent.

## 10. Authentication and Authorization

Cognito manages sign-up, confirmation, sign-in, and JWT issuance. The HTTP API authorizer validates issuer, audience, and expiry; backend roles then authorize admin, organizer, member, or minute-taker actions. A client userId is never trusted. Cross-group IDs are rejected before read, retrieval, or mutation. Lambda roles follow least privilege, logs redact sensitive values, and Google credentials remain server-side through secure Parameter Store references.

## 11. DynamoDB Data Architecture

| Table | Responsibility |
| --- | --- |
| Identity | Profiles, invitations, identities, and integration references. |
| Collaboration | Groups, memberships, and collaboration relationships. |
| MeetingData | Meetings, attendees, reminders, minutes, artifacts, and transcript records. |
| TaskData | Tasks, assignee/status views, and dashboard access patterns. |
| AIWork | AIJob, upload/transcription work, retrieval metadata, proposals, and idempotency. |

The source-controlled tables use PK/SK, workload GSIs, on-demand billing, encryption, TTL, and optional PITR. Queries use keys or GSIs, not scans. Group and meeting IDs travel with AI records. Conditional writes prevent duplicate invitations, upload completion, final segments, reminders, and confirmed tasks.

## 12. Secure File Upload

An authorized client requests an upload intent with meeting, MIME, size, and checksum. Lambda checks membership, policy, quota, and a server-owned key, then returns a short-lived presigned URL restricted to one object and required headers. The browser uploads directly to private S3. Completion verifies metadata, size, checksum, and ownership before READY; only then is an idempotent AIJob created. Public access is blocked, objects are encrypted and lifecycle-managed, and authorized downloads are short-lived.

## 13. Live Transcription and Transcript Storage

After explicit consent and capture permission, CampusMeet starts a background session and displays its state and source. Audio streams to Amazon Transcribe with a chosen language or supported auto-identification; Vietnamese is the quality benchmark. Microphone-only browser capture cannot guarantee all Google Meet audio.

Partial hypotheses update the UI but are not durable evidence. Final segments persist idempotently with meeting, timestamps, confidence, language, session, and anonymous Speaker N. Authorized users correct and approve versions; voice inference never maps labels to people. Approved transcript artifacts remain private in S3 while queryable metadata and segments live in DynamoDB. If capture fails, dependent AI reports missing evidence rather than reconstructing speech from agenda or attendees.

## 14. AIJob Processing

Parsing, transcription completion, indexing, and generation create AIJob records with QUEUED, PROCESSING, COMPLETED, FAILED, or CANCELLED. Workers claim jobs conditionally, update progress, expose safe errors, and retry only transient failures. Repeated failures enter the SQS DLQ for inspected redrive. Correlation IDs connect API requests, jobs, sessions, and CloudWatch logs.

## 15. Amazon Bedrock Multi-Meeting RAG and Citations

Only READY documents, permitted live or approved transcript versions, and approved minutes are eligible sources. After authorization, retrieval is restricted to CURRENT_MEETING, a validated SELECTED_MEETINGS set in one group, or WHOLE_GROUP. Bedrock receives passages plus source metadata, never unrestricted table or bucket access.

Every answer cites its meeting and exact document section or Speaker N timestamp; provisional live text is labeled. Missing or conflicting evidence produces an explicit limitation, not fabrication. Draft minutes and TaskProposal records retain citations. A permitted user completes required fields, reviews, and confirms before the normal Task API performs an idempotent mutation.

## 16. Notifications, Reminders, and Google Calendar

Lambda creates or updates a one-time EventBridge Scheduler schedule. At execution, Reminder Lambda rechecks meeting and membership state, writes an in-app notification, and optionally calls SES. Email failure does not erase the primary record; terminal failures go to the DLQ. SES identities and recipients must be verified for the account.

The compact Google Calendar adapter uses a server-side token to create/update events and request conference data when supported. CampusMeet records PENDING, READY, FAILED_RETRYABLE, or ACTION_REQUIRED. Google quota, permission, or artifact limitations do not block core meetings; manual upload and consent-based transcription are fallbacks.

## 17. Security

Controls include default-deny group authorization; least-privilege API, reminder, AI, and deployment roles; private encrypted S3 with OAC and public-access blocks; short presigned URLs; encrypted DynamoDB with conditional writes, TTL, and PITR where needed; Parameter Store SecureString references; schema validation, upload allowlists, checksum and size checks, output encoding, restricted CORS, consent and retention policy, transcript version history, and audited human confirmation for AI-assisted mutations.

## 18. Monitoring

CloudWatch structured logs contain request/correlation IDs but no tokens or raw sensitive transcript text. Metrics and alarms cover API errors/latency, Lambda errors/throttles, AIJob age/failures, Transcribe failures, Scheduler/SES failures, DLQ depth, and anomalous Bedrock usage. Alarms publish to an SNS operations topic. Runbooks cover retry, DLQ redrive, integration failure, recovery, and cleanup.

## 19. CI/CD

GitHub Actions currently runs repository quality checks. The target pipeline installs locked dependencies, lints, type-checks, tests, validates SAM, builds frontend and Lambda artifacts, and supports review. AWS SAM deploys with environment parameters and an approved least-privilege GitHub OIDC role, not long-lived keys. Production requires approval, smoke tests, CloudFormation change review, and rollback instructions.

## 20. Cost Control

CloudFront, S3, Lambda, HTTP API, DynamoDB on-demand, Scheduler, and asynchronous jobs avoid idle servers. AWS Budgets and SNS alert at thresholds; CloudWatch tracks Transcribe minutes, Bedrock usage, storage, and email. Upload size, retention, RAG scope, model/output, retries, and concurrency are capped. Lifecycle rules expire nonproduction artifacts and expensive AI can be disabled independently. Budgets alerts do not automatically stop resources.

## 21. Implementation Phases

| Phase | Deliverable |
| --- | --- |
| 1. Baseline | Confirm contracts, status labels, threat model, access patterns, cost limits, and architecture. |
| 2. Core | Deploy five tables; complete auth, authorization, repositories, meetings, minutes, tasks, and replace mocks. |
| 3. Web | Add CloudFront, private S3/OAC, restricted CORS, smoke tests, and rollback. |
| 4. Automation | Implement Calendar, Scheduler, notifications, SES fallback, idempotency, and DLQ. |
| 5. Content | Add user-content S3, verified uploads, AIJob workers, retention, and transcript records. |
| 6. AI MVP | Add Transcribe, transcript approval, scoped Bedrock RAG, citations, minutes drafts, and confirmed task proposals. |
| 7. Operations | Tune alarms, budgets, security/cost tests, runbooks, backup/restore, and cleanup evidence. |

## 22. Risks and Mitigations

| Risk | Mitigation |
| --- | --- |
| Skeleton is mistaken for a running system | Keep the status matrix and require deployment evidence and smoke tests. |
| Cross-group exposure | Authorize first, encode group scope, validate every meeting ID, and test negative isolation. |
| Missing or poor audio | Visible capture state, language benchmarks, correction/approval, and honest missing-evidence responses. |
| Hallucination or unsafe mutation | Source allowlist, citations, grounded prompts, human confirmation, idempotency, and audit history. |
| Unsafe upload | MIME/size/checksum policy, server-owned keys, short expiry, safety state, and no indexing before READY. |
| Google or SES limitations | Visible status, bounded retry, manual upload, in-app fallback, and verified identities. |
| Duplicate jobs | Conditional claims, idempotency keys, state machine, DLQ, and controlled redrive. |
| Unexpected cost | Budgets/SNS, usage dashboards, quotas, retention, bounded AI context/output, and no always-on servers. |

## 23. Expected Outcomes

The MVP gives a small team one secure workspace for meetings, minutes, tasks, reminders, transcripts, and cited knowledge. It demonstrates serverless AWS architecture, group-isolated authorization, private direct uploads, asynchronous speech/AI, human-controlled AI output, observable operations, repeatable SAM delivery, and measurable cost control.

Success requires functional and security tests, deployment evidence, cited AI answers, no cross-group retrieval, recoverable failures, and documentation that distinguishes source implementation from deployed operation.
