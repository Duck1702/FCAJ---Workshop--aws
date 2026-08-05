---
title: "5.3 Inspect Source and Validate the Project"
date: 2026-08-05
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---
# 5.3 INSPECT SOURCE AND VALIDATE THE PROJECT

**Goal:** understand the monorepo and establish a quality baseline. **Time:** 25–40 minutes. **Prerequisite:** complete 5.2 and work from the CampusMeet root.

`apps/web` contains React/Vite; `services/api` the Lambda API; `services/ai-worker` AI processing; `packages/shared` contracts; `infra` SAM/CloudFormation; `docs` authoritative design; and `scripts` validation helpers.

```powershell
npm install
npm run lint
npm run typecheck
npm run test
npm run build
npm run format:check
npm run infra:validate
```

With SAM available, validate without deploying:

```powershell
npm run sam:validate:data -- --region ap-southeast-1
npm run sam:validate:app -- --region ap-southeast-1
npm run sam:build:app
```

**Expected:** dependencies install and quality/infra commands exit 0. Record command names and exit codes, never secrets. Common failures include a mismatched Node/lockfile, missing SAM/Docker, or Region-aware lint. Do not weaken contracts merely to make tests pass.

Next: [5.4 Data foundation and backend](../5.4-backend/).