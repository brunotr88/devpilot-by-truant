---
name: subagent-driven-development
description: "Execute implementation plans by dispatching a fresh sub-agent per task with two-stage review. Use when you have a written plan with independent tasks to execute. Triggers: 'esegui il piano', 'implementa il piano', 'execute the plan', 'start implementation', after writing-plans completes."
---

# Subagent-Driven Development

Execute plans by dispatching a fresh sub-agent per task, with two-stage review: spec compliance first, then code quality review via the `code-reviewer` agent.

**Core principle:** Fresh sub-agent per task + two-stage review = high quality, fast iteration.

**Why sub-agents:** Isolated context per task prevents confusion. The controller constructs exactly what each sub-agent needs — they never inherit session history.

## When to Use
- Have a written implementation plan (from writing-plans)
- Tasks are mostly independent
- Want automatic review after each task

## The Process

### Step 1: Load Plan
1. Read plan file
2. Extract ALL tasks with full text
3. Review critically — raise concerns before starting
4. Create task list for tracking

### Step 2: Execute Each Task

For each task in the plan:

**2a. Dispatch Implementer Sub-Agent**
Use Agent tool (general-purpose) with:
- Full task text from the plan
- Context about where this task fits in the larger system
- List of files to create/modify
- Instruction to follow TDD (test first, verify fail, implement, verify pass, commit)

**2b. Handle Implementer Response**
- **DONE**: Proceed to review
- **NEEDS_CONTEXT**: Provide missing info, re-dispatch
- **BLOCKED**: Assess blocker — provide context, try more capable model, break into smaller pieces, or escalate to user

**2c. Spec Compliance Review**
Dispatch a sub-agent to verify:
- All plan requirements for this task are met
- Nothing extra was added (YAGNI)
- Implementation matches spec

If issues found → implementer fixes → re-review until approved.

**2d. Code Quality Review**
Dispatch the `code-reviewer` agent for code quality review.

If issues found → implementer fixes → re-review until approved.

**CRITICAL ORDER: Spec review FIRST, then code quality. Never reverse.**

**2e. Mark Task Complete**

### Step 3: Final Review
After all tasks complete:
- Dispatch `code-reviewer` for the entire implementation
- Verify all tests pass
- Present completion options to user

## Model Selection

Use the least powerful model that handles each role:
- **Mechanical tasks** (isolated functions, clear specs, 1-2 files): use fast model (haiku/sonnet)
- **Integration tasks** (multi-file coordination, pattern matching): use standard model (sonnet)
- **Architecture/review tasks**: use most capable model (opus)

## Prompt Structure for Implementers

```markdown
You are implementing Task N of [plan name].

**Task:** [Full task text from plan]

**Context:** [Where this fits — what tasks came before, what depends on this]

**Files:**
- Create: [exact paths]
- Modify: [exact paths]
- Test: [exact paths]

**Rules:**
- Follow TDD: write failing test → verify fail → implement → verify pass → commit
- Follow steps exactly as written in the plan
- Do NOT modify files outside your task scope
- If blocked or unsure, report NEEDS_CONTEXT or BLOCKED

**Return:** Summary of what you implemented, test results, commit SHA
```

## Red Flags — Never:
- Skip reviews (spec OR code quality)
- Dispatch multiple implementers in parallel (they'll conflict)
- Let implementer self-review replace actual review
- Start code quality review before spec compliance passes
- Accept "close enough" on spec compliance
- Force through blockers — stop and ask

## Integration
- **writing-plans**: Creates the plan this skill executes
- **code-reviewer**: Used for code quality review (Stage 2)
- **test-driven-development**: Implementers follow TDD
- **verification-before-completion**: Verify all tests before claiming done
- **finishing-a-development-branch**: Complete work after all tasks
- **dispatching-parallel-agents**: If 2+ independent failures arise during execution, switch to parallel dispatch for those tasks
