---
title: "Proposal"
date: 2026-08-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# CAMPUSMEET — INTELLIGENT MEETING MANAGEMENT SYSTEM ON AWS

{{% notice info %}}
This proposal separates **target architecture** from **current implementation**: M1 and core M2 have code/tests; M3 remains skeletal; M4 lacks a complete real adapter; M5 has partial API/AI Worker/IaC/tests, while upload/live transcript and the full cloud pipeline are not E2E complete.
{{% /notice %}}

## 2.1 Project overview

CampusMeet serves study groups, university projects, and small teams that need one workflow before, during, and after meetings. Members, Group Admins, organizers, and attendees use it to centralize groups, agendas, schedules, minutes, tasks, transcripts, and cited knowledge within one authorization boundary. Managed serverless services avoid continuously running servers, scale on demand, and are repeatable through IaC. CampusMeet is not a video platform; online meetings take place on external Google Meet.

## 2.2 Problem statement

Schedules, agendas, organizers, and attendees are fragmented; minutes are manual; decisions and tasks are missed; long transcripts are hard to summarize and search across meetings; AI without citations is not traceable; reminders and Google synchronization diverge from internal records; and trusting a client-provided `groupId` risks cross-group access.

## 2.3 Proposed solution

| Module | Solution |
|---|---|
| M1 | Cognito JWT; Identity, Group, Membership, Invitation; trusted identity and group authorization |
| M2 | Meeting CRUD/lifecycle, agenda, attendees, organizer, timeline, optimistic version and idempotent cancel |
| M3 | Minutes, decisions/action items, Tasks and Dashboard |
| M4 | Google OAuth, Calendar/Meet synchronization, Scheduler reminders and notifications |
| M5 | Presigned upload, transcript, AIJob, AI Worker, multi-meeting Bedrock RAG and citations |

CloudWatch provides observability; least-privilege IAM and AWS Budgets support security and cost control.

## 2.4 Objectives

- **General:** deliver a meeting-lifecycle MVP on AWS without replacing Google Meet.
- **Functional:** cover SRS M1–M5; require preview/confirmation for AI mutations.
- **Technical:** pass `lint/typecheck/test/build/infra:validate`; validate/build IaC; expose asynchronous states, retries, and idempotency.
- **Security:** every group-scoped API validates JWT, trusted identity, active membership, role, and resource; negative cross-group tests pass.
- **Learning:** demonstrate serverless, DynamoDB access patterns, observability, cost/cleanup, and external integration through evidence.

No unsupported SLA or latency target is claimed.

## 2.5 Scope

**In scope:** SRS M1–M5; React/Vite, S3/CloudFront, Cognito, HTTP API/Lambda, five DynamoDB tables, user-content S3, Scheduler, SES, and target/IaC AI services; Google Meet as an external platform; a single-Region serverless MVP.

**Out of scope:** a custom video-conference system, multi-region active-active, commercial billing, automatic AI approval, VPC/NAT/EC2/ECS/EKS/RDS, and features outside the SRS.

## 2.6 Users and use cases

Authenticated users manage profiles, groups, invitations, and notifications. Active members/attendees view authorized meetings, timelines, sources, and assigned tasks. Group Admins manage members and meetings and, when implemented, Minutes/Tasks. The organizer is meeting-specific—not a global role—and owns Google synchronization responsibility. Main use cases map directly to M1 group/invitation, M2 meeting, M3 follow-up, M4 Google/reminders, and M5 upload/transcript/RAG/citations.

## 2.7 Functional requirements

| Module | Functions | Expected result |
|---|---|---|
| M1 | Authentication, group, membership, invitation | Only active members access group data; admins manage within role |
| M2 | CRUD/lifecycle, agenda, attendees, organizer, timeline | Consistent meetings; conflicts do not overwrite; cancel is idempotent |
| M3 | Minutes, Tasks, Dashboard | Traceable decisions/action items and follow-up state |
| M4 | OAuth, Calendar/Meet, reminders | Internal record survives sync failure; Meet URL only at `READY`; no duplicate reminders |
| M5 | Upload, transcript, AIJob, RAG/citations | Direct S3 binary path, async jobs, approved sources, cited/insufficient answers |

## 2.8 Non-functional requirements

Group-level security/privacy; managed availability and scaling without an unsupported SLA; access-pattern queries instead of `Scan` on important request paths; structured logs/request IDs/metrics/alarms; maintainable ports/repositories/shared DTOs; traceable AI citations; quotas, retention, Budgets, and cleanup; and isolation across records, logs, objects, and vectors.

## 2.9 Proposed architecture

![CampusMeet AWS architecture](/FCAJ---Workshop--aws/images/campusmeet/campusmeet-aws-architecture.png)

CloudFront serves a React build from private S3. Cognito issues JWTs; API Gateway invokes Lambda; Lambda resolves resources, authorizes membership/role, and accesses DynamoDB. Presigned URLs send content directly to private S3. Transcript/final sources create AIJobs; orchestration/AI Worker ingests approved sources into Bedrock Knowledge Bases/S3 Vectors. RAG filters group/selected meetings before generation and returns citations. Google OAuth/Calendar/Meet are outside AWS. Scheduler invokes Reminder Lambda; SES adds email delivery. CloudWatch/SNS and Budgets cover operations and cost awareness.

## 2.10 AWS service selection

| Service | CampusMeet role | Rationale |
|---|---|---|
| S3 + CloudFront | Static web/private content and edge delivery | Durable managed object/edge services |
| Cognito | User pool/JWT | Managed identity |
| API Gateway + Lambda | HTTP API, business logic, workers/reminders | Serverless/event-driven |
| DynamoDB | Five physical domain tables | Access-pattern/on-demand model |
| Scheduler + SES | One-time reminders and optional email | Managed scheduling/delivery |
| Step Functions + Transcribe | Target AI orchestration/STT | Observable long workflow/managed speech |
| Bedrock + Knowledge Bases + S3 Vectors | Generation, grounded retrieval, vector storage | Metadata-filtered RAG/citations |
| CloudWatch + SNS | Logs, metrics, alarms | Operational evidence |
| IAM + SAM/CloudFormation + Budgets | Permissions, IaC, cost alerts | Least privilege/repeatability/control |

## 2.11 Data model

The v2 model uses five physical tables: `identity`, `collaboration`, `meeting-data`, `task-data`, and `ai-work`, not the legacy 17-table design. Modules own entities/access patterns using composite keys and sparse GSIs. The backend resolves `meetingId → trusted groupId`; important request paths avoid `Scan`. Binary data belongs in S3 and vectors in S3 Vectors/Knowledge Bases.

## 2.12 Security

Cognito JWT authentication is followed by trusted-claim identity, active membership, role, and resource checks. IAM is least privilege. Frontend/user-content buckets stay private and CloudFront uses origin access. Google tokens/secrets use a secret reference, never Git/logs/frontend variables. Managed encryption is used; logs exclude credentials, JWTs, full transcripts, and live presigned URLs. Retrieval accepts approved sources, filters before model invocation, and returns authorized internal citation URIs.

## 2.13 Implementation plan

| Phase | Output | Current status |
|---|---|---|
| M1 | Identity/group/invitation/auth UI, API, tests | Core complete; auth stack previously deployed/tested |
| M2 | Meeting lifecycle/timeline code and tests | Core code complete; latest app stack not E2E deployed |
| M3 | Minutes/tasks/dashboard | Partial; handlers return `501` |
| M4 | Google/reminders | Planned/architecture; real adapter incomplete |
| M5 | Upload/transcript/AI/RAG | Partial code/IaC/tests; upload/live/E2E incomplete |
| Release | Smoke/security/operations/cleanup evidence | In progress |

## 2.14 Cost

Cost drivers include Lambda requests/compute, DynamoDB operations/storage, S3 storage/transfer, CloudFront transfer, log retention, schedules/email, Transcribe minutes, Bedrock tokens/ingestion, and vector storage. Pay-per-use/Free Tier does not guarantee zero cost. Use Budgets, quotas, retention, and workshop cleanup. Any numeric estimate is illustrative and must be rechecked with AWS Pricing Calculator and the selected Region.

## 2.15 Risks and mitigations

| Risk | Mitigation |
|---|---|
| AI/transcription/log cost | Quotas, Budgets, retention, optional labs, cleanup |
| Region/quota/model access | Availability preflight and controlled test/mock fallback |
| Google OAuth | Exact redirect URI, least scopes, secret storage, token revocation |
| Sensitive transcript | Consent, private S3, encryption, retention, redacted logs |
| Cross-group access | Trusted group resolution, active membership, negative tests |
| Retry/eventual consistency | Idempotency keys, conditional/version writes, visible job states |
| Frontend bundle | Observe Vite warnings and split only with evidence |
| Incomplete services | Mark partial/planned and demonstrate tests rather than claim E2E |

## 2.16 Deliverables

CampusMeet source code, SRS, API contract, data model, architecture diagram, SAM/CloudFormation IaC, web application, test suite, deployment guide, workshop guide, and an evaluation report that records evidence, implementation limits, security, cost, and cleanup.