---
description: Run a repo-grounded pre-handoff audit
argument-hint: "[plan, repo path, or brief]"
---

Use $velvet-handoff on the supplied plan or current conversation.

Find the repo north star first: scope files, planning docs, architecture docs, schemas, tests, and current implementation files. Extract visible chat decisions that are not materialized in repo files.

Audit only what matters right before handoff: scope drift, repo fit, tool fit, contracts, failure handling, validation, and implementation segmentation.

Return a structured report sorted by severity. Every problem must include evidence, why it matters, a concrete fix, where the fix belongs, and validation.

Create or update the implementation handoff packet only when the plan is grounded enough to preserve. Do not start implementation.
