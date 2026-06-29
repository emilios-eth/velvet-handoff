# Implementation Handoff Packet Template

Use this when the audit is ready enough to preserve the implementation plan.

```markdown
# Implementation Handoff: [Feature]

Status: Draft | Ready For Implementation | Blocked
Last reviewed:
Source docs:

## Objective
[One or two sentences.]

## Included Scope
- ...

## Excluded Scope
- ...

## Accepted Decisions
- ...

## Rejected Decisions
- ...

## Open Decisions
- None
- [Decision] - non-blocking because ...

## Architecture
- ...

## Tool And Data Contracts
| Area | Owner/tool | Input | Output | Contract |
| --- | --- | --- | --- | --- |

## UI Contracts
- ...

## Error And Recovery Contracts
| Failure | User-visible signal | Recovery | Stop condition |
| --- | --- | --- | --- |

## Evidence And Verdict Contracts
- ...

## Implementation Segments
| Step | Owner/tool | Input | Output | Stop condition | Validation |
| --- | --- | --- | --- | --- | --- |

## Stop Conditions
- ...

## Validation Plan
- ...

## Known Risks
| Risk | Mitigation |
| --- | --- |

## Implementation Start Checklist
- [ ] Status is Ready For Implementation
- [ ] Open decisions are empty or non-blocking
- [ ] Segments have stop conditions
- [ ] Validation plan is explicit
- [ ] User approved implementation
```

Keep the packet short enough to execute. Use tables for contracts, segments, risks, and validation.
