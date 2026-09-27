# Interview Q&A Bank — 100 Original Practice Questions

These are **practice questions**, not leaked or claimed exact panel questions. Use the answer frames in spoken language. For every answer, state assumptions, name a trade-off, and connect the design to testing and operations.

## A. LLM and GenAI foundations

1. **What does an LLM actually predict?** — It predicts token probabilities conditioned on context; it generates plausible language, not guaranteed truth. Probe: how do grounding and abstention help?
2. **What is a token and why does it matter?** — Tokenization affects context, cost, latency, and multilingual behavior. Probe: what happens when context is too long?
3. **Explain self-attention at interview depth.** — Tokens compute relationships with other tokens so relevant context can influence representation; it is not memory or truth verification. Probe: why are Transformers useful?
4. **What is a context window?** — A bounded input/output working set, not durable memory. Probe: why can more context reduce quality?
5. **Temperature versus top-p?** — Both shape sampling; lower variation can improve repeatability but not factuality. Probe: which settings fit extraction?
6. **What is an embedding?** — A vector representation useful for similarity, search, clustering, and classification. Probe: does similarity prove authorization?
7. **Prompting, RAG, or fine-tuning?** — Prompting changes instructions, RAG supplies changing/private evidence, fine-tuning changes behavior. Probe: can they combine?
8. **Why do LLMs hallucinate?** — They optimize likely continuation and may lack evidence or misread it. Probe: what controls reduce risk?
9. **How do you choose a model?** — Evaluate task quality, tool/schema reliability, context, safety, latency, cost, privacy, lifecycle, and fallback. Probe: why not always use largest?
10. **What does structured output solve?** — It gives downstream code a contract; it does not replace validation or authorization. Probe: what if JSON is malformed?
11. **How do you explain decoder-only versus encoder-only models?** — Decoder-only models generate autoregressively; encoder-only models create representations for understanding/search. Probe: which fits semantic retrieval?
12. **What is inference?** — Running trained weights to produce output. Probe: where do prompts and decoding fit?
13. **How can a smaller model win?** — Lower cost/latency and sufficient quality on bounded tasks; prove it with evaluation. Probe: how do you route?
14. **What is model drift?** — Behavior changes from model, prompt, data, index, or environment changes. Probe: how do you detect and roll back?
15. **What does “grounded” mean?** — Claims are supported by supplied evidence. Probe: how do you test citation correctness?
16. **What is a model gateway?** — A shared policy/routing/telemetry layer; it should not hide application ownership. Probe: what belongs outside it?
17. **When is direct generation enough?** — Stable, low-risk, well-specified tasks with no external facts. Probe: what failure changes that decision?
18. **What is a foundation model?** — A broadly pretrained model adapted to many tasks; the application supplies task-specific context and controls. Probe: what does pretraining not guarantee?
19. **How do you reduce token cost?** — Routing, caching, context selection, batching, concise schemas, and smaller models, while measuring quality. Probe: what caching risk exists?
20. **How do you explain AI limitations to an executive?** — State what the system knows, where it can fail, controls, human responsibility, and the measure of success. Probe: give a 30-second version.

## B. Prompt and context engineering

21. **What belongs in a prompt contract?** — Role, goal, authoritative context, constraints, examples, output schema, uncertainty, and validation. Probe: which part is security-critical?
22. **How do you separate system, user, retrieved, and tool context?** — Label trust classes and treat external content as data. Probe: what is indirect injection?
23. **How do you handle conflicting sources?** — Rank authority/freshness, surface conflict, cite both, and abstain or escalate if material. Probe: who owns source authority?
24. **Why use few-shot examples?** — To demonstrate format or edge labels; avoid bias and context waste. Probe: how do you test example sensitivity?
25. **What is prompt injection?** — An attempt to override intended instructions. Probe: what boundary is stronger than a prompt instruction?
26. **How do you validate model output?** — Parse schema, allowed values, business rules, evidence, and side-effect permissions. Probe: should invalid output retry?
27. **When should an application abstain?** — Missing, stale, contradictory, unauthorized, or low-confidence evidence. Probe: how do you avoid over-refusal?
28. **What is context compression?** — Reduce context while preserving decision-relevant information and provenance. Probe: how do you verify lost facts?
29. **How do you version prompts?** — Store IDs, text, schema, model reference, tests, approval, and rollback metadata. Probe: is a prompt a code change?
30. **How do you defend a prompt to a panel?** — Explain objective, evidence, constraints, output, failure behavior, and test cases. Probe: what does the prompt not control?
31. **Why are delimiters useful?** — They clarify data boundaries and reduce instruction confusion; they are not authorization. Probe: what else is required?
32. **How do you handle long conversations?** — Summarize with provenance, retain critical state separately, and enforce retention/privacy. Probe: what can summary lose?
33. **What is context freshness?** — Whether supplied facts reflect the valid source version and effective date. Probe: how do you invalidate stale context?
34. **How do you prevent secret leakage?** — Keep secrets out of context/logs, use scoped credentials, redact, and authorize tools server-side. Probe: what if the model asks for a secret?
35. **How should a prompt address uncertainty?** — Specify unknown/conflict behavior and require evidence IDs or human review. Probe: does “be accurate” suffice?

## C. RAG and enterprise search

36. **Draw an enterprise RAG pipeline.** — Ingest, parse, chunk, enrich metadata/ACL, embed/index, retrieve, filter/rerank, pack context, generate, validate, cite, observe. Probe: where is authorization?
37. **Why chunk documents?** — To retrieve focused passages within budget; trade precision against lost context. Probe: how does overlap help?
38. **Vector versus keyword search?** — Semantic similarity versus exact lexical matching; hybrid often helps. Probe: when is keyword essential?
39. **What is reranking?** — Stronger scoring over a smaller candidate set. Probe: what does it cost?
40. **What does top-k control?** — Candidate coverage versus noise, latency, and tokens. Probe: how do you tune it?
41. **Why store metadata?** — Filters for tenant, permission, date, source, owner, and version. Probe: can metadata itself be wrong?
42. **How do you debug “no answer”?** — Trace parsing, chunking, query, retrieval, filters, reranking, prompt, and generation separately. Probe: which metric first?
43. **How do you evaluate retrieval?** — Recall/Precision@K, hit rate, MRR, nDCG, relevance, authority, freshness. Probe: what does Recall@K not prove?
44. **How do you evaluate grounded answers?** — Groundedness, citation correctness, relevance, completeness, abstention, human review. Probe: can an LLM judge be enough?
45. **How do you handle document updates?** — Version, re-embed affected content, publish atomic index, monitor lag, and roll back. Probe: how do you prevent mixed versions?
46. **What is ACL-aware RAG?** — Filter evidence according to user/tenant permission before model context. Probe: what about document existence leakage?
47. **When use agentic RAG?** — Multi-step source choice or question decomposition when benefit outweighs cost/risk. Probe: when is standard RAG safer?
48. **What is Graph RAG?** — Combine graph entities/relationships and retrieval for multi-hop or global questions. Probe: when is it overkill?
49. **How do you handle tables and PDFs?** — Layout-aware extraction, OCR where needed, table validation, and source provenance. Probe: how do you test parsing?
50. **What causes stale answers?** — Stale source/index, cache, memory, or model knowledge. Probe: which freshness signals do you expose?

## D. Agent architecture and orchestration

51. **Agent versus workflow?** — Workflow follows code; agent dynamically selects steps/tools. Probe: what is your default?
52. **When not to use an agent?** — Stable deterministic path, high-risk action, low ambiguity, or no benefit from autonomy. Probe: what would justify one?
53. **What is an agent loop?** — Goal, plan, validate, execute, observe, verify, continue/approve/stop. Probe: how do you prevent loops?
54. **What belongs in agent state?** — Task ID, identity, plan/version, evidence, tool results, status, deadlines, approvals, retries. Probe: what should not persist?
55. **Single versus multi-agent?** — Start single; add agents for specialization, isolation, or parallel work with measured benefit. Probe: coordination costs?
56. **Explain orchestrator-workers.** — Supervisor decomposes and assigns bounded tasks, then synthesizes verified outputs. Probe: how do you handle conflicting workers?
57. **How do you make tool calls safe?** — Narrow typed tools, authorization, validation, idempotency, timeouts, quotas, approval, audit. Probe: what if tool output is malicious?
58. **How do you recover after a crash?** — Durable checkpoint/state machine, leases, idempotency, retry classification, dead letter, compensation. Probe: how do you avoid duplicate action?
59. **What is human-in-the-loop?** — Human approves or reviews a defined step; human-on-loop monitors; human-in-command retains decision authority. Probe: where would you use each?
60. **How do you choose sequential or parallel?** — Dependencies require sequential; independent work can parallelize, but reconcile conflicts and budgets. Probe: failure propagation?
61. **How do you measure an agent?** — Task/tool success, step efficiency, escalation, unsafe-action rate, latency, cost, user outcome. Probe: which is release-blocking?
62. **How do you stop runaway cost?** — Step/token/time budgets, quotas, cancellation, caching, routing, rate limits, circuit breakers. Probe: what is user-visible?
63. **What is durable execution?** — Long-running work resumes from persisted state after failure. Probe: what does idempotency mean here?
64. **What is a compensation action?** — A safe corrective action for an already-completed side effect. Probe: is compensation always possible?
65. **What is tool poisoning?** — Malicious or misleading tool metadata/response influencing behavior. Probe: how do you review tools?

## E. MCP, A2A, knowledge graphs, and platform

66. **What is MCP?** — Protocol for an AI host to connect to server-provided tools, resources, and prompts. Probe: what does it not guarantee?
67. **What is A2A?** — Protocol for independent agents to collaborate through tasks/messages/artifacts and discovery. Probe: how is it different from MCP?
68. **Why use a conventional API instead?** — Stable deterministic contract may be simpler, faster, and easier to authorize. Probe: when is MCP worth it?
69. **What is a confused deputy?** — A privileged intermediary misuses authority on behalf of an insufficiently authorized caller. Probe: how do you stop token passthrough?
70. **What is a knowledge graph?** — Connected entities/relationships with provenance and semantics. Probe: when does it outperform vector search?
71. **Taxonomy versus ontology?** — Hierarchy versus formal semantic model and constraints. Probe: what does RDF provide?
72. **How do you govern entity resolution?** — Canonical IDs, evidence, confidence, conservative matching, review, lineage. Probe: what is over-merging risk?
73. **Compare managed and pro-code agent platforms.** — Speed/operations versus control/portability; compare on requirements, not branding. Probe: what is hybrid?
74. **How do you build a weighted model matrix?** — Hard gates then weighted criteria and measured proof of value. Probe: what if the winner fails privacy?
75. **How do you reduce lock-in?** — Portable contracts, data/provenance, traces, adapters, versioned tests, fallback. Probe: what portability costs?

## F. Security and governance

76. **Why is a prompt not an authorization boundary?** — Model obedience is probabilistic; use IAM, policy services, tool gateways, and database permissions. Probe: where is policy enforced?
77. **Threat-model an agentic RAG system.** — Identify assets, trust boundaries, injection path, tool blast radius, controls, detection, and response. Probe: residual risk?
78. **What is least privilege for agents?** — Minimum identity, data, tool, operation, time, and tenant scope. Probe: how do you implement progressive elevation?
79. **How do you secure logs?** — Redaction, minimization, encryption, RBAC, retention, audit, and safe correlation. Probe: what is useful to retain?
80. **What does NIST AI RMF add?** — Govern, Map, Measure, Manage lifecycle risk and accountability. Probe: how do you turn it into gates?
81. **Name major LLM risks.** — Injection, disclosure, poisoning, supply chain, unsafe output, excessive agency, vector weaknesses, misinformation, unbounded use. Probe: pick one and test it.
82. **How do you handle human approval?** — Define risk threshold, preview action/evidence, authorized approver, expiry, audit, and cancellation. Probe: what if approval service is down?
83. **How do you secure a multi-tenant vector store?** — Tenant-aware ACL metadata, enforcement before retrieval, isolation tests, no existence leakage. Probe: what about cached results?
84. **How do you manage model/data lifecycle?** — Version, evaluate, approve, monitor, rollback, archive, decommission. Probe: who owns it?
85. **How do you red-team safely?** — Isolated synthetic environment, mock tools, authorized test data, stop conditions, evidence, remediation. Probe: how do you measure attack success?

## G. Evaluation and operations

86. **What is the evaluation pyramid?** — Unit, component, contract, integration, end-to-end, regression, load/chaos, security, UAT. Probe: which belongs in CI?
87. **Synthetic data benefits and risks?** — Coverage and privacy benefits, but realism, bias, contamination, and validation risks. Probe: what is your holdout?
88. **Why not trust an LLM judge?** — Bias, shared blind spots, rubric ambiguity; calibrate against human labels. Probe: what deterministic checks remain?
89. **What should an agent trace contain?** — IDs, versions, retrieval IDs, tool args/results, state, policy, approvals, latency, tokens, outcome, safely redacted. Probe: what should never be logged?
90. **What is an SLO?** — Target for user-visible reliability over a window; error budget guides priorities. Probe: why not target 100%?
91. **How do you troubleshoot production failure?** — Establish impact, trace, isolate, mitigate, verify, root-cause, prevent, communicate. Probe: rollback or fix forward?
92. **What is backpressure?** — Limit intake/processing to protect system; use queues, admission, load shedding, and user-visible status. Probe: what happens to correctness?
93. **How do retries become dangerous?** — They amplify load or duplicate writes; use deadlines, classification, idempotency, budgets, jitter. Probe: what is a dead letter?
94. **Kubernetes readiness versus liveness?** — Readiness controls traffic; liveness restarts stuck containers; startup protects slow initialization. Probe: what makes a bad liveness probe?
95. **What does cost per successful task mean?** — Total operational cost divided by successfully completed tasks, not token price alone. Probe: what costs are included?

## H. Coding, advisory, ROI, and leadership

96. **How do you review AI-generated code?** — Compare to spec, test edge cases, inspect security/provenance, run checks, review diff, and commit incrementally. Probe: what would make you reject it?
97. **What belongs in CI for AI systems?** — Code tests, schema/tool contracts, security scans, eval regressions, artifact provenance, deploy gates, smoke tests, rollback. Probe: what is slow and scheduled?
98. **How do you frame a vague client AI request?** — Outcome, baseline, users, risk, alternatives, proof of value, decision owner. Probe: when is the answer “no AI”?
99. **How do you prove ROI?** — Baseline/counterfactual, realized benefit, total cost, adoption, sensitivity, phased comparison, and attribution caveats. Probe: capacity versus cash savings?
100. **What makes a Master Frontier Engineer different?** — Ownership from problem framing through architecture, implementation, security, quality, operations, advisory, and outcomes. Probe: give a truthful story that shows learning and accountability.

## Challenge questions to rehearse

- Why not use an agent?
- Why not send all retrieved chunks?
- Why not use the largest model?
- Why not fine-tune?
- Why not use MCP for every integration?
- What happens when the agent is wrong?
- How do you stop repeated actions?
- How do you prove ROI is attributable to AI?
- How do you explain the architecture to a CIO?

## Spoken answer rule

For each question, produce a **30-second version** first. If probed, expand to **2 minutes** with one example, one trade-off, one control, and one metric. Never hide uncertainty behind jargon.
