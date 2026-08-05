---
title: "5.7 Tasks, Minutes, and Google Integration"
date: 2026-08-05
weight: 7
chapter: false
pre: " <b> 5.7 </b> "
---
# 5.7 TASKS, MINUTES, AND GOOGLE INTEGRATION

**Goal:** distinguish target capabilities from runnable implementation. **Time:** 20–30 minutes. **Prerequisite:** complete 5.6.

| Area | Current state | Workshop treatment |
|---|---|---|
| Minutes, Tasks, Dashboard | Skeleton handlers return `501` | Inspect contracts/shared DTOs/tests |
| Google OAuth/Calendar/Meet | Design fixed; real adapter incomplete | Architecture walkthrough |
| Reminder/Scheduler/SES | IaC resources/roles exist; no E2E lifecycle evidence | Optional inspection |

The SRS defines minutes with summary/discussion/decisions/action items; an action item becomes a Task only after confirmation. Tasks progress `TODO → DOING → DONE`. Internal meetings survive Google sync failure; a Meet URL appears only at `READY`. Reminder processing is idempotent and skips cancelled meetings.

Verify `docs/api-contract.md`, the minutes/tasks/dashboard handlers, and integration adapter. Treat `501` as the expected boundary, not success. Common mistakes are treating organizer as a global role, describing Google Meet as the CampusMeet backend, or committing OAuth secrets. Use least-privilege scopes.

Next: [5.8 Upload, Transcription, and AI](../5.8-ai/).