---
name: velvet-handoff
description: Single-path pre-execution audit and implementation readiness check for complex, lengthy, expensive, ambiguous, mutating, or high-blast-radius work. Use when the user asks to audit a plan, prepare implementation, check for omissions or scope creep, verify tool/codebase fit before implementation, or explicitly invokes $velvet-handoff. Do not use for simple one-step tasks unless requested.
---

# Velvet Handoff

Use Velvet Handoff to stop bad plans before they enter execution.

This skill has one audit path. Do not ask the user to choose a mode.

There are two invocations:

1. `velvet-handoff`: audit the plan and create or update an implementation handoff packet.
2. `velvet-handoff-execute`: start implementation from an approved handoff packet.

`velvet-handoff-execute` is not a second audit mode. It is the implementation trigger after the packet is ready.

## Default Behavior

Run one integrated audit that is deep enough to prevent wasted implementation work and compact enough to avoid process theater.

If the user invokes Velvet Handoff before implementation, produce an execution-ready audit:

- scope contract
- accepted, rejected, and open decisions
- plan audit
- architecture audit
- tool and data contracts
- UI contracts when relevant
- error and recovery contracts
- evidence and verdict contracts when relevant
- implementation segmentation
- validation plan
- known risks
- implementation readiness verdict

If the plan is too vague to audit, return `Blocked` or `Revise` and name the smallest concrete missing decision.

Return one of:

- `Ready For Implementation`: packet is complete enough to execute after user approval
- `Revise`: packet needs fixes before execution
- `Blocked`: missing decision, unsafe assumption, stale packet, or major execution risk

Never use this skill as a substitute for implementation. If the packet is ready, say so and wait for explicit user approval to execute.

## Handoff Packet Contract

The audit stage and implementation stage are connected by a file called the handoff packet.

Default packet path for repo work:

```text
docs/planning/<project-or-feature>-implementation-handoff.md
```

For the X Pods revamped pipeline, use:

```text
docs/planning/revamped-pipeline-implementation-handoff.md
```

The handoff packet is the source of truth for implementation. It must be created or updated at the end of the audit when the plan is ready enough to preserve. Implementation must not begin from chat context alone.

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
- open decisions are empty or explicitly non-blocking
- implementation segments have stop conditions and validation checks
- the user has approved moving from audit to implementation

If the packet is missing, stale, or contradicted by newer chat decisions, return `Blocked` and update the packet first.

## Velvet Handoff Workflow

1. Freeze the draft plan.
2. Extract the actual scope from the user's request and decisions.
3. Separate included scope, excluded scope, accepted decisions, rejected decisions, and open decisions.
4. Audit the plan against that scope.
5. Audit architecture, tools, data contracts, UI contracts, failure handling, costs, tests, and blast radius.
6. Decide whether implementation must be segmented.
7. Create or update the handoff packet.
8. Return `Ready For Implementation`, `Revise`, or `Blocked`.

When independent agents are available and the risk is high, use separate reviewers only when the cost of bad execution clearly exceeds the cost of review:

- Scope Reader: extracts the user's real scope, rejections, requirements, and open decisions.
- Plan Critic: compares the draft plan against the Scope Contract.
- Execution Critic: checks tooling, codebase fit, efficiency, validation, and failure handling.

If independent agents are unavailable, do separated passes in the same response.

## Velvet Handoff Execute Workflow

Use this only when the user explicitly asks to move forward with implementation or invokes `velvet-handoff-execute`.

1. Locate the handoff packet.
2. Verify packet status is `Ready For Implementation`.
3. Verify open decisions are empty or marked non-blocking.
4. Verify each implementation segment has inputs, outputs, stop conditions, and validation.
5. Confirm the user approved implementation.
6. Execute one segment at a time.
7. Stop at any stop condition, failed validation, stale assumption, or new blocking decision.
8. Update the user with what changed, what was validated, and what remains.

Do not implement from loose chat memory when a packet exists or should exist.

## Checks

### Scope Gate

Verify:

- the objective is explicit
- included and excluded scope are separated
- accepted, rejected, and open decisions are distinct
- user rejections are not reintroduced
- the plan does not expand beyond the request

### Plan Gate

Verify:

- every major requirement is addressed
- the plan is ordered
- assumptions are named
- success criteria are observable
- unresolved decisions are not hidden

### Execution Gate

Verify:

- selected tools fit the task
- the plan uses current code paths, not obsolete ones
- required data or APIs are actually available
- failure modes are visible and recoverable
- validation is named
- time, cost, and blast radius are bounded

### Handoff Format Gate

Verify the plan is easy for Codex or another agent to execute.

Preferred format: structured Markdown with short sections and tables.

Use the template in `references/handoff-template.md` when the user asks for a handoff or when the plan is complex.

## Output Format

Use this structure unless the user explicitly asks for a shorter answer:

```markdown
## Velvet Handoff

Status: Ready For Implementation | Revise | Blocked

## Scope Contract
- Objective:
- Included:
- Excluded:
- Accepted decisions:
- Rejected decisions:
- Open decisions:

## Plan Audit
- Passes:
- Omissions:
- Contradictions:
- Scope creep:

## Execution Audit
- Architecture:
- Codebase/tool fit:
- Tool/data contracts:
- UI contracts:
- Error/recovery contracts:
- Evidence/verdict contracts:
- Validation:

## Implementation Segments
| Step | Owner/tool | Input | Output | Stop condition | Validation |

## Packet Status
- Path:
- Updated:
- Blocking decisions:

## Next
- ...
```

## Segmentation Check

Decide whether the plan should be split into implementation segments.

Suggest segmentation when:

- more than 3 files or modules are affected
- work spans multiple stages or tools
- there are separate discovery, implementation, and validation phases
- user approval is needed midstream
- failure in one step should stop later steps

Recommended segment fields:

| Field | Meaning |
| --- | --- |
| Step | Ordered action |
| Owner/tool | Agent, script, API, browser, local code |
| Input | What the step consumes |
| Output | What the step produces |
| Stop condition | When not to continue |
| Validation | How to check it |

If segmentation is needed, return the smallest useful step list. Do not build a project plan unless the user asked for one.

## Anti-Ceremony Rules

- Do not use Velvet Handoff for simple tasks unless requested.
- Do not create long reports by default.
- Do not invent architecture.
- Do not ask for extra research unless it changes execution.
- Do not require independent agents unless task risk is high.
- Do not block execution over cosmetic issues.
- Do not expose multiple Velvet modes to the user.
- Do not start implementation without a `Ready For Implementation` packet and explicit user approval.
