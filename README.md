# Velvet Handoff

Velvet Handoff is a lightweight Codex skill for auditing plans before execution.

It is for complex, lengthy, expensive, ambiguous, mutating, or high-blast-radius work. It is not a planning framework and not a substitute for shipping.

**Five minutes before execution beats five hours of cleanup.**

## Flow

```mermaid
flowchart TD
    A["Complex or lengthy brainstorming"] --> B["Draft plan"]
    B --> C["Pause before execution"]
    C --> D["Quick Velvet Handoff"]
    D --> E{"High risk?"}
    E -->|No| G["Proceed, Revise, or Block"]
    E -->|Yes| F["Scope Reader + Plan/Execution Auditor"]
    B --> F
    F --> G["Proceed, Revise, or Block"]
    G --> H{"Verdict"}
    H --> I["Proceed"]
    H --> J["Revise"]
    H --> K["Blocked"]
    J --> B
```

## What It Checks

| Gate | Question |
| --- | --- |
| Scope Gate | Did we understand what the user actually wants? |
| Plan Gate | Does the plan satisfy the scope without omissions or scope creep? |
| Execution Gate | Will the plan survive contact with code, tools, cost, tests, and failure modes? |
| Handoff Format Gate | Is the plan shaped so Codex or another agent can execute it correctly? |

## What It Will Not Do

- It will not audit simple tasks unless you ask.
- It will not create long reports by default.
- It will not block execution over cosmetic issues.
- It will not call independent agents unless the risk justifies the review cost.
- It will not replace implementation.

## Modes

| Mode | Use When |
| --- | --- |
| `quick` | Normal risky task |
| `standard` | Multi-step coding, analysis, or product work |
| `full` | Expensive, mutating, architecture-level, or unclear work |
| `handoff` | You want an execution brief for Codex or another agent |

Default to `quick`. Escalate only when risk justifies it. Quick mode should fit in 3 to 5 minutes and end with execution, one concrete revision, or a real blocker.

## Install

Copy the inner `velvet-handoff/` folder that contains `SKILL.md` into your Codex skills directory.

The final path should be:

```text
~/.codex/skills/velvet-handoff/SKILL.md
```

Then invoke it with:

```text
Use $velvet-handoff to audit this plan before execution.
```

## Skill ID

The public name is **Velvet Handoff**.

The Codex skill id is `velvet-handoff` because Codex skill names use hyphen-case.
