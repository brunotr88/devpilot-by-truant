---
name: systematic-debugging
description: "Enforces root-cause investigation before any fix attempt. Use when encountering bugs, test failures, or unexpected behavior. Triggers: 'bug', 'errore', 'non funziona', 'test fallisce', 'crash', 'debug', 'unexpected behavior', 'broken', 'failing'. Iron Law: no fixes without root cause investigation first."
---

# Systematic Debugging

## The Iron Law
```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```
If you haven't completed Phase 1, you CANNOT propose fixes. Random fixes waste time and create new bugs.

## When to Use
Use for ANY technical issue: test failures, bugs, unexpected behavior, performance problems, build failures, integration issues.

**Use ESPECIALLY when:**
- Under time pressure (emergencies make guessing tempting)
- "Just one quick fix" seems obvious
- You've already tried multiple fixes
- Previous fix didn't work

## The Four Phases

### Phase 1: Root Cause Investigation
**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully**
   - Don't skip past errors or warnings
   - Read stack traces completely
   - Note line numbers, file paths, error codes

2. **Reproduce Consistently**
   - Can you trigger it reliably?
   - What are the exact steps?
   - If not reproducible → gather more data, don't guess

3. **Check Recent Changes**
   - What changed? Git diff, recent commits
   - New dependencies, config changes
   - Environmental differences

4. **Gather Evidence in Multi-Component Systems**
   Before proposing fixes, add diagnostic instrumentation:
   ```
   For EACH component boundary:
   - Log what data enters
   - Log what data exits
   - Verify environment/config propagation
   Run once → analyze WHERE it breaks → investigate THAT component
   ```

5. **Trace Data Flow**
   - Where does bad value originate?
   - What called this with bad value?
   - Keep tracing up until source found
   - Fix at source, not at symptom

### Phase 2: Pattern Analysis
1. **Find Working Examples** — Locate similar working code in codebase
2. **Compare Against References** — Read reference implementations COMPLETELY
3. **Identify Differences** — List every difference, however small
4. **Understand Dependencies** — What other components, settings, configs needed?

### Phase 3: Hypothesis and Testing
1. **Form Single Hypothesis** — "I think X is the root cause because Y"
2. **Test Minimally** — SMALLEST possible change, one variable at a time
3. **Verify** — Did it work? Yes → Phase 4. No → NEW hypothesis (don't stack fixes)

### Phase 4: Implementation
1. **Create Failing Test Case** — Use TDD: write test that reproduces the bug
2. **Implement Single Fix** — ONE change addressing root cause, no "while I'm here" improvements
3. **Verify Fix** — Test passes? No other tests broken?
4. **If Fix Doesn't Work** — Count attempts:
   - < 3 attempts: Return to Phase 1 with new info
   - **≥ 3 attempts: STOP — question the architecture**

### The 3-Fix Rule
If 3+ fixes have failed, this is NOT a failed hypothesis — this is a wrong architecture. Signs:
- Each fix reveals new problems in different places
- Fixes require "massive refactoring"
- Each fix creates new symptoms elsewhere

**STOP and discuss with the user before attempting more fixes.**

## Red Flags — Return to Phase 1

If you catch yourself thinking:
- "Quick fix for now, investigate later"
- "Just try changing X and see"
- "Add multiple changes, run tests"
- "It's probably X, let me fix that"
- "I don't fully understand but this might work"
- Proposing solutions before tracing data flow

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Issue is simple" | Simple issues have root causes too |
| "Emergency, no time" | Systematic is FASTER than thrashing |
| "Just try this first" | First fix sets the pattern. Do it right. |
| "Multiple fixes saves time" | Can't isolate what worked |
| "I see the problem" | Seeing symptoms ≠ understanding root cause |
| "One more fix attempt" (after 2+) | 3+ failures = architectural problem |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|-----------------|
| 1. Root Cause | Read errors, reproduce, check changes, trace data | Understand WHAT and WHY |
| 2. Pattern | Find working examples, compare | Identify differences |
| 3. Hypothesis | Form theory, test minimally | Confirmed or new hypothesis |
| 4. Implementation | Create test, fix, verify | Bug resolved, tests pass |

## Real-World Impact
- Systematic approach: 15-30 minutes to fix
- Random fixes approach: 2-3 hours of thrashing
- First-time fix rate: 95% vs 40%

## Integration
- **test-driven-development**: Create failing test in Phase 4
- **verification-before-completion**: Verify fix before claiming done
- **dispatching-parallel-agents**: Multiple independent bugs → parallel investigation
