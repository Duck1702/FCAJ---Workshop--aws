---
title: "5.2 Prepare the Environment"
date: 2026-08-05
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---
# 5.2 PREPARE THE ENVIRONMENT

**Goal:** prepare a safe local and lab environment. **Time:** 20–30 minutes. **Prerequisites:** basic Git, Node.js, and AWS knowledge.

Install Git, Node.js 22 LTS, npm 10+, AWS CLI, AWS SAM CLI, and PowerShell. Hugo builds this report and is not a CampusMeet prerequisite.

```powershell
git --version
node --version
npm --version
aws --version
sam --version
```

Clone CampusMeet from the instructor-provided URL and enter its directory. Use the Region agreed by the deployment owner; the current runbook uses `ap-southeast-1`, but check Bedrock model, Transcribe, and S3 Vectors availability before the AI lab.

Use a sandbox/dev account, MFA, and an individual principal. Only the deployment owner receives reviewed CloudFormation/SAM/IAM permissions. Run `aws login` and `aws sts get-caller-identity`, but do not copy account identifiers into the report. AI requires appropriate model access/quota; Google integration requires a reviewed OAuth client and scopes. Never commit tokens, secrets, `.env`, or credentials.

**Verify:** tool versions succeed, CampusMeet is clean, and identity/Region are correct. Common failures are an unsupported Node major, SAM missing from PATH, or the wrong profile/Region. Do not deploy before reviewing permissions and cost.

Next: [5.3 Inspect and validate source](../5.3-source-validation/).