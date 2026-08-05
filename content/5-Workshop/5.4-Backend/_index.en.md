---
title: "5.4 Deploy the Data Foundation and Backend"
date: 2026-08-05
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---
# 5.4 DATA FOUNDATION AND BACKEND

**Goal:** preview the IaC path and deploy only when authorized. **Time:** 30–45 minutes. **Prerequisites:** passing gates and deployment-owner review of account, Region, IAM, cost, and `infra/parameters.example.json`.

{{% notice warning %}}
CampusMeet has no E2E deployment evidence for the current application stack. The core workshop may stop after validation/build. A valid template is not proof of a working deployment.
{{% /notice %}}

Deploy the data stack before the application stack. Follow `docs/huong-dan-trien-khai-aws.md`: validate `infra/data-foundation.yaml`, create a non-executed change set, review replacements/deletions, and let only the owner execute it. Use agreed stack names and parameters.

```powershell
powershell -NoProfile -File scripts/verify-data-foundation.ps1 `
  -Region <YOUR_AWS_REGION> `
  -TablePrefix <YOUR_TABLE_PREFIX> `
  -ExpectedAccountId <YOUR_ACCOUNT_ID>

npm run infra:validate
npm run sam:validate:app -- --region <YOUR_AWS_REGION>
npm run sam:build:app
```

**Verify:** five physical tables (`identity`, `collaboration`, `meeting-data`, `task-data`, `ai-work`) and reviewed CloudFormation outputs/status. Common failures are a mismatched prefix, `iam:PassRole`, missing bucket/model parameters, or Region support. Return to the runbook/change set; do not make untracked Console fixes.

Next: [5.5 Authentication and frontend](../5.5-auth-frontend/).