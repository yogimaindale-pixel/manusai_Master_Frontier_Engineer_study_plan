# Readiness Checklist

## Competency map

| Competency | You can explain | You can design/prove | Your evidence/story | Readiness |
|---|---|---|---|---|
| AI-native architecture | Model is one component; system has context, tools, state, policy, eval, ops | End-to-end whiteboard with failure path |  | Foundation / Working / Strong / Interview Ready |
| LLM foundations | Tokens, context, attention, sampling, embeddings, limitations | Token/decoding demo |  |  |
| Prompt/context | Contract, trust classes, injection, schemas, abstention | Validator and injection tests |  |  |
| Enterprise RAG | Ingest, chunk, metadata, hybrid retrieval, ACL, rerank, citation, freshness | RAG lab and metrics |  |  |
| Agent architecture | Workflow versus agent; patterns, state, tools, termination | Mock incident agent |  |  |
| Orchestration | Sequential/concurrent/router/orchestrator-workers trade-offs | Durable state/failure drill |  |  |
| MCP/A2A | Vertical tool/context versus horizontal agent collaboration | Governed tool/remote task lab |  |  |
| Knowledge graphs | RDF/OWL/property graph, provenance, GraphRAG | Entity resolution/GraphRAG design |  |  |
| Platform/model choice | Weighted matrix and proof of value | Comparison script |  |  |
| Security/governance | Threat→control→test→owner; NIST/OWASP | Red-team evidence bundle |  |  |
| Evaluation | QA pyramid, synthetic data, judge calibration, metrics | Eval harness and regression gate |  |  |
| Operations | OpenTelemetry, SLOs, retries, scale, rollback | Failure injection/runbook |  |  |
| Coding/CI/CD | Python, API, Docker, Git, tests, scans, release | Clean-checkout prototype |  |  |
| Advisory/ROI | Baseline, TCO, adoption, realized benefit | One-page memo and dashboard |  |  |
| Leadership | Coach, communicate, learn from failure | Truthful STAR stories |  |  |

## Green flags

- I can draw the online and offline RAG paths without notes.
- I enforce authorization before retrieval and at tools.
- I choose deterministic workflows before autonomous agents.
- I can name what happens when model, retrieval, tool, queue, or region fails.
- I distinguish quality, safety, operations, adoption, and ROI metrics.
- I can explain one design to an engineer and a CIO.
- I state assumptions and unknowns instead of inventing product facts.
- I have at least three verified stories with evidence.
- I have run or can clearly explain one hands-on lab.

## Red flags to fix

- I describe a model as a knowledge base.
- I say RAG eliminates hallucinations.
- I put authorization in a prompt.
- I expose generic shell/SQL tools to an agent.
- I cannot explain state or duplicate actions.
- I list metrics without definitions, owners, or baselines.
- I use “ROI” for theoretical time savings.
- I claim a prototype as production.
- I cannot describe rollback or incident ownership.
- I use vendor names without requirements or trade-offs.

## Final readiness gate

Mark **Interview Ready** only when you can:

1. Deliver a truthful 60-second introduction.
2. Answer 30 questions aloud in one sitting.
3. Whiteboard the global operations agent in under 8 minutes.
4. Threat-model one indirect injection path.
5. Explain a retrieval failure with metrics.
6. Defend a model/platform choice with a matrix.
7. Describe a test, deployment, rollback, and SLO.
8. Explain ROI with a baseline and attribution caveat.
9. Name one failure and what you learned.
10. Say what you do not know and how you would validate it.
