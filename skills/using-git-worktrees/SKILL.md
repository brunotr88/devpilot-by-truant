---
name: using-git-worktrees
description: "Creates isolated git worktrees for feature work. Use when starting work that needs isolation from the current workspace, or before executing implementation plans. Triggers: 'worktree', 'workspace isolato', 'isolated workspace', 'branch di lavoro', before executing plans."
---

# Using Git Worktrees

## Core Principle
Systematic directory selection + safety verification = reliable isolation.

## Directory Selection Priority

1. **Check existing**: `.worktrees/` or `worktrees/` in project root
2. **Check CLAUDE.md**: Any worktree directory preference specified
3. **Ask user**: If nothing found, offer options

## Safety Verification

For project-local directories, verify they're git-ignored:
```bash
git check-ignore -q .worktrees 2>/dev/null
```
If NOT ignored -> add to `.gitignore` and commit before proceeding.

## Creation Steps

### 1. Create Worktree
```bash
project=$(basename "$(git rev-parse --show-toplevel)")
git worktree add .worktrees/$BRANCH_NAME -b $BRANCH_NAME
cd .worktrees/$BRANCH_NAME
```

### 2. Run Project Setup
Auto-detect from project files:
```bash
[ -f package.json ] && npm install
[ -f requirements.txt ] && pip install -r requirements.txt
[ -f Cargo.toml ] && cargo build
[ -f go.mod ] && go mod download
```

### 3. Verify Clean Baseline
Run tests to ensure worktree starts clean. If tests fail -> report, ask whether to proceed.

### 4. Report
```
Worktree ready at [path]
Tests passing ([N] tests, 0 failures)
Ready to implement
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Skip ignore verification | Always `git check-ignore` first |
| Assume directory location | Follow priority: existing > CLAUDE.md > ask |
| Proceed with failing tests | Report failures, get permission |
| Hardcode setup commands | Auto-detect from project files |

## Integration
- **subagent-driven-development**: Create worktree before plan execution
- **finishing-a-development-branch**: Cleanup worktree after completion
