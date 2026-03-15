---
name: finishing-a-development-branch
description: "Guides completion of development work with structured options. Use when implementation is complete, tests pass, and you need to integrate the work. Triggers: 'finito', 'pronto per merge', 'ready to merge', 'create PR', 'branch complete', 'integra il lavoro'."
---

# Finishing a Development Branch

## Core Principle
Verify tests -> Present options -> Execute choice -> Clean up.

## The Process

### Step 1: Verify Tests
Run the project's test suite. If tests fail -> STOP, fix first. Cannot proceed until green.

### Step 2: Determine Base Branch
```bash
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

### Step 3: Present Options
Present exactly these 4 options:
```
Implementation complete. What would you like to do?

1. Merge back to [base] locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

### Step 4: Execute Choice

**Option 1 -- Merge Locally:**
```bash
git checkout [base] && git pull && git merge [branch]
# Verify tests on merged result
git branch -d [branch]
```

**Option 2 -- Push and Create PR:**
```bash
git push -u origin [branch]
gh pr create --title "[title]" --body "## Summary\n..."
```

**Option 3 -- Keep As-Is:**
Report location. Don't cleanup.

**Option 4 -- Discard:**
Require typed "discard" confirmation first. Then delete branch.

### Step 5: Cleanup Worktree (Options 1, 2, 4)
```bash
git worktree remove [path]  # if in worktree
```

## Red Flags -- Never:
- Proceed with failing tests
- Merge without verifying tests on result
- Delete work without confirmation
- Force-push without explicit request

## Integration
- **verification-before-completion**: Verify tests before offering options
- **using-git-worktrees**: Clean up worktree after completion
- **subagent-driven-development**: Called after all tasks complete
