# 07 — Mock Intermediate Assessment

## Instructions

Timebox this to 35 minutes. Write concise, production-minded answers. For architecture questions, draw a flow and include failure handling.

## Section A — Short answer

1. When is RAG preferable to fine-tuning?
2. Why can a RAG system hallucinate even when retrieval is enabled?
3. What is the difference between direct and indirect prompt injection?
4. Name five controls that do not rely on model obedience.
5. How do you evaluate an LLM application beyond exact-match accuracy?
6. What is the difference between retrieval recall and answer groundedness?
7. Why should an agent have a maximum step budget?
8. How would you reduce latency without blindly reducing quality?

## Section B — Architecture

Design an internal policy assistant that answers employee questions from versioned company policies. It must respect department permissions, cite sources, abstain when evidence is missing, and support a human escalation path.

Your answer should include:

- Ingestion and freshness handling.
- Chunking, metadata, embeddings, retrieval, and reranking.
- Permission enforcement and tenant/department isolation.
- Grounded prompting, citations, abstention, and output validation.
- Evaluation metrics and production monitoring.
- Incident response and rollback.

## Section C — Agent design

Design an agent that reads a support ticket, searches approved documentation, drafts a response, and creates a ticket update only after human approval.

Include tools, schemas, state, approval gates, timeouts, retries, auditability, and prompt-injection defenses.

## Section D — Coding challenge

Implement a function that receives ranked retrieval results and returns at most `k` authorized passages for a given tenant. It must skip empty text, reject scores below a configurable threshold, preserve ranking order, and avoid mutating the input.

State complexity and list at least five tests.

## Answer frames

### 1. RAG versus fine-tuning
RAG is best for private, current, changing, or citeable knowledge. Fine-tuning is best for stable behavior, style, or task patterns. RAG avoids retraining for every content update but adds ingestion, retrieval, access-control, and evaluation complexity.

### 2. RAG hallucination
Retrieval may be irrelevant, incomplete, stale, unauthorized, or badly packed into context. The prompt may not require evidence, the model may misinterpret it, or output validation may be absent. Debug retrieval and generation separately.

### 3. Injection
Direct injection is in the user’s request. Indirect injection is embedded in external content such as a webpage, document, or tool output. Both require untrusted-content handling, but indirect injection is particularly important in RAG and agent systems.

### 4. Boundary controls
IAM, network policy, tool allow-lists, schema validation, database permissions, secret isolation, quotas, timeouts, sandboxing, approvals, and output validators.

### 5. Evaluation
Measure task success, correctness, groundedness, citation correctness, refusal/abstention, safety, robustness, latency, cost, and user outcomes using automated tests plus human review where risk warrants it.

## Scoring rubric

| Area | Strong response |
|---|---|
| Concepts | Accurate definitions and correct trade-offs |
| Architecture | Complete happy path plus failure path |
| Security | Boundary controls, least privilege, isolation, and injection defense |
| Evaluation | Separate retrieval, generation, application, and operational metrics |
| Coding | Correct, testable, complexity-aware, and secure |
| Engineering judgment | Clear assumptions and justified choices |
