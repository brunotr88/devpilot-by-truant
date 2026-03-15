---
name: verification-before-completion
description: "Enforces running verification commands before any completion claims. Use when about to claim work is done, fixed, or passing — before committing or creating PRs. Triggers: 'fatto', 'done', 'finito', 'funziona', 'fixed', 'passing', 'pronto', 'completato', about to commit/push/PR. Iron Law: no completion claims without fresh verification evidence."
---

# Verification Before Completion

## The Iron Law
```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```
If you haven't run the verification command in THIS message, you cannot claim it passes.

## The Gate Function

```
BEFORE claiming any status:
1. IDENTIFY: What command proves this claim?
2. RUN: Execute the FULL command (fresh, complete)
3. READ: Full output, check exit code, count failures
4. VERIFY: Does output confirm the claim?
   - If NO: State actual status with evidence
   - If YES: State claim WITH evidence
5. ONLY THEN: Make the claim

Skip any step = not verifying
```

## Common Verification Requirements

| Claim | Requires | NOT Sufficient |
|-------|----------|---------------|
| Tests pass | Test command: 0 failures | Previous run, "should pass" |
| Linter clean | Linter output: 0 errors | Partial check |
| Build succeeds | Build command: exit 0 | Linter passing |
| Bug fixed | Original symptom test passes | "Code changed" |
| Requirements met | Line-by-line checklist | "Tests passing" |

## Red Flags — STOP

- Using "should", "probably", "seems to"
- Expressing satisfaction before verification ("Great!", "Done!", "Perfect!")
- About to commit/push/PR without verification
- Relying on partial verification
- Thinking "just this once"
- ANY wording implying success without having run verification

## Key Patterns

**Tests:**
```
✅ [Run test command] [See: 34/34 pass] "All tests pass"
❌ "Should pass now" / "Looks correct"
```

**Build:**
```
✅ [Run build] [See: exit 0] "Build passes"
❌ "Linter passed" (linter ≠ compiler)
```

**Requirements:**
```
✅ Re-read plan → checklist → verify each → report
❌ "Tests pass, phase complete"
```

**Agent delegation:**
```
✅ Agent reports success → check diff → verify changes → report actual state
❌ Trust agent report blindly
```

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Should work now" | RUN the verification |
| "I'm confident" | Confidence ≠ evidence |
| "Just this once" | No exceptions |
| "Linter passed" | Linter ≠ compiler |
| "Agent said success" | Verify independently |
| "Partial check enough" | Partial proves nothing |

## When to Apply

**ALWAYS before:**
- Any success/completion claim
- Any positive statement about work state
- Committing, PR creation, task completion
- Moving to next task
- Reporting to user

## The Bottom Line

**No shortcuts for verification.**
Run the command. Read the output. THEN claim the result.
Non-negotiable.

## Integration
- **test-driven-development**: Verify GREEN step is mandatory
- **systematic-debugging**: Verify fix in Phase 4
- **finishing-a-development-branch**: Verify tests before presenting options
