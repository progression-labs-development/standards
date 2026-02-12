
## Authentication

All authentication must use the `chrismlittle123/auth` package for web applications.

### Requirements

- Use `chrismlittle123/auth` for all web app authentication — never integrate Clerk directly
- Enable only the auth methods your project needs (Google, Microsoft, Email/Password)
- MFA is optional and configured per-project
- Refer to the [chrismlittle123/auth](https://github.com/chrismlittle123/auth) repository for implementation details

### Integration

Auth is integrated into the base packages — you don't install it directly:

- **TypeScript APIs:** `chrismlittle123/fastify-base` includes auth middleware
- **Python LLM services:** `chrismlittle123/llm` includes auth validation
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

If your project needs auth features not in `chrismlittle123/auth`, extend the package rather than bypassing it. Document any project-specific deviations in an ADR.