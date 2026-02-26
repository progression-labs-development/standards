---
id: repository
title: Repository Standards
category: architecture
priority: 1
tags: [repository, metadata, standards]
author: Engineering Team
lastUpdated: "2025-02-26"
summary: "Repository metadata, structure, and documentation standards"
---

## Repository Standards

### Code Quality (Production)

We aim for an **8/10 codebase** at all times in production repositories.

**The definition:** if you asked 100 instances of Claude Code to review the repository and averaged the scores, what would that score be? It should be 8/10. No more, no less.

- **9/10** — We're wasting time on polish that doesn't deliver value
- **Below 8/10** — We're creating tech debt that will slow us down

Prototypes can move faster.

### standards.toml

Every repository must have a `standards.toml` file. See [TypeScript](./typescript.md) and [Python](./python.md) guidelines for language-specific configuration.

### README

Every repository must have a `README.md` that includes:

- What the project does (one sentence)
- How to install and run it locally
- Environment variables required
- How to run tests

### .gitignore

Every repository must have a `.gitignore`. At minimum, ignore:

- `node_modules/`, `.venv/`, `dist/`, `build/`
- `.env` and `.env.*` (except `.env.example`)
- OS files (`.DS_Store`, `Thumbs.db`)
- IDE config (`.idea/`, `.vscode/` — except shared settings)

### Branching

| Repo type | Branches | Default branch |
|-----------|----------|----------------|
| Deployment repos (APIs, frontends, infra) | `dev`, `stag`, `prod` | `dev` |
| Library repos (shared packages) | `main` | `main` |

See [CI/CD guideline](./ci-cd.md) for full branching strategy and deployment flow.

### Branch Protection

Production and internal repositories must enable branch protection on the `prod` branch (or `main` for library repos).

### Git Hooks

Production and internal repositories must use Husky for Git hooks:

- `pre-push` — lint and type check before pushing
- `commit-msg` — validate conventional commit format

Prototype repositories may skip hooks to move faster.