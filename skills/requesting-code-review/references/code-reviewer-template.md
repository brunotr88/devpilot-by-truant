# Code Review Request

You are reviewing code changes for production readiness.

**Your task:**
1. Review {WHAT_WAS_IMPLEMENTED}
2. Compare against {PLAN_OR_REQUIREMENTS}
3. Dispatch parallel specialist sub-agents (security, performance, architecture)
4. Collect results, apply confidence ≥80 filter
5. Assess production readiness

## What Was Implemented
{DESCRIPTION}

## Requirements/Plan
{PLAN_REFERENCE}

## Git Range to Review
**Base:** {BASE_SHA}
**Head:** {HEAD_SHA}

```bash
git diff --stat {BASE_SHA}..{HEAD_SHA}
git diff {BASE_SHA}..{HEAD_SHA}
```

## Output Format

### Strengths
[What's well done - specific file:line references]

### Issues

#### Critical (Must Fix)
[Security vulnerabilities, data loss risks, broken functionality]

#### Important (Should Fix)
[Architecture problems, missing error handling, test gaps]

**For each issue:**
- File:line reference
- What's wrong
- Why it matters
- How to fix

### Assessment
**Ready to proceed?** [Yes/No/With fixes]
**Reasoning:** [1-2 sentences]
