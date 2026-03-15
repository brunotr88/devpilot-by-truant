---
name: brainstorming
description: "Collaborative design and planning before implementation. Use when entering plan mode, starting a new project, designing features, or before any multi-step implementation. Triggers: 'pianifichiamo', 'progettiamo', 'brainstorming', 'facciamo un piano', 'plan mode', 'come dovremmo implementare', 'prima di iniziare', 'let's plan', 'design this'. NOT for visual/creative skills (Remotion, ComfyUI). Explores intent, requirements, and architecture before writing code."
---

# Brainstorming Ideas Into Designs

Turn ideas into fully formed designs through collaborative dialogue enhanced with structured reasoning.

**Core principle:** Think before you build. Design before you code. One question at a time.

Do NOT write any code, scaffold any project, or take implementation action until you have presented a design and the user has approved it. This applies to EVERY project regardless of perceived simplicity.

## Anti-Pattern: "This Is Too Simple To Need A Design"

Every project goes through this process. A todo list, a utility function, a config change — all of them. "Simple" projects are where unexamined assumptions cause the most wasted work. The design can be short (a few sentences for truly simple projects), but you MUST present it and get approval.

## When NOT to Trigger

Do NOT auto-trigger for visual/creative output skills:
- Remotion video templates and compositions
- ComfyUI workflows and image generation
- Design assets (icons, illustrations)

These follow their own quality processes. Brainstorming can still be invoked manually for these if the user requests it.

## Process Checklist

Complete these steps in order:

### Phase 0: Context Gathering
- Read all relevant CLAUDE.md files in the project hierarchy
- Check project memory (if available) for prior decisions
- Explore project state: files, docs, recent commits, existing patterns
- Use `sequential-thinking` to synthesize context into key constraints and opportunities

### Phase 1: Understanding the Idea
- Before asking detailed questions, assess scope: if the request describes multiple independent subsystems (e.g., "build a platform with chat, billing, and analytics"), flag this immediately. Don't spend questions refining details of a project that needs decomposition first.
- If too large for a single spec, help decompose into sub-projects. Each sub-project gets its own spec → plan → implementation cycle.
- For appropriately-scoped projects, ask clarifying questions **one at a time**
- Prefer **multiple choice** questions when possible (easier to answer)
- Focus on: purpose, constraints, success criteria, target users
- Use `sequential-thinking` when a question reveals complex trade-offs that need structured analysis before asking the next question

### Phase 2: Exploring Approaches
- Use `sequential-thinking` to analyze 2-3 different approaches with trade-offs
- Present options conversationally with your recommendation and reasoning
- Lead with your recommended option and explain WHY
- Each approach should cover: architecture, complexity, timeline implications, risks

### Phase 3: Presenting the Design
- Present the design section by section
- Scale each section to its complexity: a few sentences if straightforward, up to 200-300 words if nuanced
- Ask after each section whether it looks right so far — get incremental approval
- Cover: architecture, components, data flow, error handling, testing strategy
- Be ready to revise if something doesn't make sense

**Design for isolation and clarity:**
- Break the system into smaller units with one clear purpose each
- Units communicate through well-defined interfaces
- Each unit can be understood and tested independently
- For each unit, answer: what does it do, how do you use it, what does it depend on?

**Working in existing codebases:**
- Explore the current structure before proposing changes. Follow existing patterns.
- Where existing code has problems affecting the work, include targeted improvements — the way a good developer improves code they're working in.
- Don't propose unrelated refactoring. Stay focused on the current goal.

### Phase 4: Writing the Spec Document
After design approval:
- Write the validated design to `docs/specs/YYYY-MM-DD--{name}.md`
  - User preferences or CLAUDE.md for spec location override this default
- Structure using the template in `references/design-doc-template.md`
- Commit the document to git

### Phase 5: Spec Review Loop
After writing the spec:
1. Dispatch a sub-agent (Agent tool, general-purpose) using the template in `references/spec-reviewer-prompt.md`
2. If Issues Found: fix, re-dispatch, repeat
3. If loop exceeds 5 iterations, surface to human for guidance
4. Once approved, ask the user to review the written spec

### Phase 6: Transition to Implementation Planning
After user approves the spec:
> "Spec approved. Transitioning to writing-plans to create the implementation plan."

Invoke the `writing-plans` skill to create a detailed, bite-sized implementation plan from the approved spec.

## Using Sequential-Thinking

Use the `mcp__sequential__sequentialthinking` tool at these key decision points:

1. **After context gathering** — synthesize constraints, identify risks, form initial hypotheses (2-3 thoughts)
2. **Before proposing approaches** — explore trade-offs, compare architectures, identify blind spots (3-5 thoughts)
3. **During design decisions** — when a design choice has non-obvious implications, think through consequences (2-4 thoughts)
4. **When the user's answer reveals complexity** — before asking the next question, reason about what the answer implies (1-2 thoughts)

Do NOT use sequential-thinking for simple factual questions or when the path is clear. Use it when structured reasoning adds value.

## Key Principles

- **One question at a time** — Don't overwhelm with multiple questions
- **Multiple choice preferred** — Easier to answer than open-ended
- **YAGNI ruthlessly** — Remove unnecessary features from all designs
- **Explore alternatives** — Always propose 2-3 approaches before settling
- **Incremental validation** — Present design, get approval before moving on
- **Be flexible** — Go back and revise when something doesn't make sense
- **Think structured** — Use sequential-thinking for complex decisions

## Visual Companion (Optional)

If Puppeteer MCP tools are available and the topic involves visual questions (UI mockups, layout comparisons, architecture diagrams):

Offer once: "Some upcoming questions might be easier to show visually in a browser. Want me to use visual mockups when relevant?"

Per-question decision: only use browser for content that IS visual (mockups, wireframes, layout comparisons). Use terminal for text content (requirements, tradeoffs, scope decisions).
