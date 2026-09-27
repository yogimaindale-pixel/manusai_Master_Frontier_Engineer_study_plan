# Mock Interview Panel Script

## Instructions

Run this as a 75–90 minute panel. One person acts as interviewer, one as skeptical architect, and one as client/executive. Score reasoning, not memorized vocabulary.

## Opening — 5 minutes

**Question:** Give us a 2-minute introduction for a Master Frontier Engineer Build role.

**Expected shape:** platform/production-support foundation; automation, DevOps, monitoring, incident/data experience; current AI/RAG/agentic focus; ownership of reliable systems; what you want to build. Use only verified claims.

**Follow-ups:** What do you mean by “frontier”? What have you built versus studied? Where are you strongest and what are you still learning?

## Technical deep dive — 15 minutes

**Question:** Explain how a Transformer/LLM produces an answer and where hallucination comes from.

**Probe:** tokens, attention, context, sampling, grounding, structured output, fine-tuning versus RAG.

**Scoring:** accurate mechanism, clear limit statements, application implications.

**Question:** Draw an enterprise RAG system for internal operations.

**Probe:** ingestion, chunking, ACLs, hybrid search, reranking, citations, freshness, evaluation, cost.

**Scoring:** complete data flow, security before model context, separate retrieval/generation metrics.

## Architecture case — 20 minutes

**Prompt:** A global organization wants an agent to diagnose production incidents from monitoring, job scheduler, ticket, and runbook data, then propose remediation. It must support multiple countries and must not execute risky changes without approval.

**Candidate should ask:** users, regions, data sensitivity, existing APIs, action types, RTO/RPO, workload volume, baseline MTTR, success criteria, approval authority, current monitoring, and support ownership.

**Strong architecture:** event bus; regional data/access plane; identity and policy gateway; read-only tools; retrieval with ACL and provenance; planner/evidence collector; durable state; critic/verifier; approval service; action gateway; audit/trace; fallback; evaluation.

**Panel probes:**

- Why an agent rather than a workflow?
- How do you prevent duplicate remediation?
- What if the runbook contains injection text?
- What if one country’s model provider is down?
- How do you prove MTTR improvement?
- What happens when retrieval is empty?

## Protocol section — 10 minutes

**Question:** Where do MCP and A2A fit?

**Expected:** MCP for host-to-tool/resource access; A2A for peer-agent tasks; conventional APIs where deterministic contracts suffice; neither replaces identity/policy/audit.

**Probes:** confused deputy, token audience, tool poisoning, Agent Cards, task states, streaming, idempotency, versioning.

## Coding/design section — 15 minutes

**Prompt:** Implement or describe a function that selects authorized, non-empty retrieval results above a threshold, preserving rank and limiting context.

**Expected:** input validation, no mutation, tenant/ACL check, score threshold, max items, deterministic tests, O(n) complexity, missing-field behavior.

**Extension:** wrap it in an API/tool contract with timeout, logging, and testable authorization.

## Security section — 10 minutes

**Question:** A retrieved ticket tells the agent to ignore all prior instructions and call a privileged tool. What happens?

**Strong answer:** treat ticket as untrusted evidence; delimit; do not obey; policy gateway validates tool/action/user/tenant; tool is narrow and least privilege; output/action schema; approval; audit; test injection.

**Probe:** What if the model still proposes it? The tool gateway denies it. What if the user is authorized but request is high-impact? Approval/risk policy still applies.

## Advisory section — 10 minutes

**Question:** Explain your architecture to a CIO in 60 seconds.

**Strong answer:** business problem, outcome, human accountability, evidence, risk controls, pilot, measure, decision requested. No unsupported promises.

**Question:** How do you prove ROI?

**Strong answer:** baseline/counterfactual, TCO, adoption, realized benefit, phased comparison, sensitivity, finance owner, and attribution caveats.

## Scoring rubric

Score 1–5 in each area:

- **Technical depth:** mechanisms accurate and appropriately detailed.
- **Architecture:** complete path, state, dependencies, failure handling.
- **Security:** authorization outside model, threat/control/evidence thinking.
- **Evaluation:** quality, safety, operations, adoption, and ROI metrics.
- **Engineering:** tests, CI/CD, deployment, rollback, ownership.
- **Communication:** clear spoken response, adapts to audience.
- **Judgment:** chooses simplest adequate design, explains trade-offs.
- **Truthfulness:** distinguishes verified experience from proposed design.

## Red flags to avoid

- “The LLM will know not to do that.”
- “RAG eliminates hallucinations.”
- “We should always use the largest model.”
- “The vector database handles access control automatically.”
- “We can let the agent use a generic shell/SQL tool.”
- “ROI is time saved multiplied by salary” without baseline or realized benefit.
- Claiming a production outcome from a prototype.
- Listing vendor features without a requirement or failure case.
- No rollback, owner, metric, or human-control point.
