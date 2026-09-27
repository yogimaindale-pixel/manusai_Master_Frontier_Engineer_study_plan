# Enterprise Scenario Case Studies and Mock Whiteboards

These are practice cases, not predictions of exact panel questions. For each, use: **outcome → requirements → data flow → intelligence → knowledge → tools → state → security → human control → observability → scale → ROI**.

## 1. Global agentic operations platform

**Context:** Multiple countries use different monitoring and ticket systems. **Requirements:** 24x7 triage, data residency, local escalation, read-only investigation, approval-gated remediation. **Design:** event bus → regional agent gateway → ACL-aware retrieval → specialist tools → durable state → approval service → ticket/audit. **Risks:** cross-region data, duplicate actions, model outage. **Metrics:** MTTR, triage time, unsafe-action blocks, cost/task, SLA. **Ask:** Why centralize orchestration? How do you fail regional isolation? **Whiteboard:** separate global policy/control plane from regional data/action plane.

## 2. Payments incident RCA and approved remediation

**Context:** Payment failures spike. **Requirements:** correlate traces, logs, deployment changes, runbooks, and alerts; propose rollback only after approval. **Design:** event intake → evidence collector → graph/vector retrieval → hypothesis agent → critic → human approval → deployment tool. **Controls:** no direct production credentials to model, idempotent rollback, two-person approval for high risk. **Cross-question:** What if evidence conflicts? **KPI:** MTTR, false escalation, change failure rate.

## 3. Multi-country customer-service agent

**Context:** Customers ask about orders and refunds in multiple jurisdictions. **Requirements:** language, residency, policy versions, identity, human handoff. **Design:** regional routing → CRM ACL → policy RAG → deterministic refund eligibility → approval for exceptions. **Risks:** privacy, wrong policy, unauthorized refund. **Metrics:** resolution, CSAT, escalation, refund error. **Cross-question:** Can the model decide eligibility? Answer: it can propose; deterministic policy service decides.

## 4. Enterprise policy assistant with SharePoint and ACL-aware RAG

**Context:** Employees ask HR/security questions. **Requirements:** document permissions, citations, effective dates, abstention. **Design:** SharePoint ingestion → parsing/version/ACL metadata → hybrid search → permission filter → rerank → grounded answer. **Risks:** stale policy, existence leakage, malicious document. **Cross-question:** Where is ACL applied? Before context and again at citation/response.

## 5. AI coding-agent platform integrated with Git and CI/CD

**Context:** Teams want AI-generated code. **Requirements:** repository context, tests, review, secrets/license controls, reproducible artifacts. **Design:** spec prompt → isolated branch → generated diff → tests/scans → PR → protected main → progressive deploy. **Metrics:** cycle time, escaped defects, review rework, test pass, security findings. **Cross-question:** Who owns generated code? Human engineering owner.

## 6. Agent governance control plane

**Context:** Ten business units build agents independently. **Requirements:** inventory, policy, model/tool catalog, approvals, evaluation, cost. **Design:** registry → risk classification → policy gateway → approved model/tool catalog → telemetry/eval platform → incident response. **Risks:** shadow agents, inconsistent controls, data leakage. **Cross-question:** What must remain decentralized? Domain ownership and data stewardship.

## 7. Knowledge graph plus RAG for dependency analysis

**Context:** A service dependency question spans CMDB, deployments, alerts, owners, and tickets. **Design:** entity resolution → graph with provenance/time → local traversal + vector evidence → answer with cited path. **Risks:** wrong merges, stale edges, ACL leakage. **Metrics:** entity precision, path correctness, citation support, query latency. **Cross-question:** Why not SQL? Graph answers multi-hop relationship queries more naturally, but baseline SQL/RAG remains valid.

## 8. Model/platform standardization across ten delivery teams

**Context:** Teams use inconsistent models and tools. **Requirements:** quality, cost, portability, security, support. **Design:** weighted decision matrix → shared gateway → approved patterns → exception process → benchmark suite. **Risks:** lock-in, central bottleneck, local needs ignored. **Cross-question:** What is a hard gate? Data residency/security requirement.

## 9. HR case-assist agent

**Context:** HR analysts need draft answers from policy and case records. **Requirements:** sensitive data, jurisdiction, human review, audit. **Design:** identity → ACL-trimmed RAG → draft only → citations → analyst approval. **No autonomous decisions.** **Metrics:** handling time, citation correctness, review edits, privacy incidents. **Cross-question:** How do you prevent employment decisions by the model? Keep decision authority with qualified human/policy service.

## 10. Regulatory evidence assistant

**Context:** Auditors need evidence across documents, controls, tickets, and logs. **Requirements:** immutable provenance, effective dates, access, exportable evidence. **Design:** governed ingestion → claim graph → source spans → retrieval → evidence packet. **Risks:** altered evidence, unsupported claim, retention. **Metrics:** evidence completeness, citation accuracy, audit rework. **Cross-question:** Can generated text be evidence? No; source artifacts and traceable claims are evidence.

## 11. Data-engineering quality agent

**Context:** Pipelines fail or data quality drifts. **Requirements:** inspect metadata and logs, propose fixes, never alter production schema without approval. **Design:** event → rule engine → evidence retrieval → bounded diagnosis → ticket/draft change. **Metrics:** detection time, diagnosis precision, false remediation, MTTR. **Cross-question:** How do you test synthetic failures? Use labeled fixtures and fault injection.

## 12. Knowledge-management migration

**Context:** A company consolidates fragmented runbooks. **Requirements:** deduplicate, classify, owner review, retention, access. **Design:** parsing → entity/topic extraction → similarity/graph clustering → human validation → versioned knowledge base. **Risks:** deleting authoritative nuance, wrong merge, permissions. **Metrics:** duplicate rate, freshness, retrieval recall, owner approval.

## 13. Software-release readiness agent

**Context:** Release managers need a risk summary. **Requirements:** pull request, CI, incidents, vulnerabilities, change calendar, approval. **Design:** deterministic checks first → evidence RAG → summary agent → policy gate. **Metrics:** escaped failures, review time, false blocks. **Cross-question:** Why agent? Only for synthesis; release gate remains deterministic.

## 14. Contact-center escalation router

**Context:** Route complex cases to specialists. **Requirements:** sentiment, language, skill, SLA, privacy. **Design:** classifier → policy/routing engine → human queue; LLM drafts context, not final route for high-risk categories. **Metrics:** routing accuracy, transfer rate, SLA, satisfaction. **Cross-question:** How do you monitor bias? Segment outcomes and review errors.

## 15. Cloud-cost optimization advisor

**Context:** Engineers need recommendations from usage, tags, commitments, and incidents. **Requirements:** no destructive change, explain evidence, owner approval. **Design:** metrics/FinOps data → rules + RAG → recommendation → approval → controlled change. **Metrics:** realized savings, avoided risk, false recommendations. **Cross-question:** Why not autonomous shutdown? Irreversibility and business impact.

## 16. Agent-to-agent supply-chain workflow

**Context:** Procurement, compliance, and engineering agents collaborate on a supplier. **Requirements:** identity, task contracts, artifacts, audit, human decisions. **Design:** A2A coordinator → specialist agents → MCP tools → approval. **Risks:** untrusted artifacts, task replay, data leakage. **Metrics:** task success, artifact quality, approval latency, unsafe action.

## 17. Banking fraud investigation assistant

**Context:** Analysts correlate alerts, accounts, devices, and transactions. **Requirements:** strict access, explainable evidence, no autonomous account action. **Design:** graph/vector retrieval → evidence map → analyst-facing hypotheses → human decision. **Risks:** bias, privacy, false positives. **Metrics:** investigation time, precision, override, fairness review. **Cross-question:** Who makes the decision? Authorized human/process.

## 18. Kubernetes reliability copilot

**Context:** Engineers investigate alerts and deployment health. **Requirements:** cluster read access, namespace isolation, safe recommendations, approval for changes. **Design:** metrics/logs/events tools → runbook RAG → proposed command plan → policy/approval → executor. **Metrics:** time to diagnose, unsafe command blocks, success, cost. **Cross-question:** How do you prevent arbitrary shell? Do not expose arbitrary shell; narrow typed operations.

## 19. Global data-sovereignty assistant

**Context:** Data cannot leave country boundaries. **Requirements:** regional inference/retrieval, central policy, local deletion. **Design:** global control plane → regional data plane → region-local indexes/models → aggregate metrics only. **Risks:** metadata leakage, fallback routing. **Cross-question:** What if region fails? Local degraded mode; fail closed rather than cross border without approval.

## 20. AI adoption and benefits-realization program

**Context:** A client wants an AI pilot but cannot explain value. **Requirements:** baseline, user readiness, controls, finance-approved benefit. **Design:** discovery → options → narrow pilot → phased rollout → control group/segmentation → benefits report. **Metrics:** adoption, task success, MTTR/time, realized capacity/cash, safety, satisfaction. **Cross-question:** What if ROI is not demonstrated? Stop or redesign; do not scale on enthusiasm.

## Whiteboard checklist

For any case, ask:

- Who is the user and what outcome matters?
- What data is sensitive, authoritative, stale, or cross-border?
- Which steps are deterministic and which need model flexibility?
- What tools exist and what is the narrowest safe contract?
- Where do identity, ACLs, approvals, and policy enforcement happen?
- How is state persisted, deduplicated, resumed, and deleted?
- What happens when retrieval, model, tool, queue, or region fails?
- Which retrieval, generation, agent, operational, safety, adoption, and ROI metrics prove success?
- What is the rollout, owner, rollback, and decision gate?
