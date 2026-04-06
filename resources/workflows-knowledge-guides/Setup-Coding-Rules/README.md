# Setup Coding Rules

A quick-start guide for installing language-aware coding rules from [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) into your Claude Code environment.

## What You Get

These rules provide Claude Code with structured guidance via `~/.claude/rules/` files that activate automatically based on file type:

- **common/** - Language-agnostic principles (coding style, git workflow, testing, security, patterns, hooks, agents, performance, code review, development workflow)
- **typescript/** - TypeScript/JavaScript specific rules (coding style, hooks, patterns, security, testing)
- **python/** - Python specific rules (coding style, hooks, patterns, security, testing)

Rules use YAML frontmatter `paths` to scope themselves to relevant file types (e.g., `**/*.ts` for TypeScript rules).

## Installation

### Option 1: Clone and copy (recommended)

```bash
git clone https://github.com/affaan-m/everything-claude-code.git

# Always install common rules (language-agnostic)
cp -r everything-claude-code/rules/common ~/.claude/rules/

# Then pick your language(s)
cp -r everything-claude-code/rules/typescript ~/.claude/rules/
cp -r everything-claude-code/rules/python ~/.claude/rules/
```

### Option 2: Copy from this resource directory

```bash
# From the awesome-claude-code repo root
cp -r resources/workflows-knowledge-guides/Setup-Coding-Rules/rules/common ~/.claude/rules/
cp -r resources/workflows-knowledge-guides/Setup-Coding-Rules/rules/typescript ~/.claude/rules/
cp -r resources/workflows-knowledge-guides/Setup-Coding-Rules/rules/python ~/.claude/rules/
```

### Verify installation

```bash
ls ~/.claude/rules/common/
ls ~/.claude/rules/typescript/  # if installed
ls ~/.claude/rules/python/      # if installed
```

## Rules Overview

### Common (always install)

| File | Purpose |
|------|---------|
| coding-style.md | Immutability, KISS/DRY/YAGNI, file organization, naming conventions |
| git-workflow.md | Commit message format, PR workflow |
| testing.md | 80% coverage minimum, TDD workflow, AAA pattern |
| security.md | Pre-commit security checklist, secret management |
| patterns.md | Skeleton projects, repository pattern, API response format |
| hooks.md | Hook types, auto-accept permissions, TodoWrite practices |
| agents.md | Agent orchestration table, parallel execution |
| performance.md | Model selection, context window management, extended thinking |
| code-review.md | Review triggers, checklists, severity levels |
| development-workflow.md | Research-first, plan, TDD, review, commit pipeline |

### TypeScript/JavaScript (scoped to `**/*.ts`, `**/*.tsx`, `**/*.js`, `**/*.jsx`)

| File | Purpose |
|------|---------|
| coding-style.md | Types/interfaces, avoid `any`, React props, Zod validation, no console.log |
| hooks.md | Prettier auto-format, tsc checks, console.log warnings |
| patterns.md | API response interface, custom hooks, repository pattern |
| security.md | Environment variable secret management |
| testing.md | Playwright for E2E testing |

### Python (scoped to `**/*.py`, `**/*.pyi`)

| File | Purpose |
|------|---------|
| coding-style.md | PEP 8, type annotations, frozen dataclasses, black/isort/ruff |
| hooks.md | black/ruff auto-format, mypy/pyright type checking |
| patterns.md | Protocol pattern, dataclass DTOs, context managers |
| security.md | dotenv secret management, bandit scanning |
| testing.md | pytest framework, coverage, markers |

## Source

From [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) by [Affaan Mustafa](https://github.com/affaan-m/) (MIT License).
