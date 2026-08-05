---
title: "5.5 Configure Authentication and Frontend"
date: 2026-08-05
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---
# 5.5 AUTHENTICATION AND FRONTEND

**Goal:** connect the web app to owner-provisioned Cognito/API resources. **Time:** 25–40 minutes. **Prerequisite:** three public outputs; no secret is needed.

```powershell
sam validate --template-file infra/auth-integration.yaml --lint --region <YOUR_AWS_REGION>
npm run sam:build:auth
```

Create local `apps/web/.env` and never commit it:

```dotenv
VITE_COGNITO_USER_POOL_ID=<YOUR_USER_POOL_ID>
VITE_COGNITO_USER_POOL_CLIENT_ID=<YOUR_USER_POOL_CLIENT_ID>
VITE_API_BASE_URL=<YOUR_API_URL>
```

Run `npm run dev`, open `http://localhost:5173`, complete sign-up/confirmation/sign-in, and exercise health/profile/group calls. Use `npm run build` for static output; publish to S3/CloudFront only with reviewed deployment outputs, never guessed resource names.

**Expected:** protected routes work, requests carry JWTs, and API identity comes from trusted claims. Verify refresh/sign-out and a `401` for missing/invalid tokens. Common failures: an extra API stage, wrong CORS origin, mismatched Region/pool/client, or failure to restart Vite. Never log tokens.

Next: [5.6 Meeting Management](../5.6-meetings/).