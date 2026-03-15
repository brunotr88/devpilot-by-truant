---
name: writing-plans
description: "Creates detailed, bite-sized implementation plans from specs or requirements. Use when you have a design spec ready and need to create step-by-step implementation tasks before writing code. Triggers: 'crea il piano', 'implementation plan', 'piano implementativo', 'scrivi il piano', 'plan the implementation', after brainstorming completes. Each task is 2-5 minutes with TDD, exact file paths, and exact commands."
---

# Writing Implementation Plans

Create comprehensive, bite-sized implementation plans that assume the implementer has zero context about the codebase. Document everything: which files to touch, exact code, exact test commands, exact expected output. DRY. YAGNI. TDD. Frequent commits.

**Announce at start:** "Creating implementation plan from the approved spec."

## Scope Check

Before writing the plan, verify:
- Is the spec focused on a single system/feature? If it covers multiple independent subsystems, suggest separate plans — one per subsystem.
- Each plan should produce working, testable software on its own.
- Use `sequential-thinking` to decompose complex specs into logical implementation phases.

## Step 1: File Structure Mapping

Before defining tasks, use `sequential-thinking` to map out:
- Which files will be created or modified
- What each file is responsible for
- How files relate to each other (dependencies, imports)
- Where tests will live

Design principles:
- Units with clear boundaries and well-defined interfaces
- Each file has one clear responsibility
- Prefer smaller, focused files over large ones
- Files that change together should live together
- In existing codebases, follow established patterns

This structure informs task decomposition. Each task should produce self-contained changes.

## Step 2: Task Decomposition

**Each step is one action (2-5 minutes):**
- "Write the failing test" — step
- "Run it to make sure it fails" — step
- "Implement the minimal code to make the test pass" — step
- "Run the tests and make sure they pass" — step
- "Commit" — step

Use `sequential-thinking` to verify:
- Are tasks in the right order? (dependencies respected)
- Can each task be completed independently?
- Is TDD flow maintained? (test first, implement, verify)
- Are commit points at logical boundaries?

## Plan Document Format

Every plan MUST start with this header:

```markdown
# {Feature Name} Implementation Plan

**Goal:** [One sentence describing what this builds]
**Spec:** [Link to design spec document]
**Architecture:** [2-3 sentences about approach]
**Tech Stack:** [Key technologies/libraries]

---
```

### Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Plan Output Location

Save plans to: `docs/plans/YYYY-MM-DD--{name}.md`
- User preferences or CLAUDE.md settings override this default

## Plan Review Loop

After completing each chunk of the plan (chunks ≤1000 lines, logically self-contained):

1. Dispatch a sub-agent (Agent tool, general-purpose) using the template in `references/plan-reviewer-prompt.md`
2. If Issues Found: fix issues, re-dispatch reviewer
3. If loop exceeds 5 iterations, surface to human for guidance
4. Once all chunks approved, present to user

## Completion

After the plan is approved:

> "Plan complete and saved to `docs/plans/{filename}.md`. To begin implementation, invoke the **subagent-driven-development** skill."

## Key Rules

- **Exact file paths** — always, no "somewhere in src/"
- **Complete code in plan** — not "add validation", show the actual code
- **Exact commands with expected output** — `pytest tests/path.py -v`, expect PASS
- **DRY, YAGNI, TDD** — no exceptions
- **Frequent commits** — after each task, not at the end
- **Checkbox syntax** — `- [ ]` for every step, enables tracking
- **Sequential-thinking** for decomposition — verify task order and independence
