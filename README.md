# 🤖 Javis Workspace

This repository contains all code, scripts, and projects written or delegated by **Javis** — the AI assistant for [PALOMA AI](https://www.mypaloma.ai).

## Structure

```
javis-workspace/
├── scripts/        # Standalone utility scripts (bash, python, etc.)
├── projects/       # Full project folders (apps, services, tools)
├── experiments/    # One-off explorations and proof-of-concepts
├── docs/           # Shared documentation and references
└── .github/        # GitHub workflows and templates
```

## Conventions

### Branches
- `main` — stable, reviewed code only
- `javis/<topic>` — Javis working branches (e.g. `javis/web-scraper`, `javis/auth-fix`)
- `agent/<agent>-<topic>` — Coding agent branches (e.g. `agent/codex-snake-game`)

### Commits
All commits follow [Conventional Commits](https://www.conventionalcommits.org/):
```
feat: add new feature
fix: correct a bug
docs: update documentation
chore: housekeeping tasks
refactor: restructure without behaviour change
```

Commits by Javis are attributed:
```
Co-authored-by: Javis <submissions@mypaloma.ai>
```

### Documentation
Every project/script must include:
- A `README.md` with: purpose, usage, dependencies, examples
- Inline comments for non-obvious logic
- A `CHANGELOG.md` for projects with multiple versions

### Code Quality
- Scripts: include a usage/help block (`--help` flag or header comment)
- Projects: include a `.env.example` if environment variables are used
- Always include a `.gitignore` appropriate to the language/framework

## Coding Agents

Tasks may be delegated to background coding agents:
- **Codex** (`gpt-4o`) — OpenAI-powered, strong at general coding
- **Claude Code** — Anthropic-powered, strong at reasoning and refactoring
- **Pi** — Lightweight, multi-model
- **OpenCode** — Open-source, multi-model

Agent work is done on `agent/*` branches and merged to `main` after review.

## About Javis

Javis is the AI assistant for PALOMA AI, running on [OpenClaw](https://openclaw.ai).
He writes, reviews, and maintains code on behalf of Buddy — keeping things clean, documented, and versioned.
