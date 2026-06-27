---
name: velvet-handoff
description: Lightweight pre-execution audit for complex, lengthy, expensive, ambiguous, mutating, or high-blast-radius work. Use when the user asks to audit a plan, prepare an execution handoff, check for omissions or scope creep, verify tool/codebase fit before implementation, or explicitly invokes $velvet-handoff. Do not use for simple one-step tasks unless requested.
---

# Velvet Handoff

Use Velvet Handoff to stop bad plans before they enter execution. Keep it lightweight: run the smallest review that can prevent wasted effort.

## Default Behavior

Start with `quick` mode unless the user requests more.

Return one of:

- `Proceed`: plan is clear enough to execute
- `Revise`: plan needs fixes before execution
- `Blocked`: missing decision, unsafe assumption, or major execution risk

Never use this skill as a substitute for implementation. If the plan is good enough, say so and move on.

## Modes

| Mode | Use When | Output |
| --- | --- | --- |
| `quick` | Normal risky task | 5 to 10 bullets |
| `standard` | Multi-step coding, analysis, or product work | Short structured sections |
| `full` | Expensive, mutating, architecture-level, or unclear work | Scope contract plus audit report |
| `handoff` | User wants a plan passed to Codex or another agent | Execution handoff brief |

Escalate only when the task is long-running, costly, mutating, architecture-level, unclear, safety-sensitive, or high blast radius.

## Workflow

1. Freeze the draft plan.
2. Extract the actual scope from the user's request.
3. Audit the plan against that scope.
4. Audit execution readiness against the codebase, tools, costs, tests, and failure modes.
5. Audit whether the handoff format is easy for Codex to execute.
6. Return `Proceed`, `Revise`, or `Blocked`.

When independent agents are available and the risk is high, use separate reviewers:

- Scope Reader: extracts the user's real scope, rejections, requirements, and open decisions.
- Plan Critic: compares the draft plan against the Scope Contract.
- Execution Critic: checks tooling, codebase fit, efficiency, validation, and failure handling.

If independent agents are unavailable, do separated passes in the same response.

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

Default compact output:

```markdown
## Velvet Handoff

Verdict: Proceed | Revise | Blocked

Scope:
- ...

Risks:
- ...

Execution:
- ...

Format:
- ...

Next:
- ...
```

Full output:

```markdown
## Scope Contract
- Agreed:
- Rejected:
- Required:
- Non-goals:
- Open decisions:

## Plan Audit
- Passes:
- Omissions:
- Contradictions:
- Scope creep:

## Execution Audit
- Codebase/tool fit:
- Failure handling:
- Efficiency:
- Validation:

## Handoff Audit
- Format issues:
- Segmentation needed:
- Suggested handoff shape:

## Verdict
Proceed | Revise | Blocked
```

## Segmentation Check

Decide whether the plan should be split into steps before execution.

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

## Anti-Ceremony Rules

- Do not use Velvet Handoff for simple tasks unless requested.
- Do not create long reports by default.
- Do not invent architecture.
- Do not ask for extra research unless it changes execution.
- Do not require independent agents unless task risk is high.
- Do not block execution over cosmetic issues.
- Prefer "Proceed with these notes" over process theater.
