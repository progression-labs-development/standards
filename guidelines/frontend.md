---
id: frontend
title: Frontend
category: architecture
priority: 5
tags: [typescript, nextjs, react, frontend, vercel]
author: Engineering Team
lastUpdated: "2025-02-26"
summary: "Frontend architecture standards for Next.js and React"
---

## Frontend

All frontends use Next.js with the `progression-labs-development/ui` component library.

### Stack

- Next.js (App Router)
- TypeScript
- Tailwind CSS
- `progression-labs-development/ui` (shared component library)
- Vercel for deployment

### Architecture

Frontend and backend are separate services but colocated in a monorepo (best practice):

- Separate deployments (Vercel for frontend, GCP Cloud Run for backend)
- Same repository when using a monorepo
- Frontend imports types from backend (single source of truth)
- Frontend calls backend APIs — no business logic in Next.js

### API Routes

Only allowed as a thin BFF layer:

- Auth token exchange
- Aggregating backend calls
- Proxying to avoid CORS

**Not allowed:** Business logic, database access, anything requiring unit tests.

### Components

Use `progression-labs-development/ui` for all standard components. This gives all Progression Labs apps a consistent look and feel.

```bash
pnpm add progression-labs-development/ui
```

- Use `progression-labs-development/ui` components by default for simplicity
- Custom components are allowed when needed, but prefer extending the library
- Built with React + Tailwind CSS

### API Client Generation

`progression-labs-development/ui` includes tooling to generate a typed API client from your backend's OpenAPI spec. Works with both TypeScript (Fastify) and Python (FastAPI via `progression-labs-development/llm`) backends.

- Type-safe frontend API calls
- Stays in sync with backend schemas (Zod → OpenAPI → client)
- No manual API client code

### Standards Enforcement

Frontend projects use frontend-specific rulesets that include React and Next.js rules on top of the base TypeScript rules:

```toml
[standards]
ruleset = "typescript-frontend-production"  # or typescript-frontend-internal
```

Available rulesets:
- `typescript-frontend-production` — Customer-facing frontends (strictest)
- `typescript-frontend-internal` — Internal dashboards and tools