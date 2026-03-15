- ask me clarifying questions (one per prompt) until you're 95% sure you understand all the requirements

## Development Workflow — Skill Invocation Rules

Follow these rules to invoke the correct skill at each phase of development. Skills are in the SkillsCreator project and agents are in ~/.claude/agents/.

### Planning Phase
- When entering plan mode, starting a new project, or designing a feature → use the **brainstorming** skill (explore context, ask questions, propose approaches, write spec)
- After the spec is approved → use the **writing-plans** skill (create bite-sized TDD implementation plan)

### Execution Phase
- When you have a written plan to execute → use the **subagent-driven-development** skill (fresh sub-agent per task + two-stage review)
- When facing 2+ independent problems (different bugs, different subsystems) → use the **dispatching-parallel-agents** skill
- **Decision rule:** If during plan execution you encounter 2+ independent failures (different test files, different subsystems), switch to dispatching-parallel-agents for those specific tasks, then resume subagent-driven-development for the remaining plan

### Quality Phase
- When implementing any feature or bugfix → follow the **test-driven-development** skill (Iron Law: no production code without a failing test first)
- When encountering any bug, test failure, or unexpected behavior → follow the **systematic-debugging** skill (Iron Law: no fixes without root cause investigation first)
- Before claiming any work is complete, fixed, or passing → follow the **verification-before-completion** skill (Iron Law: no completion claims without fresh verification evidence)
- After finishing implementing a new feature → call the **code-reviewer** agent and implement its suggestions. Auto-review applies to: web applications, websites, APIs, Python programs, backend services, CLI tools, scripts. Do NOT auto-trigger code review for visual/creative output skills (remotion video templates, comfyui workflows, image generation, design assets) — these follow their own quality processes.

### Completion Phase
- When implementation is complete and tests pass → use the **finishing-a-development-branch** skill (verify tests → present 4 options → execute → cleanup)
- When starting feature work that needs isolation → use the **using-git-worktrees** skill
