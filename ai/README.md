# AI Essentials (AIE)

This repository is the source of AIE: a portable set of commands, skills, agents, and hooks meant to be copied into other codebases. The content in `ai/` is tool-agnostic by design.

## Directory Layout

- `ai/commands/` - Workflow playbooks (test, review, refactor, etc.)
- `ai/skills/` - Reusable development patterns
- `ai/agents/` - Expert personas for complex tasks
- `ai/hooks/` - Optional automation scripts

## How To Use In Another Repo

1. Copy `ai/` into your project.
2. Add root pointer files (AGENT.md, CLAUDE.md, OPENAI.md) that point to `ai/README.md`.
3. Map rules and skills per tool. See `docs/extending-for-projects.md`.

By convention, examples use the `aie:` namespace, but you should map it to whatever your tool supports.

## Working In This Repo

- Keep content model/tool agnostic. Avoid plugin-specific language.
- Treat `ai/` as the canonical source of truth.
- Prefer small, composable skills and commands.
