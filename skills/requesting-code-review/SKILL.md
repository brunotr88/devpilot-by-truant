---
name: requesting-code-review
description: "Defines when and how to request code reviews using the code-reviewer agent. Use when completing tasks, implementing features, fixing bugs, or before merging. Triggers: 'rivedi il codice', 'code review', 'controlla le modifiche', 'review my changes', on completion of implementation work, before commits/PRs. Works with code-reviewer agent."
---

# Requesting Code Review

Core principle: Review early, review often. The code-reviewer agent gets precisely crafted context — never your session history.

## When to Request Review

**Mandatory** (from CLAUDE.md: "after you finish implementing a new feature call the code review agent"):
- After implementing a new feature
- After completing a major task
- Before merge to main
- Before creating a PR

**Optional but valuable:**
- When stuck on a bug (fresh perspective)
- Before refactoring (baseline check)
- After fixing a complex bug
- After significant changes to existing code

## How to Request

**Step 1: Determine scope**
```bash
# For unstaged changes (default)
git diff --stat

# For specific commit range
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
git diff --stat $BASE_SHA..$HEAD_SHA
```

**Step 2: Dispatch code-reviewer agent**

Use the Agent tool to launch the `code-reviewer` agent. Provide context:
- WHAT was implemented (brief description)
- REQUIREMENTS it should meet (plan, ticket, user request)
- SCOPE of changes (files, commit range)

**Step 3: Act on feedback**
- Fix **Critical** issues immediately — these are blockers
- Fix **Important** issues before proceeding
- Issues below confidence 80 are filtered out automatically
- Push back if reviewer is wrong — with technical reasoning

## Size-Aware Review

| Change Size | Review Depth | Sub-agents |
|-------------|-------------|------------|
| < 50 lines | Quick scan | None (orchestrator only) |
| 50-300 lines | Standard | All 3 specialists |
| > 300 lines | Deep review | All 3 + scope creep check |

## When NOT to Auto-Trigger

Do NOT auto-trigger code review for visual/creative output skills:
- **Remotion** video templates, compositions, animations
- **ComfyUI** workflows, image generation pipelines
- **Design assets** — icons, illustrations, CSS-only visual tweaks
- **Configuration-only changes** — JSON/YAML config files with no logic

These follow their own quality processes. Code review can still be requested manually if needed.

## Integration with Workflows

**Feature Development:**
- Review after EACH feature implementation
- Fix before proceeding to next feature

**Bug Fixes:**
- Review after fix to ensure no regressions
- Focus on the root cause, not just the symptom

**Refactoring:**
- Review before AND after refactoring
- Before: baseline check
- After: ensure behavior preserved

## Red Flags — Never:
- Skip review because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback without evidence

## Template

See `references/code-reviewer-template.md` for a ready-to-paste prompt template when dispatching the code-reviewer agent with full context.

## If Reviewer is Wrong:
- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification on unclear feedback
