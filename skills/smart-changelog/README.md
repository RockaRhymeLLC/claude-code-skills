# smart-changelog

Generate professional changelogs and release notes from your git history — automatically.

## What it does

`smart-changelog` analyzes your git commits, categorizes them, suggests a semantic version, and produces clean changelogs in [Keep a Changelog](https://keepachangelog.com/) format.

Works with **conventional commits** (`feat:`, `fix:`, etc.) and **regular commits** — it reads your diffs to categorize changes intelligently.

## Usage

```
/smart-changelog              # changelog since last tag
/smart-changelog v1.2.0       # changelog since specific version
"generate release notes"       # natural language works too
"what changed since v2.0?"     # also works
```

## Example output

```markdown
## [1.4.0] - 2026-02-14

### Added
- Rate limiting for API endpoints (#123)
- Export to CSV from dashboard
- Dark mode support

### Fixed
- Login redirect loop on Safari (#456)
- Memory leak in WebSocket handler

### Performance
- 40% faster search with new index strategy
```

## Features

- **Auto-categorization** — Groups commits into Added, Fixed, Changed, Removed, Performance, etc.
- **Semantic versioning** — Suggests MAJOR/MINOR/PATCH based on change types
- **Human-readable entries** — Transforms "fix null check in processOrder" into "Fixed crash when processing orders with empty shipping address"
- **Issue/PR linking** — Automatically detects and includes `#123` references
- **GitHub Releases** — Creates releases via `gh` CLI with changelog as notes
- **CHANGELOG.md management** — Prepends new versions to existing changelog file
- **Monorepo support** — Scope changelog to specific packages/directories
- **Format matching** — Adapts to your project's existing changelog format

## Requirements

- git
- gh CLI (optional, for GitHub Releases)

## Install

```bash
cp -r smart-changelog/ ~/.claude/skills/smart-changelog/
```

## Part of the Release Workflow

Pairs perfectly with the **PR Workflow Pack**:

```
smart-review → smart-commit → smart-pr → merge → smart-changelog → ship 🚀
```

## License

MIT
