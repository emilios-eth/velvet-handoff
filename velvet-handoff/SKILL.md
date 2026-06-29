---
name: velvet-handoff
description: Pre-handoff implementation audit for complex or lengthy work. Use when Codex must turn a messy discussion, draft plan, or implementation packet into a grounded repo-aware handoff before execution. The skill first reports source coverage, including whether chat history is visible, then finds the repo north star, extracts visible chat decisions that are not materialized in files, audits scope drift, tooling, contracts, failure handling, validation, and returns prioritized problems with concrete fixes. Do not use for simple one-step tasks unless requested.
---

# Velvet Handoff

Use Velvet Handoff right before implementation begins.

This is not a planning exercise. It is a pre-handoff audit with receipts.

## Core Job

1. Report source coverage, especially whether relevant chat history is visible.
2. Find the north star.
3. Audit the draft plan or handoff packet against that north star.
4. Return a prioritized report where every problem has evidence and a concrete fix.
5. Create or update the implementation handoff packet only when the plan is grounded enough to preserve.

## Entry Points

- `$velvet-handoff`: run the pre-handoff audit and create or update the implementation packet.
- `/prompts:velvet-handoff-execute`: optional slash prompt that starts implementation from an approved ready packet.

`/prompts:velvet-handoff-execute` is not a second skill and not a planning mode.

## Source Coverage Gate

Before judging the plan, report what evidence is actually available.

The audit must explicitly state:

- repo files inspected
- handoff packet path and status, if any
- chat source: `raw visible`, `compressed summary visible`, `decision log or packet only`, `not provided`, or `repo-only requested`
- whether the task requires chat-to-repo comparison
- whether source coverage is `sufficient`, `sufficient with caveat`, or `insufficient`
- what decision areas are out of scope because source coverage is missing

Compressed chat is usable evidence. Treat it as a lossy source, not as no source.

If compressed chat, a decision log, or a handoff packet is available and appears to cover the implementation scope:

- use it
- mark source coverage as `sufficient with caveat`
- name the caveat as `based on compressed chat/decision artifact, not raw transcript`
- do not use vague language like `cannot prove all unmaterialized decisions are captured`
- phrase the limitation as `decisions not present in the available chat artifact, decision log, or packet are out of scope for this audit`
- do not block implementation only because the raw transcript is unavailable

If the task depends on prior chat decisions and there is no visible chat artifact, decision log, or packet:

- mark source coverage as `insufficient`
- do not claim accepted decisions, rejected decisions, or unmaterialized decisions are complete
- do not return `Ready For Implementation`
- return `Revise` unless another blocker requires `Blocked`
- add a P1 finding called `Chat-to-repo coverage missing`
- set the concrete fix to: provide the relevant chat excerpt, decision log, or existing handoff packet, then rerun the audit

If the user explicitly requested a repo-only audit, say `repo-only requested` in Source Coverage and continue. In that case, do not report chat omissions as blockers.

## North Star Discovery

Before judging the plan, find the current source of truth.

Search the repo for scope and planning artifacts, especially:

- `AGENTS.md`
- `README.md`
- `docs/`
- `docs/planning/`
- `docs/specs/`
- `docs/decisions/`
- `docs/architecture/`
- `schemas/`
- tests that define expected behavior
- existing implementation files that the plan claims it will change

Also extract visible chat decisions that are not materialized in files.

If the chat history is unavailable or incomplete, say that explicitly in Source Coverage and ask for the missing excerpt, packet, or plan only when the missing context can change implementation. Do not reconstruct missing decisions from vibes.

Classify each item as:

| Type | Meaning |
| --- | --- |
| Repo north star | Existing file that defines scope, architecture, contracts, validation, or decisions. |
| Materialized decision | Chat decision already reflected in repo files. |
| Unmaterialized decision | Chat decision that matters but is not written into the repo or packet. |
| Open decision | Required choice that is still unresolved. |
| Rejected path | Thing the user explicitly rejected and the plan must not reintroduce. |

Guardrails:

- Do not invent a north star.
- Do not treat chat as implemented until it appears in a repo file or packet.
- If the repo has no source-of-truth file, report that as a blocker or required fix.

## Audit Scope

Run only audits that matter right before handoff:

| Audit | Required question |
| --- | --- |
| Scope audit | Does the plan match accepted decisions, excluded scope, rejected paths, and open decisions? |
| Repo-fit audit | Does the plan match current code paths, current files, tests, schemas, and conventions? |
| Tool-fit audit | Are the selected tools real, available, efficient enough, and appropriate for the job? |
| Contract audit | Are data, UI, error, recovery, evidence, verdict, and stop-condition contracts explicit? |
| Execution audit | Is the work split into implementation segments only where needed? |
| Validation audit | Are validation checks concrete enough to run after implementation? |

Skip audits that do not apply. Do not pad the report.

## Independent Reviewers

Use independent audit agents only when they materially reduce handoff risk, usually for P0/P1-heavy plans, expensive implementation, unclear scope, or high blast radius.

When independent audit agents are used:

- use the strongest available reasoning model unless the user explicitly asks to optimize for cost or it is unavailable
- keep the reviewer count small
- give each reviewer a bounded audit surface
- require repo evidence and concrete fixes in their findings
- ignore any finding that has no evidence or no realistic fix

## Problem Reporting Rules

Every problem must have all fields below:

| Field | Requirement |
| --- | --- |
| Severity | `P0`, `P1`, `P2`, or `P3`. |
| Problem | One concrete issue. |
| Evidence | File path with line when available, command result, visible chat decision, packet section, or named missing artifact. |
| Why it matters | Practical implementation risk, not abstract concern. |
| Fix | Realistic change tied to the repo, packet, tooling, or validation. |
| Where | File, packet section, command, tool, or implementation segment. |
| Validation | Exact check that proves the fix worked. |

Sort by severity, then by implementation order.

Severity guide:

| Severity | Meaning |
| --- | --- |
| P0 | Implementation must not start. Scope, data, safety, or validation is fundamentally blocked. |
| P1 | Implementation can start only after this is fixed. High chance of wrong build or wasted work. |
| P2 | Should fix before handoff. Manageable risk if explicitly accepted. |
| P3 | Cleanup, clarity, or minor efficiency issue. Never block on P3 alone. |

If a finding has no evidence, do not report it.

If a finding has no concrete fix, call it an open decision or omit it.

## Decision Driver

Do not leave the user with a passive findings dump.

After reporting findings, turn blocking findings into a decision queue.

Rules:

- Start with P0 findings, then P1 findings.
- Merge findings only when the same concrete decision fixes them.
- Pick one current decision. Usually choose the first P0/P1 item that blocks the most later work.
- Recommend one default option.
- Give 2 to 4 realistic options, including the recommended option.
- Tie each option to repo files, packet sections, tooling, validation, and likely tradeoff.
- Ask the user to decide only the current decision unless they explicitly ask to batch decisions.
- After the user decides, write or update the relevant packet section, then move to the next decision.
- Do not move to execution until the queue is empty or remaining items are marked non-blocking.
- If no P0/P1 decisions remain, make the current decision `Approve implementation start`.
- For `Approve implementation start`, options must be `A: mark packet Ready For Implementation and proceed`, `B: keep Draft and name the remaining concern`, and `C: rerun audit after user-supplied changes`.

Use this decision shape:

```markdown
## Decision Queue
| # | Severity | Decision needed | Blocks | Recommended option |
| --- | --- | --- | --- | --- |

## Current Decision
Decision:
Why now:
Recommendation:

Options:
| Option | What changes | Tradeoff | Validation |
| --- | --- | --- | --- |

Reply with:
- `A` to accept the recommendation
- `B` / `C` to choose another option
- `defer` only if you accept the listed blocker staying open
```

Do not ask vague questions like "What do you want to do?" Ask for a decision against named options.

## Output Format

Use this structure:

```markdown
## Velvet Handoff

Status: Ready For Implementation | Revise | Blocked

## Source Coverage
| Source type | Available? | Evidence | Coverage | Caveat |
| --- | --- | --- | --- | --- |

## North Star
| Source | What it controls | Status |
| --- | --- | --- |

## Unmaterialized Decisions
| Decision | Source | Needed file or packet section | Blocking? |
| --- | --- | --- | --- |

## Findings
| Severity | Problem | Evidence | Why it matters | Fix | Where | Validation |
| --- | --- | --- | --- | --- | --- | --- |

## Decision Queue
| # | Severity | Decision needed | Blocks | Recommended option |
| --- | --- | --- | --- | --- |

## Current Decision
Decision:
Why now:
Recommendation:

Options:
| Option | What changes | Tradeoff | Validation |
| --- | --- | --- | --- |

Reply with:
- `A` to accept the recommendation
- `B` / `C` to choose another option
- `defer` only if you accept the listed blocker staying open

## Implementation Segments
| Step | Input | Output | Stop condition | Validation |
| --- | --- | --- | --- | --- |

## Packet Status
- Path:
- Status:
- Source coverage:
- Blocking open decisions:

## Next Action
- ...
```

Keep the report short. Prefer five sharp findings over fifteen soft ones.

## Handoff Packet Contract

Default packet path for repo work:

```text
docs/planning/<project-or-feature>-implementation-handoff.md
```

Required packet sections:

```markdown
# Implementation Handoff: <feature>

Status: Draft | Ready For Implementation | Blocked
Last reviewed:
Source docs:

## Objective
## Included Scope
## Excluded Scope
## Accepted Decisions
## Rejected Decisions
## Open Decisions
## Architecture
## Tool And Data Contracts
## UI Contracts
## Error And Recovery Contracts
## Evidence And Verdict Contracts
## Implementation Segments
## Stop Conditions
## Validation Plan
## Known Risks
## Implementation Start Checklist
```

Implementation can start only when:

- packet status is `Ready For Implementation`
- source coverage is sufficient for the audit scope
- open decisions are empty or explicitly non-blocking
- P0 and P1 findings are fixed
- implementation segments have stop conditions and validation checks
- the user has approved moving from audit to implementation

If the packet is missing, stale, or contradicted by newer chat decisions, return `Blocked` or `Revise` and update the packet first.

## Execute Entry Point

Use `/prompts:velvet-handoff-execute` only when the user explicitly approves implementation.

Before editing anything:

1. Locate the handoff packet.
2. Verify status is `Ready For Implementation`.
3. Verify source coverage is sufficient for the audit scope.
4. Verify open decisions are empty or non-blocking.
5. Verify P0 and P1 findings are fixed.
6. Verify each implementation segment has input, output, stop condition, and validation.
7. Stop if chat contains newer decisions that contradict the packet.

During implementation, execute from the packet, not from loose chat memory.

## Zero Fluff Rules

- Do not write motivational framing.
- Do not use abstract frameworks unless tied to repo evidence.
- Do not write "consider", "maybe", "improve", or "ensure" without a concrete file, tool, or check.
- Do not hide missing chat context. State it in Source Coverage.
- Do not block only because raw chat is unavailable when a compressed summary, decision log, or handoff packet covers the implementation scope.
- Do not write `cannot prove all decisions are captured`; state the actual source used and what is out of scope.
- Do not approve implementation with vague validation.
- Do not approve implementation when source-of-truth files are missing.
- Do not block on cosmetic issues.
- Do not create a giant report to look thorough.
- Do not spawn low-value reviewers. If reviewers are needed, use the strongest available reasoning model and keep them scoped.
