---
id: secrets
title: Secrets Management
category: security
priority: 2
tags: [typescript, python, security, infrastructure, gcp, aws, secrets, deployment, pglabs]
author: Engineering Team
lastUpdated: "2026-02-27"
summary: "Standards for creating, storing, and accessing secrets across all environments"
---

## Secrets Management

All secrets must be stored in the cloud platform's secrets manager and registered in the `pglabs` secret registry. No local `.env` files.

### Creating Secrets

Secrets are managed through two layers:

1. **Infrastructure** — `progression-labs-development/infra` creates the secret resource in the cloud platform's secrets manager
2. **Registry** — `pglabs registry secrets` tracks secret metadata (ownership, rotation, expiry)

#### Step 1: Create the Secret Resource

Create the secret resource using the `progression-labs-development/infra` package. Never use `gcloud`, `aws`, or `az` CLI commands to create or delete secret resources.

```typescript
import { Secret } from 'progression-labs-development/infra';

const apiKey = new Secret("api/stripe-api-key");
```

#### Step 2: Set the Secret Value

After Pulumi creates the secret resource, set the sensitive value using the `pglabs` CLI:

```bash
pglabs secrets set api/stripe-api-key
```

The CLI prompts for the value securely and writes it to the appropriate cloud secrets manager.

### Querying Secrets

Use the `pglabs` CLI to discover and inspect secrets:

```bash
# List all secrets
pglabs registry secrets list

# Filter by service, environment, type, or status
pglabs registry secrets list --service stripe
pglabs registry secrets list --environment production
pglabs registry secrets list --type api-key
pglabs registry secrets list --status expiring-soon

# Get metadata for a specific secret
pglabs registry secrets get <id>

# Search by name or service
pglabs registry secrets search "stripe"
```

The registry tracks metadata — not values. It provides a unified view of what secrets exist, who owns them, when they were last rotated, and whether they are expiring.

### Secret Metadata

Every secret in the registry includes:

| Field | Description |
|-------|-------------|
| `id` | Auto-generated with `sec_` prefix (e.g., `sec_abc123xyz`) |
| `name` | Human-readable name |
| `type` | `API_KEY`, `OAUTH_TOKEN`, `CERTIFICATE`, `SSH_KEY`, `PASSWORD` |
| `service` | Associated service (e.g., `stripe`, `github`) |
| `environment` | `production`, `staging`, `development` |
| `owner` | Person slug (e.g., `alice-smith`) |
| `lastRotated` | When the secret was last rotated |
| `expiresAt` | When it expires |
| `status` | `ACTIVE`, `EXPIRING_SOON`, `EXPIRED` |

### Supported Secrets Managers

Use the secrets manager corresponding to your project's cloud platform:

- **AWS**: AWS Secrets Manager
- **GCP**: Google Cloud Secret Manager
- **Azure**: Azure Key Vault

### Requirements

- No `.env` files — never store secrets locally
- All secrets in the platform's secrets manager
- All secret resources created through `progression-labs-development/infra`
- All secret values set through `pglabs` CLI
- All secrets registered in the `pglabs` secret registry
- Local dev authenticates via platform CLI (AWS SSO, `gcloud auth`, `az login`)
- CI/CD authenticates via OIDC (no static keys)

### Local Development

Authenticate using your platform's CLI to **load** secrets at runtime:

```bash
# AWS
aws sso login --profile progression-labs-development

# GCP
gcloud auth application-default login

# Azure
az login
```

Secrets are loaded automatically by the base packages. CLI usage is only for authenticating your local session — never for creating or managing secrets directly.

### GitHub Actions

Prefer using `progression-labs-development/github-actions` reusable workflows which handle OIDC automatically. If writing custom workflows, use the appropriate pattern for your platform:

**AWS:**
```yaml
permissions:
  id-token: write
  contents: read

steps:
  - uses: aws-actions/configure-aws-credentials@v4
    with:
      role-to-assume: arn:aws:iam::ACCOUNT_ID:role/github-actions
      aws-region: eu-west-2
```

**GCP:**
```yaml
permissions:
  id-token: write
  contents: read

steps:
  - uses: google-github-actions/auth@v2
    with:
      workload_identity_provider: projects/PROJECT_ID/locations/global/workloadIdentityPools/POOL/providers/PROVIDER
      service_account: github-actions@PROJECT_ID.iam.gserviceaccount.com
```

**Azure:**
```yaml
permissions:
  id-token: write
  contents: read

steps:
  - uses: azure/login@v2
    with:
      client-id: ${{ secrets.AZURE_CLIENT_ID }}
      tenant-id: ${{ secrets.AZURE_TENANT_ID }}
      subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
```

### Secret Naming

Use kebab-case names scoped to the service:

```
{service}/{secret-name}
```

Examples: `api/stripe-api-key`, `llm-service/openai-key`

The `progression-labs-development/infra` package automatically namespaces secrets by environment (via the Pulumi stack), so do not include the environment in the secret name.

### What NOT to Do

- Set secret values via cloud CLIs directly (`aws secretsmanager put-secret-value`, `gcloud secrets versions add`, etc.) — use `pglabs secrets set` instead
- Create or delete secret resources via CLI (`gcloud secrets create`, `aws secretsmanager create-secret`, `gcloud secrets delete`, etc.)
- Create `.env` files
- Store cloud credentials/access keys in GitHub
- Put sensitive values in Pulumi code, config, or stack outputs
- Commit secrets to git
- Share secrets via Slack
- Write raw Pulumi secret resources instead of using `progression-labs-development/infra`
- Skip registering secrets in the `pglabs` registry
