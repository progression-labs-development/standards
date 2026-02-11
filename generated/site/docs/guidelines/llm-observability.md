
## LLM Observability

All LLM observability must use the `progression-labs/llm` Python package (Langfuse).

### Requirements

- Use `progression-labs/llm` for all LLM tracing — never integrate Langfuse directly
- Include metadata on all LLM calls: project, feature, userId
- Use the same `requestId` as app logs for correlation

### Installation

```bash
pip install progression-labs-llm
```

### Required Metadata

| Field | Description |
|-------|-------------|
| `project` | Project identifier |
| `feature` | Feature or use case name |
| `userId` | User who triggered the call |
| `requestId` | Correlation ID (links to app logs) |

### What Gets Tracked

- Model used
- Token count (input/output)
- Latency
- Cost
- Success/failure
- Prompt and response content

Refer to [progression-labs/llm](https://github.com/progression-labs/llm) for implementation details.
