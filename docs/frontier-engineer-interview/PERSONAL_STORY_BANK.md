# Personalized Story Bank

## Evidence rule

The supplied candidate profile confirms areas of experience, not specific projects, metrics, clients, approvals, or outcomes. Before the interview, replace placeholders with verified details from your resume, project notes, tickets, dashboards, change records, or manager-approved examples. Do not claim a production AI deployment, savings figure, client result, or certification unless you can substantiate it.

## Story format

For each story prepare:

- 30-second version: situation, action, result, Frontier relevance.
- 2-minute version: add constraints, trade-offs, collaboration, evidence, and learning.
- Technical deep dive: architecture, failure, testing, operations, security, and metric.
- Claims not to make: anything not supported by your evidence.

## 1. Platform engineering and production support

**Prompt:** Tell me about a platform or production-support problem you owned.

**Fill from evidence:** Situation: `[service/process and impact]`. Task: `[responsibility]`. Action: `[diagnosis, automation, controls, communication]`. Result: `[verified metric or qualitative outcome]`. Learning: `[changed design or operating practice]`.

**Frontier connection:** Explain how the same ownership applies to AI systems: trace the whole path, make failure observable, automate safely, and define an SLO/rollback.

**Follow-ups:** How did you know the root cause? What trade-off did you make? What happened when the dependency failed? What would you change now?

## 2. Automation and operational improvement

**Prompt:** Describe automation that reduced repetitive work or risk.

**Evidence to collect:** baseline volume, manual steps, error modes, approvals, test approach, deployment, monitoring, and realized benefit. If you do not have a finance-approved value, say “capacity returned” rather than “saved money.”

**Frontier connection:** Compare deterministic automation with an agent. Use an agent only for ambiguity or synthesis; keep permission and action execution deterministic.

## 3. Incident and risk handling

**Prompt:** Tell me about a high-severity incident or operational risk.

**Evidence to collect:** detection, impact, timeline, incident role, mitigation, communication, recovery, postmortem, and prevention. Avoid exposing confidential names or sensitive data.

**Frontier connection:** Map the story to AI failure handling: evidence, model/retrieval/tool isolation, human approval, degraded mode, and audit.

## 4. RAG/GenAI experimentation

**Prompt:** What have you tried with RAG, LLMs, or agents?

**Safe structure:** State exactly what you built or studied, data source, prompt/retrieval approach, evaluation, failure, and learning. If it was a proof of concept, call it a proof of concept. Do not imply production scale without evidence.

**Technical deep dive:** chunking, embeddings, retrieval metrics, citations, ACLs, prompt injection, latency, cost, and rollback.

## 5. Azure or cloud implementation

**Prompt:** How would you take an AI prototype into Azure or another cloud?

**Answer using verified experience:** Describe the cloud services you have actually used. For unfamiliar services, say you would validate the current product documentation and run a proof. Cover identity, networking, secrets, observability, region, cost, CI/CD, and incident response.

## 6. DevOps and CI/CD

**Prompt:** How do you release an AI system safely?

**Answer:** protected branch and review; code, schema, tool, prompt, model/config, retrieval, and evaluation tests; scans; build once/promote; canary; monitor; rollback; owner. Link to actual Azure DevOps/Git experience only where verified.

## 7. Innovation or hackathon

**Prompt:** Describe an experiment that did not start with a perfect specification.

**Evidence to collect:** hypothesis, timebox, prototype, feedback, failure, decision. Show curiosity plus discipline: a demo is not production readiness.

## 8. Mentoring engineers

**Prompt:** How do you raise engineering quality across a team?

**Evidence to collect:** review mechanism, standards, coaching, pairing, feedback, measurable change. Frontier connection: teach engineers to review AI output, write evals, preserve security boundaries, and own the outcome.

## 9. Stakeholder and client communication

**Prompt:** Tell me about making a technical issue understandable to a non-technical stakeholder.

**Answer:** business impact, options, risk, decision needed, evidence, next step. Then translate that skill to explaining model limitations, human accountability, and ROI.

## 10. Governance and compliance

**Prompt:** How have you handled control, audit, or compliance requirements?

**Evidence to collect:** policy, access, change approval, evidence, exception, owner, and remediation. Do not claim formal compliance ownership unless documented.

**Frontier connection:** inventory models/tools/data, apply least privilege, version changes, record approvals, test risks, monitor, and respond.

## 11. Failure and changed approach

**Prompt:** Describe a time an approach did not work.

**Answer:** own the result, explain evidence, what you changed, and how you prevented recurrence. Frontier answer should mention rollback, evaluation, and learning instead of blaming the model or user.

## 12. Model/platform/tool selection decision

**Prompt:** Tell me about choosing among tools or platforms.

**Answer:** requirements, hard gates, weighted criteria, prototype, evidence, trade-off, decision, fallback, reassessment. If no specific example is verified, use the proposed model-selection lab and state it is a practice case.

## Story evidence worksheet

| Story | Verified situation | Action evidence | Result evidence | Technical detail | Claims to avoid |
|---|---|---|---|---|---|
| Platform/production |  |  |  |  |  |
| Automation |  |  |  |  |  |
| Incident |  |  |  |  |  |
| AI/RAG |  |  |  |  |  |
| Azure/cloud |  |  |  |  |  |
| DevOps |  |  |  |  |  |
| Mentoring |  |  |  |  |  |
| Advisory |  |  |  |  |  |
| Governance |  |  |  |  |  |
| Failure/learning |  |  |  |  |  |

## Safe language for uncertain evidence

- “My verified experience is…, and for the unfamiliar component I would validate…”
- “In a prototype, I measured…; I would not call that production impact.”
- “I would establish a baseline before claiming ROI.”
- “The design proposal is…, while the past project facts I can substantiate are…”
- “I do not know the exact provider behavior; I would pin the version and test the contract.”
