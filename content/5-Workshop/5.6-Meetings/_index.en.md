---
title: "5.6 Practice M2 Meeting Management"
date: 2026-08-05
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---
# 5.6 PRACTICE M2 MEETING MANAGEMENT

**Goal:** complete the core user flow and a negative authorization test. **Time:** 45–60 minutes. **Prerequisites:** local web/API or an owner-deployed stack, plus two users in different groups.

Sign in as User A and select/create Group A as Group Admin. Create a future meeting with title, agenda, organizer, and active-member attendees. POST requires `Idempotency-Key`; the current UI/service follows this contract. Verify one meeting, its trusted `groupId`, details, and timeline; inactive attendees or past start times must fail.

Open the meeting in two sessions. Update agenda/attendees/organizer in the first, then submit the stale version from the second. A valid update increments the version; stale writes return a conflict rather than overwrite data.

Cancel the meeting, then repeat the same cancellation. It remains `CANCELLED`, history is not duplicated, and the record is not hard-deleted. A cancelled meeting may remain a historical AI source only when its source is approved and authorization remains valid.

Sign in as User B, who belongs only to Group B, and request Group A's `meetingId` or alter client `groupId`. Expect `403`/not-found behavior with no leaked title, attendees, agenda, or timeline. The backend resolves `meetingId → trusted groupId` and verifies active membership; it never trusts client-supplied identity/role/group.

Common results: `401` means missing/expired JWT; `403` means membership/role denial; `409` means stale version/idempotency conflict; `404/501` can mean the latest application routes are not deployed. Use local tests rather than misdiagnosing Cognito. Never record real personal data or tokens.

Next: [5.7 Tasks, Minutes, and Google](../5.7-follow-up-google/).