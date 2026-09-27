# Hands-On Lab Portfolio

Use Python by default. Every lab should have a README, `pyproject.toml` or `requirements.txt`, environment-variable configuration, type hints, structured logging, exception/timeout handling, JSON/schema validation, unit tests, mock external services, a Dockerfile, and a CI workflow. Never use real credentials or production data.

## Lab 1 — Enterprise RAG assistant with citations and abstention

**Objective:** Build a small assistant over public runbooks or synthetic operational documents.

**Architecture:** ingestion/parser → chunker → metadata/ACL → embedding and keyword indexes → hybrid retrieval → reranker → grounded prompt → citation validator → answer/abstain.

**Steps:** create a source registry; preserve title, owner, version, effective date, sensitivity, and source span; compare chunking strategies; add identity-aware filters; test answerable and unanswerable questions; expose a FastAPI endpoint; add traces and cost proxies.

**Expected files:** `src/ingest.py`, `src/retrieve.py`, `src/answer.py`, `src/policy.py`, `tests/`, `eval/golden.jsonl`, `Dockerfile`, `README.md`, CI workflow.

**Tests:** relevant passage in top-k, stale document rejected, unauthorized chunk excluded, citation span supports claim, malformed model output rejected, missing evidence produces `INSUFFICIENT_EVIDENCE`.

**Failure/security:** poisoned document, prompt injection, cross-tenant leakage, OCR/table parsing error, vector index unavailable, provider timeout. Fail closed for unauthorized data and unsafe actions.

**Interview talking points:** RAG versus fine-tuning; retrieval versus generation metrics; chunking trade-offs; freshness and index rollback; evidence authority; latency/cost.

## Lab 2 — Agentic production-support workflow with mock tools

**Objective:** Build a bounded incident workflow using mock Control-M, Splunk, ServiceNow, and Teams APIs.

**Architecture:** alert → classifier/router → evidence collector → runbook RAG → proposal → approval → ticket update/remediation simulator → audit.

**Steps:** implement read-only tools first; add typed arguments and user/tenant policy; create proposal and approval token; add idempotency key, deadline, retry classification, circuit breaker, and dead letter; simulate tool failure.

**Expected files:** `src/agent.py`, `src/tools/`, `src/policy.py`, `src/state.py`, `tests/`, `fixtures/incidents/`, `Dockerfile`, CI.

**Tests:** duplicate event does not duplicate action; expired approval denied; non-authorized tool denied; timeout returns safe status; ticket update is auditable; injection in ticket text is not obeyed.

**Failure/security:** model suggests destructive action, API returns malformed data, worker crashes, duplicate delivery, mock provider 429/500, approval service unavailable.

**Interview talking points:** why deterministic workflow first; safe autonomy levels; human-in-the-loop; compensation; MTTR and unsafe-action metrics.

## Lab 3 — Governed MCP server with two narrow tools

**Objective:** Expose `read_incident_summary` and `get_approved_runbook` through an MCP server; do not expose arbitrary shell or SQL.

**Architecture:** host/client → MCP gateway → server → synthetic store; policy service validates identity, tenant, role, tool, arguments, and rate.

**Steps:** follow a pinned MCP specification/SDK; implement initialization and capability discovery; define schemas; add auth mock; write audit events; add malicious tool-description and resource tests; optionally add a separate approval-gated proposed action.

**Expected files:** `mcp_server/`, `schemas/`, `tests/security/`, `policy.md`, `Dockerfile`, CI.

**Tests:** invalid schema rejected; unknown ID denied; over-broad query rejected; unauthorized tenant denied; prompt injection in resource text treated as data; trace ID preserved.

**Failure/security:** token passthrough, confused deputy, data exfiltration, tool poisoning, rate exhaustion, server timeout.

**Interview talking points:** MCP versus API; tool boundaries; server responsibility; consent; least privilege; audit and versioning.

## Lab 4 — Multi-agent RCA with durable state

**Objective:** Build an orchestrator, evidence collector, hypothesis analyst, critic, and report writer with a durable state machine.

**Architecture:** coordinator → parallel evidence workers → critic → report; state store tracks queued, investigating, awaiting review, completed, failed, compensated.

**Steps:** define task and artifact schemas; use correlation IDs; checkpoint after each stage; enforce max steps/deadlines; add worker lease and resume; require evidence IDs in reports; approval before action.

**Tests:** worker killed mid-task resumes; duplicate task is deduplicated; conflicting evidence triggers review; critic rejects unsupported hypothesis; state corruption moves to dead letter.

**Failure/security:** stale state, worker disagreement, untrusted artifact, prompt injection, cross-agent data scope, runaway loop.

**Interview talking points:** orchestrator-workers versus single agent; parallelism trade-off; state durability; compensation and failure semantics.

## Lab 5 — Agent evaluation harness and synthetic incidents

**Objective:** Measure agent quality, safety, operations, and business proxies.

**Dataset:** 30–50 synthetic incidents covering normal, ambiguous, stale, permission, tool-failure, duplicate, and injection cases; manually validate a holdout.

**Metrics:** task success, tool selection/argument validity, Recall@K, groundedness, citation correctness, abstention, escalation, unsafe-action rate, step count, p95 latency, token/cost proxy, and cost per successful task.

**Tests:** baseline versus candidate prompt/model/retriever; judge calibration against human labels; regression gate on critical safety failure; replay from stored trace.

**Security:** synthetic identities, mock tools, canary secret, isolated environment, no real credentials, redacted traces.

**Interview talking points:** evaluation pyramid; synthetic-data caveats; LLM-as-judge limitations; release gates and confidence.

## Lab 6 — Weighted model/platform selection script

**Objective:** Build a decision matrix and proof-of-value report.

**Criteria:** task quality 25%, tool/schema reliability 15%, privacy/residency 15%, latency 10%, cost 10%, availability/fallback 10%, integration effort 5%, observability 5%, portability 5%. Adjust and defend weights.

**Steps:** define hard gates first; collect measured results on a golden set; normalize scores; calculate weighted result; run sensitivity analysis; document recommendation, risks, fallback, and reassessment date.

**Tests:** invalid weights rejected; hard-gate failure eliminates option; score changes with evidence; no claim based only on public benchmark.

**Interview talking points:** model versus platform; build/buy/reuse; lock-in; total cost; proof-of-value design.

## Lab 7 — Prompt-injection and tool-abuse red team

**Objective:** Attack a disposable RAG/agent application safely and document control effectiveness.

**Attack set:** direct injection, indirect injection in document/ticket/log, secret extraction, tool argument manipulation, unauthorized tenant query, replay, oversized input, poisoned metadata, and malformed output.

**Steps:** define assets and stop rules; use synthetic canary secret; run attacks against mock tools; capture traces; map each attack to preventive/detective control; remediate and rerun.

**Tests:** no secret returned; unauthorized tool denied; external instructions not obeyed; destructive action requires approval; audit record retained; rate limit works.

**Interview talking points:** threat model; control outside LLM; residual risk; red-team ethics and isolation.

## Lab 8 — Executive one-page memo and change plan

**Objective:** Turn a technical pilot into an outcome-led recommendation.

**Deliverables:** one-page memo with problem, baseline assumptions, options, recommendation, TCO, risks/controls, milestones, decision required; RACI; stakeholder map; training/communications; rollout and rollback; benefits dashboard.

**Metrics:** adoption, task completion, override/escalation, user satisfaction, MTTR/triage time, cost per successful task, safety incidents, and realized cash/capacity value.

**Tests:** every metric has definition, source, owner, cadence, and baseline; theoretical savings separated from realized value; no invented candidate metric.

**Interview talking points:** CIO explanation, why now, why not AI, attribution, adoption, accountability, and change readiness.

## Shared repository template

```text
project/
├── README.md
├── pyproject.toml
├── src/
├── tests/
├── fixtures/
├── eval/
├── Dockerfile
├── .env.example
├── .github/workflows/ci.yml
└── docs/
```

## Definition of done

A lab is ready to discuss when it runs from a clean checkout, has tests for normal and failure cases, contains no secrets, records assumptions, shows one trace, explains one trade-off, reports at least one quality and one operational metric, and states what remains unverified.
