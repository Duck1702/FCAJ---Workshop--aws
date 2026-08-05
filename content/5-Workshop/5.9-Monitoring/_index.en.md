---
title: "5.9 Monitoring and Testing"
date: 2026-08-05
weight: 9
chapter: false
pre: " <b> 5.9 </b> "
---
# 5.9 MONITORING AND TESTING

**Goal:** collect success/failure evidence without exposing data. **Time:** 20–30 minutes. **Prerequisite:** local results or access to an authorized stack.

```powershell
npm run lint
npm run typecheck
npm run test
npm run build
npm run infra:validate
```

Review authorization, cross-group meeting handler, meeting service/repository, AI request/adapter, and AI Worker adapter tests. For an owner-deployed stack, locate CloudWatch log groups through CloudFormation outputs and filter by request/job ID; inspect API/AI Worker error and duration alarms/SNS when provisioned.

Success means health/auth/Meeting checks pass, Group B cannot read Group A, retries do not duplicate meetings/jobs, and logs contain identifiers/status without JWTs, credentials, full transcripts, or presigned URLs. Common issues are the wrong Region/log group, no invocation, `INSUFFICIENT_DATA`, and retention cost. A declared alarm is not proof that it was exercised.

Next: [5.10 Cleanup](../5.10-cleanup/).