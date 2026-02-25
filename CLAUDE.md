# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the central registry of coding standards, guidelines, and rulesets for Progression Labs. It produces two outputs:
1. A MkDocs Material documentation site deployed to GitHub Pages
2. Generated markdown consumed by the `@standards-kit/conform` MCP server

## Commands

| Task | Command |
|------|---------|
| Install | `pnpm install` |
| Generate all output | `pnpm generate` |
| Build TypeScript | `pnpm build` |
| Dev (watch mode) | `pnpm dev` |

After modifying any guideline (`guidelines/*.md`) or ruleset (`rulesets/*.toml`), run `pnpm generate` to regenerate the `generated/` directory.

## Architecture

The generator (`src/index.ts`) is a single-file TypeScript script that:
1. Reads TOML rulesets from `rulesets/` and converts them to markdown
2. Reads markdown guidelines from `guidelines/`, validates their YAML frontmatter against schema constraints
3. Outputs everything into `generated/` — both raw ruleset markdown and a full MkDocs site structure (`generated/site/docs/` + `mkdocs.yml`)

### Guidelines

Markdown files in `guidelines/` with YAML frontmatter. Required frontmatter fields:
- `id` — kebab-case identifier (must match `^[a-z][a-z0-9-]*$`)
- `title` — display name
- `category` — one of: `security`, `architecture`, `infrastructure`, `operations`, `data`, `reliability`
- `priority` — integer >= 1 (lower = higher priority, controls sort order)
- `tags` — array of kebab-case strings used by the MCP server for context matching

### Rulesets

TOML files in `rulesets/` defining tool configurations at different strictness tiers (production/internal/prototype) for TypeScript and Python. The TOML structure uses nested sections (e.g., `[code.linting.eslint.rules]`) that get rendered into hierarchical markdown.

### Generated Output (do not edit)

`generated/rulesets/` — standalone ruleset markdown files
`generated/site/` — complete MkDocs site (docs + mkdocs.yml)

## CI/CD

- `deploy-pages.yml` — on push to `main`: installs deps, runs `pnpm generate`, builds MkDocs site, deploys to GitHub Pages
- Site URL: https://chrismlittle123.github.io/standards/

## Workflow

- Create feature branches from `main`
- Use conventional commits (`feat:`, `fix:`, `chore:`, etc.)

## MCP Tools

Use these to query the standards registry at any time:

| Tool | Purpose |
|------|---------|
| `get_standards` | Get guidelines matching a context string (e.g., `typescript fastapi`) |
| `list_guidelines` | List all available guidelines with metadata |
| `get_guideline` | Get a specific guideline by ID |
| `get_ruleset` | Get a ruleset by ID (e.g., `typescript-production`) |
