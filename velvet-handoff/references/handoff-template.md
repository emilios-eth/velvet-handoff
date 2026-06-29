# Implementation Handoff Packet Template

Use this packet only after the pre-handoff audit has found the north star and resolved blocking findings.

```markdown
# Implementation Handoff: [Feature]

Status: Draft | Ready For Implementation | Blocked
Last reviewed:
Source docs:

## Source Coverage
| Source type | Available? | Evidence | Caveat |
| --- | --- | --- | --- |

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

## North Star Sources
| Source | What it controls |
| --- | --- |

## Unmaterialized Decisions
| Decision | Source | Materialize in |
| --- | --- | --- |

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
| Step | Input | Output | Stop condition | Validation |
| --- | --- | --- | --- | --- |

## Stop Conditions
- ...

## Validation Plan
- ...

## Known Risks
| Risk | Mitigation | Accepted? |
| --- | --- | --- |

## Implementation Start Checklist
- [ ] North star sources are listed
- [ ] Source coverage is sufficient for the audit scope
- [ ] Unmaterialized decisions are written into this packet or another repo file
- [ ] Status is Ready For Implementation
- [ ] Open decisions are empty or non-blocking
- [ ] P0 and P1 findings are fixed
- [ ] Segments have stop conditions
- [ ] Validation plan is explicit
- [ ] User approved implementation
```
