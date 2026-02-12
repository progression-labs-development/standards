
## LLM Observability

All LLM observability must use the `chrismlittle123/llm` Python package (Langfuse).

### Requirements

- Use `chrismlittle123/llm` for all LLM tracing — never integrate Langfuse directly
- Include metadata on all LLM calls: project, feature, userId
- Use the same `requestId` as app logs for correlation

### Installation

```bash
pip install chrismlittle123-llm
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

Refer to [chrismlittle123/llm](https://github.com/chrismlittle123/llm) for implementation details.
