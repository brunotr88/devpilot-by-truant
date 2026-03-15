---
name: receiving-code-review
description: "Defines how to process and act on code review feedback. Use when the code-reviewer agent returns results. Ensures systematic handling of review findings — verify before implementing, push back when wrong, implement one fix at a time. Triggers: 'gestisci feedback review', 'handle review feedback', 'review results'. Works with code-reviewer agent."
---

# Code Review Reception

## Core Principle
Verify before implementing. Ask before assuming. Technical correctness over social comfort.

## The Response Pattern

```
WHEN receiving code review feedback:
1. READ: Complete feedback without reacting
2. UNDERSTAND: Restate requirement in own words (or ask)
3. VERIFY: Check against codebase reality
4. EVALUATE: Technically sound for THIS codebase?
5. RESPOND: Technical acknowledgment or reasoned pushback
6. IMPLEMENT: One item at a time, test each
```

## Forbidden Responses

**NEVER say:**
- "You're absolutely right!"
- "Great point!" / "Excellent feedback!"
- "Let me implement that now" (before verification)

**INSTEAD:**
- Restate the technical requirement
- Ask clarifying questions if unclear
- Push back with technical reasoning if wrong
- Just start fixing (actions > words)

## Handling Unclear Feedback

```
IF any item is unclear:
  STOP — do not implement anything yet
  ASK for clarification on unclear items

WHY: Items may be related. Partial understanding = wrong implementation.
```

## Implementation Order

For multi-item feedback:
1. Clarify anything unclear FIRST
2. Then implement in this order:
   - Critical issues (security, data loss, crashes)
   - Simple fixes (typos, imports, naming)
   - Complex fixes (refactoring, logic changes)
3. Test each fix individually
4. Verify no regressions after all fixes

## YAGNI Check

```
IF reviewer suggests "implement properly" or adding features:
  Check: Is this actually used/needed?
  IF unused: Flag it — "This isn't used. YAGNI — remove or keep?"
  IF used: Then implement properly
```

## When to Push Back

Push back when:
- Suggestion breaks existing functionality
- Reviewer lacks full context
- Violates YAGNI (unused feature suggested)
- Technically incorrect for this stack
- Legacy/compatibility reasons exist
- Conflicts with CLAUDE.md or project constraints

**How to push back:**
- Use technical reasoning, not defensiveness
- Reference working tests/code
- Ask specific questions
- Involve the user if architectural decision needed

## Acknowledging Correct Feedback

When feedback IS correct:
- "Fixed. [Brief description of what changed]"
- "Good catch — [specific issue]. Fixed in [location]."
- Or just fix it silently and show the diff

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Performative agreement | State requirement or just act |
| Blind implementation | Verify against codebase first |
| Batch without testing | One at a time, test each |
| Assuming reviewer is right | Check if it breaks things |
| Avoiding pushback | Technical correctness > comfort |
| Partial implementation | Clarify all items first |

## Integration
- **requesting-code-review**: Triggers the review that produces the feedback this skill handles
- **code-reviewer** agent: Produces the review report
- **test-driven-development**: Fixes should follow TDD (test the fix first)
- **verification-before-completion**: Re-verify all tests after applying fixes
