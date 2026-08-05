---
title: "5.10 Cleanup"
date: 2026-08-05
weight: 10
chapter: false
pre: " <b> 5.10 </b> "
---
# 5.10 CLEANUP

**Goal:** stop charges without deleting a shared environment. **Time:** 15–30 minutes. **Prerequisite:** the deployment owner confirms exact stacks/resources and retention.

{{% notice warning %}}
Never execute deletion commands containing placeholders. Learners must not delete shared stacks. Review which buckets, vectors, and knowledge sources contain retained data first.
{{% /notice %}}

1. Revoke/delete Google test credentials in Google Console when created; never commit them.
2. Stop live sessions/jobs and check for orphaned one-time EventBridge Scheduler schedules.
3. Remove test user-content under the retention policy; CloudFormation cannot remove non-empty/versioned buckets.
4. Inspect Bedrock Knowledge Base/data source, S3 Vectors bucket/index, and ingestion jobs; follow dependency order only when real AI resources exist.
5. The owner removes the **application stack first**, then the auth stack when unused, and the data stack last after approved export/deletion.
6. Verify CloudFront/frontend S3, Lambda/API/Cognito, Scheduler, SES, SNS, alarms/log groups, and DynamoDB are gone or intentionally retained.
7. Review Billing/Cost Explorer and AWS Budgets; log retention can continue to cost money.

The current runbook requires reviewed cleanup but provides no universal safe destroy command, so this workshop does not invent a `sam delete` command or stack name. Use the CloudFormation Console or an owner-reviewed command with the exact `<YOUR_STACK_NAME>`.

Cleanup is complete only when lab stacks, unintended objects, schedules/jobs, and Google tokens are gone and Billing/Budgets are checked. Record intentionally retained resources and their owner.