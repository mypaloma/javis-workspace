# Coding Standards

Standards applied to all code in this repository.

## General

- Prefer clarity over cleverness
- Every file should be readable by someone unfamiliar with the project
- Keep functions small and single-purpose
- Avoid hardcoded secrets — use environment variables

## Languages

### Python
- Follow [PEP 8](https://pep8.org/)
- Use type hints where practical
- Docstrings on all public functions/classes
- Use `argparse` for CLI scripts

### JavaScript / TypeScript
- Use ES modules (`import/export`)
- Prefer `async/await` over callbacks
- TypeScript preferred for anything non-trivial

### Bash
- `set -euo pipefail` at the top of every script
- Quote all variables: `"$VAR"` not `$VAR`
- Include a usage function

## Documentation

Every project must have:
- `README.md` — what it is, how to install, how to use, examples
- `CHANGELOG.md` — version history (for versioned projects)
- `.env.example` — template for required environment variables (if any)
- Inline comments for non-obvious logic

## Git

- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/)
- One logical change per commit
- Never commit secrets, credentials, or `.env` files
- Branches: `javis/<topic>` or `agent/<agent>-<topic>`
