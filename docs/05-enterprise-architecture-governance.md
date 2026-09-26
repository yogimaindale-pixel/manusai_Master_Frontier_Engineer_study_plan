# 05 — Enterprise AI Architecture and Governance

## Reference architecture

```text
Experience: chat, API, copilot, workflow, batch
    ↓
AI gateway: routing, rate limits, model policy, secrets, quotas
    ↓
Orchestration: prompts, RAG, tools, agents, workflows
    ↓
Model layer: hosted models, local models, fine-tunes
    ↓
Knowledge layer: object store, SQL, search, vector index
    ↓
Platform: IAM, network, queues, compute, CI/CD, secrets

Cross-cutting: security, privacy, evaluation, tracing, cost, governance,
model/data lifecycle, incident response, and change control
```

## Governance questions

1. What business purpose and risk tier does the system have?
2. What data is used, and is it personal, confidential, regulated, or tenant-specific?
3. Who can invoke models, retrieve data, call tools, and approve actions?
4. Which model, prompt, data version, and tool versions are in production?
5. Where is human review mandatory?
6. Which quality, safety, privacy, bias, robustness, and security tests are required?
7. What is logged, who owns alerts, and how is drift detected?
8. How are changes approved, versioned, tested, rolled back, and communicated?
9. What are the provider’s retention, residency, SLA, and exit terms?
10. How are incidents contained, investigated, and remediated?

## Security and privacy checklist

- Least-privilege IAM for every service and tool.
- Tenant isolation before retrieval and before response.
- Secret redaction and controlled handling of personal data.
- No credentials in prompts, model context, client code, or unprotected logs.
- Untrusted treatment of retrieved content and tool output.
- Output validation before code, SQL, or business actions execute.
- Network restrictions, allow-lists, timeouts, quotas, and circuit breakers.
- Secure logging with minimization and retention policies.
- Dependency, model-artifact, prompt, and data-pipeline supply-chain review.
- Tests for prompt injection, sensitive-data disclosure, poisoning, denial of service, insecure output handling, and excessive agency.

## Operations

### Cost
Token budgets, caching, batching, smaller-model routing, retrieval limits, quotas, and usage attribution.

### Latency
Streaming, parallel retrieval, precomputation, bounded reranking, timeouts, and graceful degradation.

### Reliability
Retries with backoff, fallbacks, circuit breakers, idempotency, provider health checks, and rollback.

### Observability
Record request ID, model/version, prompt version, retrieved source IDs, tool calls, tokens, latency, errors, policy decisions, and outcome while protecting sensitive data.

## Governance answer frame

A strong assessment answer connects **purpose → risk → controls → evidence → owner → monitoring → response**. Mentioning “responsible AI” without naming mechanisms, owners, and tests is incomplete.
