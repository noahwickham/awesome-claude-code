# CLAUDE.md — Awesome Claude Code

## Project Overview

This is **Awesome Claude Code**, a curated list of skills, agents, plugins, hooks, and tools for Claude Code. It's a "full-stack" Awesome list hosted entirely on GitHub — a single CSV data source drives multiple generated README styles via a Python-based pipeline.

Repository: `github.com/anthropics/awesome-claude-code`

## Architecture

### Data Flow

```
THE_RESOURCES_TABLE.csv  (single source of truth)
        ↓
templates/categories.yaml  (category definitions)
acc-config.yaml            (style/root config)
templates/*.template.md    (style templates)
        ↓
scripts/readme/generate_readme.py  (generator)
        ↓
README.md                          (root style copy)
README_ALTERNATIVES/README_*.md    (all style variants)
assets/*.svg                       (generated badges/headers)
```

### Key Files

| File | Purpose |
|------|---------|
| `THE_RESOURCES_TABLE.csv` | Master resource data — all entries live here |
| `acc-config.yaml` | Root style selection (`readme.root_style`) and style selector config |
| `templates/categories.yaml` | Single source of truth for categories and subcategories |
| `templates/*.template.md` | Markdown templates per style (Extra, Classic, Awesome) |
| `templates/announcements.yaml` | Announcement content |
| `templates/resource-overrides.yaml` | Per-resource display overrides |
| `pyproject.toml` | Python project config, dependencies, tool settings |

### Generated Outputs (DO NOT EDIT DIRECTLY)

- `README.md` — generated from CSV; controlled by `root_style` in `acc-config.yaml`
- `README_ALTERNATIVES/` — all style variants (Extra, Classic, Awesome, Flat permutations)
- `assets/badge-*.svg` — generated badge/header SVGs

### README Styles

| Style | Description |
|-------|-------------|
| **Awesome** | Clean awesome-list compliant style |
| **Extra** | Full visual theme with SVG badges and CRT-style TOC |
| **Classic** | Plain markdown with collapsible sections |
| **Flat** | 44 table views (category × sort permutations) for pseudo-dynamic filtering |

## Project Structure

```
scripts/
  readme/           — README generation pipeline
    generators/     — Style-specific generator classes (awesome, flat, minimal, visual)
    helpers/        — Config loading, path resolution, utilities
    markup/         — Markdown/HTML renderers per style
    svg_templates/  — SVG template renderers (badges, dividers, headers, toc)
    generate_readme.py  — Main entry point
  badges/           — Badge notification system
  categories/       — Category management (add_category.py)
  graphics/         — Logo SVG generation
  ids/              — Resource ID generation
  maintenance/      — Repo health checks, GitHub release data updates
  resources/        — Resource download and sorting
  testing/          — TOC validation, regeneration cycle tests
  ticker/           — Repo ticker SVG generation
  utils/            — Shared utilities (git, GitHub, resource parsing)
  validation/       — Link validation (single + batch)
templates/          — Category definitions, templates, overrides
tests/              — pytest test suite
tools/              — Utility tools (readme_tree updater)
data/               — Repo ticker CSV data
docs/               — Project documentation
resources/          — Downloaded resource files, example CLAUDE.md files
```

## Development Setup

**Python version:** 3.11 (see `.python-version`)

```bash
# Create and activate virtual environment
python3.11 -m venv venv
source venv/bin/activate

# Install with dev dependencies
make install
# or: pip install -e ".[dev]"
```

## Common Commands

```bash
make ci               # Run full CI locally: format-check + mypy + test + docs-tree-check
make test             # Run pytest test suite
make format           # Auto-fix formatting with ruff
make format-check     # Check formatting without fixing
make mypy             # Run mypy type checks
make generate         # Regenerate all READMEs from CSV (runs sort first)
make sort             # Sort resources in CSV by category/subcategory/name
make validate         # Validate all links in the CSV
make coverage         # Run tests with coverage reports
make docs-tree        # Update README-GENERATION file tree
make docs-tree-check  # Verify file tree is up to date
make test-regenerate  # Delete and regenerate READMEs, fail if diff (requires clean tree)
make clean            # Remove caches and test artifacts
```

## CI Pipeline

CI runs on every push and PR via `.github/workflows/ci.yml`:
- `make ci` = `format-check` + `mypy` + `test` + `docs-tree-check`
- Python 3.11 on ubuntu-latest
- Uses system Python (not venv) in CI (`CI=true` env var)

## Code Quality

- **Formatter/Linter:** ruff (line length 100, target Python 3.11)
- **Type checking:** mypy (excludes `resources/`)
- **Pre-commit hooks:** ruff lint+format, large file checks, yaml/json validation, make test, README generation check
- **Test framework:** pytest with coverage via pytest-cov

### Ruff Rules

Enabled: `E, W, F, I, N, UP, B, C4, SIM`. Ignored: `E203, E402`. Line-too-long (`E501`) ignored in `scripts/`. `scripts/archive` is excluded entirely.

## Key Conventions

### Adding Resources

Resources are **never** added by editing README.md. The workflow is:
1. Add/modify entries in `THE_RESOURCES_TABLE.csv`
2. Run `make generate` to regenerate all READMEs
3. The pre-commit hook verifies README.md matches CSV data

### Adding Categories

Use `make add-category` (interactive) or update `templates/categories.yaml` directly. This file is the single source of truth for category definitions.

### Modifying README Output

1. Edit the template in `templates/` (for structure changes)
2. Edit markup renderers in `scripts/readme/markup/` (for content formatting)
3. Edit generators in `scripts/readme/generators/` (for generation logic)
4. Run `make generate` to see results
5. Run `make test-regenerate` to verify reproducibility

### SVG Assets

Badge and visual assets are generated programmatically by `scripts/readme/svg_templates/` and `scripts/readme/helpers/readme_assets.py`. Edit those scripts, not the SVG files directly.

## Important Warnings

- **README.md is generated** — never edit it directly; changes will be overwritten
- **README_ALTERNATIVES/ is generated** — same as above
- **Pre-commit hooks run tests** — `make test` runs on every commit for Python changes
- **CLAUDE.md is gitignored** at root level (but not under `resources/`)
- The `.claude/` directory is mostly gitignored; only `.claude/commands/evaluate-repository.md` is tracked
- `scripts/archive/` is excluded from linting, tests, and coverage

## Dependencies

**Runtime:** PyGithub, PyYAML
**Dev:** pytest, requests, python-dotenv, types-requests, types-PyYAML, ruff, pre-commit, pytest-cov, mypy

## Environment Variables

- `GITHUB_TOKEN` — avoids GitHub API rate limiting for validation and resource downloads
- `CI=true` — switches from venv Python to system Python in Makefile
