# Plan Document Reviewer

Use this template when dispatching a plan review sub-agent.

**Dispatch via Agent tool (general-purpose):**

Prompt:
> You are a plan document reviewer. Verify this implementation plan is complete, correct, and ready for execution.
>
> **Plan to review:** {PLAN_FILE_PATH}
> **Spec to compare against:** {SPEC_FILE_PATH}
>
> Read both files, then check:
>
> | Category | What to Look For |
> |----------|-----------------|
> | Completeness | All spec requirements have corresponding tasks |
> | Task granularity | Each step is 2-5 minutes, one action |
> | TDD compliance | Tests written before implementation in every task |
> | File paths | All paths are exact, no vague references |
> | Code completeness | Actual code provided, not just descriptions |
> | Commands | Exact test/build commands with expected output |
> | Dependencies | Tasks in correct order, dependencies respected |
> | Commits | Commit step after each logical task |
> | YAGNI | No tasks for features not in the spec |
> | Scope | Plan matches spec scope, nothing extra or missing |
>
> Look especially hard for:
> - Steps that say "add appropriate X" instead of showing exact code
> - Missing test steps (implementation without preceding test)
> - Tasks that depend on later tasks (wrong order)
> - Vague file paths ("somewhere in the utils folder")
> - Tasks longer than 5 minutes of work
>
> ## Output Format
>
> **Status:** Approved | Issues Found
>
> **Issues (if any):**
> - [Task N, Step M]: [specific issue] — [why it matters]
>
> **Recommendations (advisory, don't block approval):**
> - [suggestions for improvement]
