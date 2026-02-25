# Extending AI Essentials for Your Projects

AIE (AI Essentials) provides generic development patterns. Real projects need project-specific context. This guide shows how to wrap and extend AIE in a model/tool agnostic way so it can live inside your codebase.

## The Extension Pattern

AIE is intentionally generic. Your project's `ai/` directory should:

1. **Centralize AI context** in `ai/` (skills, rules, commands, hooks)
2. **Add root pointer files** (AGENT.md, CLAUDE.md, OPENAI.md, etc.) that point to `ai/README.md`
3. **Reference aie skills** in your rules (don't duplicate them)
4. **Add project commands** that wrap your actual tooling
5. **Create project skills** for domain-specific knowledge
6. **Configure hooks** to enforce project conventions

## Directory Structure

```
your-project/
├── AGENT.md                   # Pointer to ai/ (model/tool-specific)
├── CLAUDE.md                  # Pointer to ai/
├── OPENAI.md                  # Pointer to ai/
├── GEMINI.md                  # Pointer to ai/
└── ai/
    ├── README.md              # Project overview, architecture, quick commands
    ├── commands/              # Project-specific slash commands
    │   ├── myproject:test.md
    │   ├── myproject:dev.md
    │   └── myproject:deploy.md
    ├── skills/                # Domain knowledge
    │   └── myproject-models/
    │       └── SKILL.md
    ├── rules/                 # Auto-injected context by file path
    │   ├── testing.md
    │   └── backend/
    │       └── api.md
    └── hooks/                 # Pre/post tool scripts
        └── lint.sh
```

## 1. Root Pointer Files (AGENT.md, CLAUDE.md, OPENAI.md, etc)

Keep these files thin. They exist so each tool/model can locate the shared `ai/` content:

````markdown
# <Agent> Configuration

All AI agent documentation lives in the [`ai/`](./ai/) directory.

**Start here:** [ai/README.md](./ai/README.md)
````

If a tool requires a specific filename or directory, keep that file as a pointer and do not duplicate the content in multiple places.

## 2. Project AI README (ai/README.md)

Keep this focused on what an assistant needs to work in your codebase:

````markdown
# MyProject

Brief description of what the project does.

## Architecture

High-level structure. What lives where.

## Quick Commands

```bash
make dev          # Start development
make test         # Run tests
make lint         # Lint and format
```

## Key Patterns

Project-specific conventions assistants should follow.
````

## 3. Shared Rules and Skill References

Rules are shared content in `ai/rules/`, but each tool decides how they get attached (path globs, directory scoping, or manual include). Reference AIE skills from those rules instead of duplicating content:

````markdown
# Testing Rules

When writing tests, load the aie:writing-tests skill for general patterns.

## Project-Specific Patterns

| Area    | HTTP Mocking | Notes                    |
| ------- | ------------ | ------------------------ |
| Python  | `respx`      | Must match exact URL     |
| Frontend| `msw`        | Handlers in tests/mocks/ |

## Async Waiting

When fixing flaky tests, load the aie:condition-based-waiting skill.
````

Per-tool mapping examples (pick the one your agent supports):
- Path-glob rules: map `**/*.test.*` to `ai/rules/testing.md`
- Directory-scoped rules: add an `AGENT.md` under `tests/` that points to `ai/rules/testing.md`
- Manual include: add a short note in `CLAUDE.md`/`OPENAI.md` to include `ai/rules/testing.md` when working on tests

This keeps rules lightweight while connecting to the deeper AIE knowledge.

## 4. Project Commands

Create commands that wrap your actual workflows. Namespace them to avoid conflicts:

````markdown
---
description: Run tests on a remote runner
argument-hint: [target]
---

Run tests using the project's remote runner (preferred over local).

```bash
projectctl test              # All tests
projectctl test backend      # Backend only
projectctl test -x           # Stop on first failure
```

For advanced usage, load the projectctl:runner skill.
````

> Example only. Replace `projectctl` and skill names with your actual tooling.

Commands should be thin wrappers that show:
- What to run
- Common variations
- Where to find more info (skills)

## 5. Project Skills

Create skills for domain knowledge that doesn't fit in rules:

````markdown
---
name: myproject-models
description: Data model patterns for MyProject. Use when creating new models, modifying schemas, or understanding relationships.
---

# Model Development

## Hierarchy

1. **Base models** - Shared fields, timestamps
2. **Domain models** - Business logic
3. **View models** - Query-time transforms

## Creating a New Model

Location: `src/models/{domain}.py`

```python
from myproject.models.base import BaseModel

class Widget(BaseModel):
    """Document the business purpose."""

    name: str
    status: WidgetStatus
```

## Key Files

- `src/models/base.py` - Base classes
- `src/schemas/` - Pydantic schemas
````

## 6. Rules Organization

Organize rules by scope. More specific paths take precedence:

```
rules/
├── testing.md           # All test files
├── error-handling.md    # Global error patterns
├── frontend/
│   ├── testing.md       # Frontend-specific test patterns
│   └── components.md    # React component patterns
└── backend/
    ├── api.md           # API endpoint patterns
    └── testing.md       # Backend test patterns
```

Each rule file uses path matching:

```markdown
---
paths:
  - frontend/**/*.tsx
  - frontend/**/*.ts
---

# Frontend Rules

React patterns for this project...
```

## Complete Example

Here's how a real project combines everything:

**`ai/rules/testing.md`** - References AIE skills:
```markdown
---
paths:
  - "**/*.test.*"
  - "**/test_*.py"
---

# Testing

Load aie:writing-tests for Testing Trophy patterns.
Load aie:condition-based-waiting for async test fixes.

## Project Specifics

- Python: Use `respx` for HTTP mocking
- Frontend: Use `msw` with handlers in `tests/mocks/`
- Always run tests via `make test-remote`
```

**`ai/commands/myproject:test.md`** - Wraps tooling:
````markdown
---
description: Run project tests
argument-hint: [area]
---

```bash
make test                      # All tests
make test-backend              # Backend only
make test-backend ARGS="-x"    # Stop on first failure
```
````

**`ai/skills/myproject-api/SKILL.md`** - Domain knowledge:
```markdown
---
name: myproject-api
description: API development patterns. Use when creating endpoints, handling auth, or designing responses.
---

# API Development

## Route Structure

All routes in `src/api/routes/`. Thin handlers that delegate to services...
```

## Key Principles

1. **Don't duplicate AIE content** - Reference skills, don't copy them
2. **Keep commands thin** - Show what to run, link to skills for details
3. **Scope rules narrowly** - More specific paths = more relevant context
4. **Use hooks for enforcement** - Block bad patterns, auto-fix on stop
5. **Document the "why"** - `ai/README.md` explains architecture decisions
