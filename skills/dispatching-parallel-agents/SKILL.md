---
name: dispatching-parallel-agents
description: "Pattern for dispatching multiple independent sub-agents in parallel. Use when facing 2+ independent tasks without shared state or sequential dependencies. Triggers: 'fix multiple bugs', 'parallel investigation', 'independent problems', 'multiple failures', 'risolvi in parallelo', 'problemi indipendenti'."
---

# Dispatching Parallel Agents

## Core Principle
One agent per independent problem domain. Let them work concurrently.

## When to Use
- 2+ independent failures (different test files, subsystems, bugs)
- Each problem can be understood without context from others
- No shared state between investigations

## When NOT to Use
- Failures are related (fix one might fix others)
- Need full system state understanding
- Agents would edit the same files
- Exploratory debugging (don't know what's broken yet)

## The Pattern

### 1. Identify Independent Domains
Group problems by what's broken. Each domain must be independent.

### 2. Create Focused Agent Tasks
Each agent gets:
- **Specific scope**: One problem domain
- **Clear goal**: What success looks like
- **Constraints**: What NOT to change
- **Expected output**: Summary of findings and changes

### 3. Dispatch in Parallel
Use the Agent tool with multiple parallel calls in a single message.

### 4. Review and Integrate
When agents return:
- Read each summary
- Verify fixes don't conflict
- Run full test suite
- Integrate all changes

## Agent Prompt Structure

```markdown
Fix the failing tests in [specific file/module]:

1. [Test name] — [expected vs actual]
2. [Test name] — [expected vs actual]

Your task:
1. Read the code and understand what each test verifies
2. Identify root cause (use systematic-debugging approach)
3. Fix the issue
4. Do NOT change files outside [scope]

Return: Summary of root cause and what you fixed.
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Too broad: "Fix all tests" | Specific: "Fix file-X.test.ts" |
| No context: "Fix the race condition" | Paste error messages and test names |
| No constraints | "Do NOT change production code" |
| Vague output | "Return summary of root cause and changes" |

## Integration
- **systematic-debugging**: Each agent follows systematic debugging
- **code-reviewer**: Review integrated result after all agents complete
