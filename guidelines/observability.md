---
id: observability
title: Observability
category: operations
priority: 2
tags: [typescript, python, logging, observability, backend]
author: Engineering Team
lastUpdated: "2024-03-15"
summary: "Logging, tracing, and monitoring standards"
---

## Observability

All application observability must use the `chrismlittle123/monitoring` package (SigNoz).

### Requirements

- Use `chrismlittle123/monitoring` for all logging and error tracking — never integrate SigNoz directly
- Use structured JSON logging with consistent fields
- Include `requestId` in all log entries for correlation
- Never log secrets, passwords, or unmasked API keys

### Installation

**TypeScript:**
```bash
pnpm add chrismlittle123/monitoring
```

**Python:**
```bash
uv add chrismlittle123/monitoring
```

### Required Log Fields

| Field | Description |
|-------|-------------|
| `timestamp` | ISO 8601 timestamp |
| `level` | debug, info, warn, error |
| `message` | Human-readable message |
| `requestId` | Correlation ID |
| `service` | Service name |
| `environment` | development, staging, production |

### Log Levels

| Level | Use For |
|-------|---------|
| `debug` | Development only |
| `info` | Normal operations |
| `warn` | Recoverable issues |
| `error` | Failures requiring attention |

Refer to [chrismlittle123/monitoring](https://github.com/chrismlittle123/monitoring) for implementation details.