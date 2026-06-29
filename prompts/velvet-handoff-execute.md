---
description: Start implementation from a ready Velvet Handoff packet
argument-hint: "[handoff packet path]"
---

Use $$velvet-handoff in implementation stage.

Read the handoff packet at the supplied path, or locate the obvious implementation handoff packet for this repo.

Before editing anything, verify:

- packet status is `Ready For Implementation`
- source coverage is sufficient for the audit scope
- source coverage is allowed to be `sufficient with caveat` when the caveat is compressed chat, decision log, or packet coverage
- open decisions are empty or explicitly non-blocking
- north star sources are listed
- unmaterialized chat decisions are written into the packet or another repo file
- P0 and P1 findings are fixed
- implementation segments have stop conditions and validation checks
- the user has approved moving from audit to implementation

If any check fails, return `Blocked` and explain the smallest fix needed.

If all checks pass, execute one implementation segment at a time. Stop on failed validation, stale assumptions, missing data, or any packet stop condition.
