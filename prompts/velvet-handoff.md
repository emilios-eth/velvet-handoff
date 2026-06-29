---
description: Run a repo-grounded pre-handoff audit
argument-hint: "[plan, repo path, or brief]"
---

Use $$velvet-handoff on the supplied plan or current conversation.

Start by reporting source coverage: repo files, handoff packet, raw chat, compressed chat summary, decision log, and what is out of scope.

If compressed chat, a decision log, or a handoff packet covers the implementation scope, use it and mark source coverage as `sufficient with caveat`. Do not block only because raw chat is unavailable.

If the task depends on prior chat decisions and there is no visible chat artifact, decision log, or handoff packet, do not return `Ready For Implementation`. Mark source coverage as `insufficient` and ask for the relevant chat excerpt, decision log, or existing handoff packet.

Find the repo north star first: scope files, planning docs, architecture docs, schemas, tests, and current implementation files. Extract visible chat decisions that are not materialized in repo files.

Audit only what matters right before handoff: scope drift, repo fit, tool fit, contracts, failure handling, validation, and implementation segmentation.

Return a structured report sorted by severity. Every problem must include evidence, why it matters, a concrete fix, where the fix belongs, and validation.

Create or update the implementation handoff packet only when the plan is grounded enough to preserve. Do not start implementation.
