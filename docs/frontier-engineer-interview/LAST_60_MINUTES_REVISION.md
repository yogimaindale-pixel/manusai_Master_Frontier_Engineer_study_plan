# Last 60 Minutes Revision Sheet

## Minute 0–10 — Say the system loop

> **Goal → context → model → retrieval/tools → validate → observe → improve.**

A model is one component. The application owns identity, data, tools, state, evaluation, operations, and accountability.

## Minute 10–20 — RAG drawing

```text
sources → parse/OCR → chunk → metadata/ACL → embed/index
user → auth → query rewrite → hybrid retrieve → ACL filter → rerank
→ context pack → grounded model → citation/schema check → answer/audit
```

Remember:

- Small chunks improve precision; large chunks preserve context.
- Vector similarity is not truth or permission.
- Retrieval quality and generation quality are different.
- Freshness and versioning matter.
- Abstain when evidence is insufficient.

## Minute 20–30 — Safe agent drawing

```text
goal → plan → validate action → narrow tool → observe → verify
→ continue / approval / compensate / stop
```

Name: max steps, deadline, token/cost budget, least privilege, typed schema, idempotency, timeout, retry classification, circuit breaker, audit, human approval, rollback.

## Minute 30–40 — Security flash cards

- Prompt is not authorization.
- External content is untrusted data.
- ACL before model context.
- Tool gateway validates actor, tenant, action, schema, and approval.
- Keep secrets out of prompts/logs.
- Test injection, disclosure, poisoning, unsafe output, excessive agency, and unbounded consumption.
- Trust boundaries are stronger than model obedience.

## Minute 40–50 — Metrics flash cards

**Retrieval:** Recall@K, Precision@K, MRR, nDCG, hit rate, freshness.

**Generation:** groundedness, faithfulness, relevance, completeness, citation accuracy, abstention.

**Agent:** task/tool success, step efficiency, escalation, override, unsafe-action rate.

**Operations:** availability, p95/p99 latency, error rate, cost per successful task, token cost, queue depth, SLO/error budget.

**Business:** MTTR, SLA, triage time, adoption, completion, satisfaction, realized benefit, risk reduction.

## Minute 50–55 — Spoken answers

- **Why not agent?** Stable deterministic path or high-risk action; autonomy must earn its complexity.
- **Why not largest model?** Optimize measured task quality, safety, latency, availability, and cost.
- **Why not fine-tune?** Use it for stable behavior; use RAG for changing/private facts.
- **Why not MCP?** A conventional API may be simpler and safer when the contract is stable.
- **What if wrong?** Detect, abstain/escalate, deny unsafe action, audit, compensate/rollback, learn.

## Minute 55–60 — Personal truth and close

Say your 60-second introduction. Choose three verified stories: production/incident, automation/DevOps, and AI/stakeholder. Replace claims with evidence. If you do not know a product detail, say how you would validate it.

> **Final line:** Build the smallest system that can prove what it did, explain why it did it, and stop safely when it does not know.
