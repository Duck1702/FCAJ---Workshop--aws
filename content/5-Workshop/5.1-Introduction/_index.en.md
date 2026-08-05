---
title: "5.1 Introduction to the CampusMeet Workshop"
date: 2026-08-05
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---
# 5.1 INTRODUCTION TO THE CAMPUSMEET WORKSHOP

## Introduction

CampusMeet centralizes schedules, agendas, participants, minutes, and follow-up tasks. It manages the meeting lifecycle; Google Meet remains an external video-meeting platform. This workshop turns Part 2 into an evidence-based path through source code, tests, IaC, and observable results.

## Measurable objectives

Learners will identify the CloudFront/S3 → Cognito → API Gateway/Lambda → DynamoDB flow; run `lint`, `typecheck`, `test`, `build`, and `infra:validate`; configure the three public frontend variables without committing `.env`; exercise M1 and core Meeting behavior including cross-group denial; explain the upload/transcript/AIJob/RAG/citation boundary; inspect CloudWatch evidence; and clean up safely.

## Workshop architecture

![CampusMeet AWS architecture](/FCAJ---Workshop--aws/images/campusmeet/campusmeet-aws-architecture.png)

React/TypeScript/Vite is distributed from private S3 through CloudFront. Cognito issues JWTs; HTTP API invokes Lambda; the backend verifies active membership and role before accessing the five-table DynamoDB model. User content, Transcribe, Step Functions, AI Worker, Bedrock Knowledge Bases, and S3 Vectors form the target AI path. Google OAuth, Calendar, and Meet are external systems. CloudWatch, SNS, IAM, SAM/CloudFormation, and Budgets support operations. The baseline contains no VPC, NAT Gateway, EC2, ECS, EKS, RDS, or subnet.

## End-to-end scenario

1. Sign in and create or join a group (core).
2. Manage invitations and members as Group Admin (core).
3. Create a meeting with agenda, organizer, and attendees (core).
4. View the timeline, update with optimistic versioning, and repeat cancellation to test idempotency (core).
5. Verify that a user from another group receives no data/authorization (core).
6. Review Tasks/Minutes/Dashboard (partial: handlers still return `501`).
7. Review Google and reminder integration (architecture walkthrough).
8. Run AI adapter tests and inspect AIJob/RAG/citations (advanced; not a mandatory live-cloud lab).

## Scope and duration

| Track | Content | Time |
|---|---|---:|
| 5.1–5.3 | Architecture, environment, source validation | 45–60 min |
| 5.4–5.5 | Conditional data/auth/backend/frontend setup | 45–75 min |
| 5.6 | Core Meeting flow | 45–60 min |
| 5.7–5.9 | Partial domains, AI, monitoring | 30–60 min |
| 5.10 | Cleanup | 15–30 min |

Core covers M1/M2. AI is optional; live transcription, complete upload, Google lifecycle, and reminders are architecture-only until E2E evidence exists. Building a video platform, production deployment, commercial billing, and multi-region active-active are out of scope.

Transcribe, Bedrock, Knowledge Bases, S3 Vectors, storage, and logs can incur charges. Do not use a production account or commit credentials. Verify current prices with AWS Pricing Calculator and complete cleanup.

Next: [5.2 Prepare the environment](../5.2-environment/).