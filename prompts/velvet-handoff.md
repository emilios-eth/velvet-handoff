---
description: Audit a plan and produce a ready-for-implementation handoff packet
argument-hint: "[plan, repo path, or brief]"
---

Use $velvet-handoff on the supplied plan or current conversation.

Create or update an implementation handoff packet. Do not start implementation.

The packet must include objective, included scope, excluded scope, accepted decisions, rejected decisions, open decisions, architecture, tool and data contracts, UI contracts, error and recovery contracts, evidence and verdict contracts, implementation segments, stop conditions, validation plan, known risks, and implementation start checklist.

Return `Ready For Implementation`, `Revise`, or `Blocked`.

Implementation can start only when the packet says `Ready For Implementation`, open decisions are empty or non-blocking, and the user approves moving forward.
