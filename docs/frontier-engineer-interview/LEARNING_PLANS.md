# Learning Plans

## 1-day emergency revision

### Block 1 — 60 minutes: architecture memory

- Review the master mental loop: goal → context → model → retrieval/tools → validation → observe → improve.
- Draw RAG offline/online paths.
- Draw a bounded agent with tools, state, approval, timeout, and rollback.
- Say the difference between prompting, RAG, fine-tuning, workflow, agent, MCP, and A2A.

**Resource:** [Microsoft RAG solution design guide](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/rag/rag-solution-design-and-evaluation-guide).

**Hands-on:** sketch the payments RCA or policy assistant case.

**Questions:** 1–10 from the Q&A bank.

### Block 2 — 60 minutes: security and evaluation

- Review injection, ACL trimming, least privilege, tool validation, approval, trace, and failure handling.
- Memorize retrieval, groundedness, agent, operational, safety, adoption, and ROI metrics.

**Resource:** [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) and [OWASP LLM Top 10](https://genai.owasp.org/llm-top-10/).

**Hands-on:** write a threat/control/evidence table for an agentic RAG system.

**Questions:** 76–95.

### Block 3 — 60 minutes: stories and spoken practice

- Prepare one truthful platform, automation, incident, DevOps, AI, stakeholder, and failure story.
- Deliver 60-second and 2-minute introductions.
- Run the mock panel opening and architecture case.

**Checklist:** no invented metrics; state unknowns; include a trade-off and a metric in every answer.

## 3-day focused preparation

### Day 1 — Foundations and retrieval

**Objective:** Explain LLM, embeddings, prompt/context, RAG, and Graph RAG.

**Read:** Google LLM module; Microsoft RAG guide; W3C RDF/OWL.

**Hands-on:** Lab 1 RAG assistant or a paper design if time is limited.

**Whiteboard:** enterprise ACL-aware RAG.

**Questions:** 1–50.

**Rehearse:** “Why not fine-tune?” and “How do you debug retrieval?”

### Day 2 — Agents, MCP/A2A, security, evaluation

**Objective:** Design bounded agent orchestration and secure tool interoperability.

**Read:** Anthropic effective agents; MCP specification/security; A2A project; OWASP/NIST.

**Hands-on:** Lab 2 or Lab 3 with mock tools.

**Whiteboard:** production-support agent with approval.

**Questions:** 51–95.

**Rehearse:** “Why not use an agent?” and “What happens when the agent is wrong?”

### Day 3 — Build, operate, advise

**Objective:** Connect Python/CI/CD/observability to outcomes and ROI.

**Read:** FastAPI, OpenTelemetry, Kubernetes probes/HPA, FinOps unit economics.

**Hands-on:** Lab 6 model matrix or Lab 8 executive memo.

**Whiteboard:** platform selection and operating model.

**Questions:** 96–100 plus mock panel.

**Rehearse:** 60-second CIO explanation and one truthful failure story.

## 7-day interview plan

| Day | Objectives | Resource | Hands-on | Whiteboard | Verbal rehearsal |
|---|---|---|---|---|---|
| 1 | LLM, Transformer, tokens, sampling, model choice | Google LLM module | Token/decoding demo | Model request path | Explain hallucination |
| 2 | Prompt/context and structured output | OpenAI/Anthropic prompt docs | Schema validator | Trust-class context map | Explain injection |
| 3 | Enterprise RAG | Microsoft RAG guide | RAG lab | Ingestion/query flows | Retrieval debugging |
| 4 | Agent orchestration and state | Anthropic/Microsoft patterns | Mock incident workflow | Safe agent loop | Workflow vs agent |
| 5 | MCP/A2A and knowledge graphs | MCP/A2A/W3C docs | Governed tool or graph lab | Interoperability boundaries | MCP vs A2A |
| 6 | Security, evaluation, observability | NIST/OWASP/OpenTelemetry | Red-team/eval harness | Threat/control/evidence | Release gate |
| 7 | Client advisory, ROI, coding, mock panel | FinOps/Cloud Adoption/ FastAPI | Executive memo + coding drill | Platform decision | 60/120/300-second answers |

**Daily completion:** explain one concept; implement or inspect one proof; answer ten questions; draw one architecture; rehearse one story; record one uncertainty.

## 14-day mastery plan

- **Day 1:** LLM foundations and Transformer.
- **Day 2:** embeddings, vector search, and chunking.
- **Day 3:** prompt contracts and structured output.
- **Day 4:** RAG ingestion, ACLs, freshness, evaluation.
- **Day 5:** GraphRAG, ontology, and entity resolution.
- **Day 6:** workflows versus agents and orchestration patterns.
- **Day 7:** durable state, retries, idempotency, and human control.
- **Day 8:** MCP, A2A, tool governance, and interoperability.
- **Day 9:** security, threat modeling, privacy, and supply chain.
- **Day 10:** QA, synthetic data, red teaming, and LLM judges.
- **Day 11:** OpenTelemetry, SLOs, Kubernetes scale, failure drills.
- **Day 12:** Python APIs, Docker, Git, CI/CD, AI-assisted development.
- **Day 13:** client advisory, adoption, ROI, and executive memo.
- **Day 14:** full mock panel, story bank, gaps, and final recall.

**Each day:** one official resource, one hands-on task, ten Q&A questions, one whiteboard, one spoken rehearsal, and a completion note. Spend more time on a weak competency than rereading mastered definitions.

## Priority rule

If a day is missed, do not restart. Keep the order: **architecture/security first, then retrieval/agent evaluation, then code/operations/advisory depth**. The interview rewards defensible engineering judgment more than the number of products memorized.
