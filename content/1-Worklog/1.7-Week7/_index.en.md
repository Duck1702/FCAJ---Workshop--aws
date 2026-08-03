---
title: "Week 7 Worklog"
date: 2026-07-31
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

# WEEK 7: AI VERTICAL SLICE AND OPERATIONS

## Tasks to be completed this week

| Day | Tasks | Start date | Completion date | References |
|:---:|---|:---:|:---:|---|
| Monday | - Learn about the Reliability Pillar in the AWS Well-Architected Framework.<br>- Learn about fault tolerance, retries, timeouts, idempotency, and recovery when a system component fails.<br>- Review the scope of the CampusMeet AI vertical slice.<br>- Define the overall workflow from Meetings and Consent to live transcription, RAG, and Task proposals. | 27/07/2026 | 27/07/2026 | [AWS Optimization](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)<br>[CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md) |
| Tuesday | - Learn about Performance Efficiency and how to select appropriate configurations for serverless workloads.<br>- Conduct a Vietnamese live-transcription technical spike using Amazon Transcribe Streaming.<br>- Analyze the tab-audio, PCM, WebSocket, partial-result, final-result, stop, and reconnect workflows.<br>- Define the rule that only final segments are stored and anonymous speaker labels such as `Speaker N` are used. | 28/07/2026 | 28/07/2026 | [AWS Optimization](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md) |
| Wednesday | - Learn about Cost Optimization and cost-control approaches for Amazon Transcribe, Amazon Bedrock, Amazon S3, and AWS Lambda.<br>- Complete the design of the S3 presigned-upload workflow.<br>- Define Attachment states, MIME-type allowlists, file-size limits, and checksum validation.<br>- Design complete-upload verification using `HeadObject` and define how to prevent duplicate AIJobs during retries. | 29/07/2026 | 29/07/2026 | [AWS Optimization](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |
| Thursday | - Design asynchronous AIJob processing using AWS Step Functions and an AI Worker.<br>- Design normalized sources, KnowledgeSource records, and the ingestion workflow.<br>- Analyze Amazon Bedrock Knowledge Bases and S3 Vectors.<br>- Design RAG for the current Meeting, selected Meetings, and the whole Group.<br>- Define mandatory metadata filters including `groupId`, `approved`, and a list of `meetingId` values when required. | 30/07/2026 | 30/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet DynamoDB v2](https://github.com/Ngct253/CampusMeet/blob/main/docs/dynamodb-data-model.md) |
| Friday | - Design the chatbot question-answering workflow and the summary feature for members who join a Meeting late.<br>- Design citations that refer to Meetings, transcripts, timestamps, or internal documents.<br>- Design draft Meeting Minutes and Task proposals with preview, confirmation, and idempotency steps.<br>- Prepare tests for cross-group data access, missing Consent, duplicate transcript segments, and duplicate AIJobs.<br>- Summarize the results and complete the Week 7 Worklog. | 31/07/2026 | 31/07/2026 | [CampusMeet SRS](https://github.com/Ngct253/CampusMeet/blob/main/docs/CampusMeet-SRS.md)<br>[CampusMeet M5 Plan](https://github.com/Ngct253/CampusMeet/blob/main/docs/ke-hoach-m5-upload-transcript-ai.md)<br>[CampusMeet Architecture](https://github.com/Ngct253/CampusMeet/tree/main/docs/architecture) |

## Week 7 results

- Learned the fundamental principles of Reliability, Performance Efficiency, and Cost Optimization on AWS.
- Understood the roles of retries, timeouts, idempotency, and fault tolerance in serverless systems.
- Finalized the CampusMeet AI vertical slice:

> Meeting and active membership  
> → Consent and capture permission  
> → Live transcription  
> → Final transcript segment  
> → Asynchronous AIJob  
> → Normalized approved source  
> → Knowledge Base and S3 Vectors  
> → RAG with citations  
> → Minutes and Task proposals  
> → User preview and confirmation

- Defined that live transcription must start only after an explicit user action grants permission.
- Defined that signed streaming connections are valid only for a short period.
- Defined that partial transcripts are displayed temporarily, are not stored in the database, and are not used to generate citations.
- Defined that final transcript segments are stored using `sessionId` and `sequence` to prevent duplicate data.
- Defined that speaker labels use anonymous formats such as `Speaker 1` and `Speaker 2` and are not automatically mapped to member identities.
- Designed the secure upload workflow:

> The frontend requests an upload  
> → The API validates the JWT and active membership  
> → An Attachment with the `PENDING_UPLOAD` state is created  
> → A short-lived presigned URL is returned  
> → The browser uploads the file directly to Amazon S3  
> → The frontend calls the complete-upload API  
> → The backend uses `HeadObject` to verify the uploaded object  
> → MIME type, file size, checksum, and object key are validated  
> → The Attachment changes to `READY` or `REJECTED`  
> → Exactly one AIJob is created when appropriate

- Defined that binary files must not be transferred through API Gateway or Lambda payloads.
- Designed the following AIJob states:

> `QUEUED`  
> `PROCESSING`  
> `COMPLETED`  
> `FAILED`  
> `CANCELLED`

- Defined that AIJobs must run asynchronously so that API requests do not wait for long-running tasks.
- Designed normalized sources and metadata for ingestion.
- Defined that only sources with `approved=true` are ingested into the Knowledge Base.
- Defined three RAG query scopes:

> Current Meeting  
> Selected Meetings  
> Whole Group

- Defined that every RAG request must validate active membership and apply the `groupId` filter before sending retrieved data to the model.
- Defined that queries covering selected Meetings must verify that every `meetingId` belongs to the correct Group.
- Defined that when there is insufficient source content, the API must return `insufficientContext=true` instead of generating an unsupported answer.
- Designed citations containing the Meeting name, source type, timestamp or document name, and an internal link.
- Designed draft Meeting Minutes containing Summary, Decision, and Action Item sections.
- Defined that AI only creates Task proposals; actual Tasks are created only after the user previews and confirms them.
- Prepared security, retry, and idempotency test cases for the AI vertical slice.

## Challenges encountered

- Browser-based live transcription depends on capture permissions and explicit user actions.
- Converting tab audio into PCM with the correct sample rate and encoding requires practical testing.
- WebSocket connections may be interrupted and require safe reconnection handling.
- Partial and final transcript results may be resent or arrive out of order.
- Transcripts, recordings, and uploaded documents may contain sensitive data.
- AIJob processing is long-running and is not suitable for normal synchronous requests.
- Building Knowledge Bases and S3 Vectors requires correctly configured permissions and metadata.
- Multi-Meeting RAG may expose cross-group data when metadata filters are applied incorrectly.
- Citations must be useful without exposing raw S3 keys or presigned URLs.
- Amazon Transcribe, Amazon Bedrock, and ingestion costs may increase when quotas are not configured.
- AI features depend on the Group, Membership, Meeting, and Task APIs operating correctly.
- Architecture diagrams or CloudFormation templates alone do not prove that the complete AI pipeline is operational.

## Solutions

- Conduct a live-transcription technical spike before implementing the complete pipeline.
- Start capture only after the user provides Consent and performs an explicit action.
- Do not automatically switch to a microphone or another audio source when tab audio is unavailable.
- Store only final transcript segments.
- Use `sessionId`, `sequence`, or `ResultId` to prevent duplicate transcript segments.
- Generate a new signed streaming URL during reconnection instead of reusing an expired URL.
- Use presigned URLs so that browsers upload files directly to Amazon S3.
- Use `HeadObject` to verify object metadata after upload.
- Apply a MIME-type allowlist, file-size limits, and checksum validation.
- Use AIJobs and AWS Step Functions for parsing, transcription, normalization, and ingestion tasks.
- Use a controlled fake provider or mock when the real AI service is not yet available.
- Apply the mandatory `groupId` and `approved=true` filters before calling the model.
- Do not retrieve data globally and filter it only after retrieval.
- Normalize citations into internal CampusMeet URIs.
- Do not write audio data, complete transcripts, prompts, tokens, secrets, or active presigned URLs to CloudWatch Logs.
- Apply quotas for transcription minutes, ingestion attempts, and AI token usage.
- Use CloudWatch Metrics and Alarms to monitor errors, processing duration, and estimated costs.
- Mark the AI vertical slice as complete only after source code, tests, deployment outputs, smoke tests, and CloudWatch Logs confirm that it works.

## Plan for the following week

- Complete the end-to-end AI vertical-slice demonstration.
- Test RAG for the current Meeting, selected Meetings, and the whole Group.
- Test mandatory `groupId` and `approved=true` filtering.
- Test that users in Group B cannot retrieve data belonging to Group A.
- Test that transcript segments and AIJobs are not duplicated during retries.
- Test that a Task proposal creates a real Task only after user confirmation.
- Test prompt-injection scenarios and untrusted document content.
- Complete CloudWatch Metrics, Alarms, and Amazon SNS notifications.
- Estimate costs for Amazon Transcribe, Amazon Bedrock, Amazon S3, AWS Lambda, and DynamoDB.
- Review data retention, cleanup, and backup requirements.
- Complete the workshop, deployment documentation, and presentation content.
- Prepare the demonstration and Week 8 final report.