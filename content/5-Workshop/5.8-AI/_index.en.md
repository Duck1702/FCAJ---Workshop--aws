---
title: "5.8 Upload, Transcription, and AI"
date: 2026-08-05
weight: 8
chapter: false
pre: " <b> 5.8 </b> "
---
# 5.8 UPLOAD, TRANSCRIPTION, AND AI

**Goal:** understand security boundaries and run existing validation/tests. **Time:** 25–40 minutes. **Prerequisite:** 5.3 passes; cloud AI only after owner confirms model, quota, and IaC.

{{% notice info %}}
This is an **Advanced/Architecture walkthrough**. Upload and live-transcript endpoints remain unimplemented; the full pipeline is not presented as runnable.
{{% /notice %}}

The API authenticates JWT, resolves `meetingId → trusted groupId`, and checks active membership. Browser uploads use a presigned URL followed by backend `HeadObject` checks. Only final segments persist, with anonymous `Speaker N` labels. AIJob progresses through `QUEUED`, `PROCESSING`, terminal states, with idempotent retries. Only `approved=true` sources are ingested. Retrieval applies `groupId` before generation and validates every selected meeting. RAG covers current/selected/whole-group scopes and returns citations or `insufficientContext=true`. AI produces drafts/proposals; a human confirms through standard APIs. Cancelled meetings may remain approved historical sources but cannot be mutated.

```powershell
npm run test -- services/api/tests/ai-request-service.test.ts
npm run test -- services/api/tests/ai-aws-adapters.test.ts
npm run test -- services/ai-worker/tests/aws-adapters.test.ts
npm run infra:validate
```

Passing tests demonstrate fixture-level isolation, adapter/error mapping, and citation behavior—not cloud E2E. Common failures include model/Region access, incorrect filters, unapproved sources, or duplicate jobs. Never log full transcripts, prompts, tokens, raw S3 keys, or live presigned URLs. AI/transcription/ingestion incur cost.

Next: [5.9 Monitoring and testing](../5.9-monitoring/).