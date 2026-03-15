# Design Doc Template

Use this structure when writing spec documents.

---

# {Feature Name} — Design Spec

**Date:** YYYY-MM-DD
**Status:** Draft | Approved | Implemented
**Author:** Claude + {user}

## Goal
[One sentence: what are we building and why]

## Context
[What exists today, what prompted this work, relevant constraints from CLAUDE.md]

## Non-Goals
[What we are explicitly NOT building — keeps scope clear]

## Approach
[Architecture overview, key design decisions, why this approach over alternatives]

### Components
[For each component:]
- **Name**: What it does
- **Interface**: How other components interact with it
- **Dependencies**: What it needs

### Data Flow
[How data moves through the system — can be text or diagram]

## Trade-offs Considered
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| Option A | ... | ... | Selected / Rejected |
| Option B | ... | ... | Selected / Rejected |

## Error Handling
[How errors are caught, reported, recovered from]

## Testing Strategy
[What to test, how to test it, what coverage looks like]

## Open Questions
[Anything unresolved — should be empty before approval]

---
