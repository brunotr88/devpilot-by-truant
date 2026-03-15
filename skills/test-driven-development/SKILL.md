---
name: test-driven-development
description: "Enforces test-first development. Use when implementing any feature or bugfix, before writing implementation code. Triggers: 'implementa', 'scrivi la funzione', 'aggiungi feature', 'fix bug', 'implement', 'write code', 'add feature', 'bugfix'. Iron Law: no production code without a failing test first. NOT for visual/creative skills (Remotion, ComfyUI)."
---

# Test-Driven Development (TDD)

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? Delete it. Start over. No exceptions.

## When to Use

**Always:** New features, bug fixes, refactoring, behavior changes.

**Exceptions (ask user):** Throwaway prototypes, generated code, configuration files.

**Do NOT auto-trigger for:** Remotion templates, ComfyUI workflows, design assets.

## Red-Green-Refactor Cycle

### RED — Write Failing Test

1. Write one minimal test showing what should happen.
2. Use a clear name describing the behavior under test.
3. Use real code, no mocks unless unavoidable.
4. Test one behavior per test function.

### Verify RED — Watch It Fail (MANDATORY)

```bash
# Run the test — use whatever the project uses
pytest / npm test / go test / cargo test / dotnet test
```

Confirm all three:

- Test **FAILS** (not errors — a failure, not a crash).
- Failure message matches what you expected.
- It fails **because the feature is missing**, not because of a typo or import error.

Test passes? You are testing existing behavior. Fix the test.

### GREEN — Minimal Code

Write the **SIMPLEST** code that makes the test pass.

- Do not add features beyond what the test requires.
- Do not refactor other code.
- Do not "improve" beyond the test.
- YAGNI applies here.

### Verify GREEN — Watch It Pass (MANDATORY)

Confirm all three:

- The new test passes.
- All other tests still pass.
- Output is clean (no warnings, no skipped tests).

### REFACTOR — Clean Up (only after green)

- Remove duplication.
- Improve names.
- Extract helpers.
- Keep tests green throughout. Do not add behavior during refactoring.

### Repeat

Pick the next behavior. Write the next failing test. Continue the cycle.

## Good Tests vs Bad Tests

| Quality | Good | Bad |
|---------|------|-----|
| **Minimal** | One thing. "and" in name? Split it. | `test_validates_email_and_domain_and_whitespace` |
| **Clear** | Name describes behavior | `test1` |
| **Real** | Tests actual code | Tests mock behavior |
| **Intent** | Shows desired API | Obscures what code should do |

## Why Order Matters

**"I'll write tests after"** — Tests written after pass immediately. Passing immediately proves nothing. You never saw it catch the bug.

**"Already manually tested"** — Manual testing is ad-hoc. No record, can't re-run, easy to forget cases.

**"Deleting X hours of work is wasteful"** — Sunk cost fallacy. Working code without real tests is technical debt.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Need to explore first" | Fine. Throw away exploration, start with TDD. |
| "Test hard = design unclear" | Hard to test = hard to use. Simplify interface. |
| "TDD will slow me down" | TDD is faster than debugging. |
| "Must mock everything" | Code too coupled. Use dependency injection. |

## Red Flags — STOP and Start Over

- Code written before test.
- Test passes immediately (testing existing behavior).
- Cannot explain why the test failed.
- Tests added "later."
- "Just this once."
- "Keep as reference" (delete means delete).

## Verification Checklist

Before marking work complete:

- [ ] Every new function/method has a test.
- [ ] Watched each test fail before implementing.
- [ ] Each test failed for the expected reason.
- [ ] Wrote minimal code to pass each test.
- [ ] All tests pass.
- [ ] Tests use real code (mocks only if unavoidable).
- [ ] Edge cases and errors covered.

## Integration

- **systematic-debugging**: Bug found -> write failing test -> TDD cycle.
- **verification-before-completion**: Verify ALL tests pass before claiming done.
- **writing-plans**: Plans include TDD steps (test first, verify fail, implement, verify pass, commit).
