
## Authentication

All authentication must use the `progression-labs/auth` package for web applications.

### Requirements

- Use `progression-labs/auth` for all web app authentication — never integrate Clerk directly
- Enable only the auth methods your project needs (Google, Microsoft, Email/Password)
- MFA is optional and configured per-project
- Refer to the [progression-labs/auth](https://github.com/progression-labs/auth) repository for implementation details

### Integration

Auth is integrated into the base packages — you don't install it directly:

- **TypeScript APIs:** `progression-labs/fastify-base` includes auth middleware
- **Python LLM services:** `progression-labs/llm` includes auth validation
- **Frontend BFF:** Auth token exchange in Next.js API routes is allowed for the BFF layer

### Supported Methods

| Method | Status |
|--------|--------|
| Google OAuth | Supported |
| Microsoft OAuth | Supported |
| Email/Password | Supported |
| Magic Links | Available |
| MFA | Optional |

### Deviations

If your project needs auth features not in `progression-labs/auth`, extend the package rather than bypassing it. Document any project-specific deviations in an ADR.