# AI Essentials (AIE)

AIE is a portable set of markdown files for AI-assisted development. It is model/tool agnostic and meant to be copied
into your codebase so you can use it however your agent supports. Forked
from [Riley Hilliard's claude-code-essentials](https://github.com/rileyhilliard/claude-essentials).

The goal is to be able to drop a shared `ai/` directory into any repo, point your agent at it, and get consistent workflows
without tying yourself to a specific tool.

## Quick Start (Copy Into Your Repo)

1. Create an `ai/` directory in your project.
2. Copy the AIE content from this repo.

Commands: `plugins/ce/commands` -> `ai/commands`
Skills: `plugins/ce/skills` -> `ai/skills`
Agents: `plugins/ce/agents` -> `ai/agents`
Hooks: `plugins/ce/hooks` -> `ai/hooks` (optional)

3. Add `ai/README.md` with project overview, architecture, and quick commands.
4. Add root pointer files so tools can find `ai/`:

````markdown
# Claude Configuration

All AI agent documentation lives in the [`ai/`](./ai/) directory.

**Start here:** [ai/README.md](./ai/README.md)
````

5. Map rules and skills per tool. See [docs/extending-for-projects.md](docs/extending-for-projects.md).

By convention, this repo refers to skills as `aie:<name>`, but the namespace is yours to map to your tool of choice.

## What's Included

### Commands

Command playbooks you can wire into your tool's command system or invoke manually.

| Command                                           | Description                                                   |
|---------------------------------------------------|---------------------------------------------------------------|
| [test](ai/commands/test.md)               | Run tests and analyze failures                                |
| [explain](ai/commands/explain.md)         | Break down code or concepts                                   |
| [debug](ai/commands/debug.md)             | Launch systematic debugging                                   |
| [optimize](ai/commands/optimize.md)       | Find performance bottlenecks                                  |
| [refactor](ai/commands/refactor.md)       | Improve code quality                                          |
| [review](ai/commands/review.md)           | Code review with tracked findings and fix workflow            |
| [commit](ai/commands/commit.md)           | Preflight checks, semantic commit, auto-fix on hook failure   |
| [deps](ai/commands/deps.md)               | Audit and upgrade dependencies                                |
| [fix-issue](ai/commands/fix-issue.md)     | Fix a GitHub issue by number                                  |
| [pr](ai/commands/pr.md)                   | Create a pull request with auto-generated description         |
| [document](ai/commands/document.md)       | Create or improve documentation                               |
| [plan](ai/commands/plan.md)               | Create a detailed implementation plan                         |
| [execute](ai/commands/execute.md)         | Execute an implementation plan from the plans folder          |
| [init](ai/commands/init.md)               | Bootstrap repo with config (rules, permissions, settings)     |
| [post-mortem](ai/commands/post-mortem.md) | Review a session to assess execution and extract improvements |

### Skills

Reusable development patterns.

**Testing & Quality:**

| Skill                                                                                       | Description                                             |
|---------------------------------------------------------------------------------------------|---------------------------------------------------------|
| [writing-tests](ai/skills/writing-tests/SKILL.md)                                   | Testing Trophy methodology, behavior-focused tests      |
| [verification-before-completion](ai/skills/verification-before-completion/SKILL.md) | Verify before claiming success                          |
| [preflight-checks](ai/skills/preflight-checks/SKILL.md)                             | Auto-detect and run project linters/formatters/checkers |

**Debugging & Problem Solving:**

| Skill                                                                         | Description                                   |
|-------------------------------------------------------------------------------|-----------------------------------------------|
| [systematic-debugging](ai/skills/systematic-debugging/SKILL.md)       | Four-phase debugging framework                |
| [fixing-flaky-tests](ai/skills/fixing-flaky-tests/SKILL.md)           | Diagnose and fix tests that fail concurrently |
| [condition-based-waiting](ai/skills/condition-based-waiting/SKILL.md) | Replace race conditions with polling          |
| [reading-logs](ai/skills/reading-logs/SKILL.md)                       | Efficient log analysis using targeted search  |

**Code Quality:**

| Skill                                                                       | Description                                                 |
|-----------------------------------------------------------------------------|-------------------------------------------------------------|
| [refactoring-code](ai/skills/refactoring-code/SKILL.md)             | Behavior-preserving code improvements                       |
| [optimizing-performance](ai/skills/optimizing-performance/SKILL.md) | Measurement-driven optimization                             |
| [handling-errors](ai/skills/handling-errors/SKILL.md)               | Error handling best practices                               |
| [migrating-code](ai/skills/migrating-code/SKILL.md)                 | Safe migration patterns for databases, APIs, and frameworks |

**Planning & Execution:**

| Skill                                                                   | Description                                             |
|-------------------------------------------------------------------------|---------------------------------------------------------|
| [writing-plans](ai/skills/writing-plans/SKILL.md)               | Create implementation plans with devils-advocate review |
| [executing-plans](ai/skills/executing-plans/SKILL.md)           | Execute plans with mandatory code review                |
| [architecting-systems](ai/skills/architecting-systems/SKILL.md) | Clean, scalable system architecture for the build phase |
| [design](ai/skills/design/SKILL.md)                             | Frontend design skill                                   |

**Documentation & Writing:**

| Skill                                                                             | Description                                                                                               |
|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| [writer](ai/skills/writer/SKILL.md)                                       | Writing style guide with 7 personas (Architect, Engineer, PM, Marketer, Educator, Contributor, UX Writer) |
| [strategy-writer](ai/skills/strategy-writer/SKILL.md)                     | Executive-quality strategic documents in Economist/HBR style                                              |
| [documenting-systems](ai/skills/documenting-systems/SKILL.md)             | Best practices for writing markdown documentation                                                         |
| [documenting-code-comments](ai/skills/documenting-code-comments/SKILL.md) | Standards for self-documenting code and inline comments                                                   |

**Data & Infrastructure:**

| Skill                                                               | Description                                                         |
|---------------------------------------------------------------------|---------------------------------------------------------------------|
| [managing-databases](ai/skills/managing-databases/SKILL.md) | PostgreSQL, DuckDB, Parquet, and PGVector architecture              |
| [managing-pipelines](ai/skills/managing-pipelines/SKILL.md) | GitHub Actions CI/CD security, performance, and deployment patterns |

**Meta Skills:**

| Skill                                                                           | Description                                         |
|---------------------------------------------------------------------------------|-----------------------------------------------------|
| [visualizing-with-mermaid](ai/skills/visualizing-with-mermaid/SKILL.md) | Create professional technical diagrams              |
| [post-mortem](ai/skills/post-mortem/SKILL.md)                           | Review sessions to extract actionable improvements  |
| [configuring-claude](ai/skills/configuring-claude/SKILL.md)             | Best practices for writing skills, rules, CLAUDE.md |

### Agents

Expert personas for complex work.

| Agent                                                   | Description                                          |
|---------------------------------------------------------|------------------------------------------------------|
| [code-reviewer](ai/agents/code-reviewer.md)     | Comprehensive PR/MR reviews enforcing standards      |
| [haiku](ai/agents/haiku.md)                     | Lightweight agent for simple delegated tasks         |
| [log-reader](ai/agents/log-reader.md)           | Efficient log file analysis using targeted search    |
| [devils-advocate](ai/agents/devils-advocate.md) | Rigorous critique to find flaws in plans and designs |

### Hooks

Optional shell scripts for session automation.

- **Session start** - Auto-detects project tooling (linters, formatters, type checkers) and injects available skills
- **Notifications** - Cross-platform alerts when an agent needs input, with git branch info (macOS + Linux)

## Usage Examples

These are tool-agnostic examples. Use the equivalent mechanism in your agent.

**Fix failing tests:**

- Ask your agent to follow the command in `ai/commands/test.md`.
- If the failure is complex, load `aie:systematic-debugging`.

**Review before merge:**

- Run the workflow in `ai/commands/review.md`.
- Apply the fixes and follow `ai/commands/commit.md`.

**Plan a feature:**

- Load `aie:architecting-systems` for design.
- Then use `aie:writing-plans` to produce an implementation plan.

## Customization

All components are just markdown files. Want to customize? Edit them directly in your repo.

### Creating Your Own Command

Add a markdown file to `ai/commands/`:

```markdown
---
description: Your command description
argument-hint: "[optional-arg]"
allowed-tools: Bash, Read
---

Your command instructions here.
```

### Creating Your Own Skill

Add a directory with `SKILL.md` to `ai/skills/`:

```markdown
---
name: my-skill
description: What this skill does and when to use it
---

# Skill Instructions

Your skill workflow here.
```

### Creating Your Own Agent

Add a markdown file to `ai/agents/`:

```markdown
---
name: my-agent
description: Expert at specific domain
tools: Read, Grep, Glob, Bash
color: blue
---

Your agent personality and workflow here.
```

## Project Structure (Recommended)

```
your-project/
├── AGENT.md
├── CLAUDE.md
├── OPENAI.md
└── ai/
    ├── README.md
    ├── commands/
    ├── skills/
    ├── agents/
    ├── rules/
    └── hooks/
```

## Documentation

- [Extending for Projects](docs/extending-for-projects.md) - How to adapt AIE for your specific codebase

## Contributing

Found a bug? Have an idea? Contributions welcome.

1. Fork this repo
2. Create a feature branch
3. Test your changes locally
4. Submit a PR with details

Ideas for contributions:

- New commands for common workflows
- Additional skills for specific patterns
- Specialized agents for other domains
- Documentation improvements

## Resources

- [Model Context Protocol](https://modelcontextprotocol.io/)

## License

MIT - Use it, share it, make it better.
