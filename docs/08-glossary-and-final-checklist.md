# 08 — Frontier AI Engineering Glossary and Final Checklist

## 📖 Frontier AI Engineering Glossary (50+ Core Terms)

### Category 1: LLM Foundations & Transformer Mechanics
1. **Self-Attention:** Mechanism computing $\text{Softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ to weigh relationships between all tokens in a sequence.
2. **Query ($Q$), Key ($K$), Value ($V$):** Learned projection matrices where Query represents search intent, Key represents token content, and Value represents output representations.
3. **Grouped-Query Attention (GQA):** Memory-efficient attention where multiple query heads share a single Key-Value head, dramatically reducing KV cache footprint.
4. **Byte Pair Encoding (BPE):** Subword tokenization algorithm iteratively merging frequent byte pairs into single vocabulary tokens.
5. **Context Window:** The maximum combined length of input prompt tokens and generated output completion tokens supported by a model.
6. **KV Cache:** GPU VRAM memory buffer storing previously computed Key and Value vectors for historical tokens to avoid redundant re-computation during decode phase.
7. **Temperature ($T$):** Logit scaling parameter controlling entropy in token probability distributions ($z_i / T$).
8. **Top-$p$ (Nucleus) Sampling:** Dynamic sampling technique selecting tokens from the smallest candidate subset whose aggregate probability exceeds $p$.
9. **Mixture of Experts (MoE):** Sparse architecture replacing dense feed-forward networks with multiple specialized expert sub-networks routed by a gating network.
10. **Quantization:** Compression technique converting 32-bit/16-bit floating-point weights to lower precision (INT8, INT4) to reduce VRAM requirements.
11. **GGUF / AWQ / GPTQ:** Popular INT4/INT8 model quantization file formats and post-training quantization algorithms optimized for CPU/GPU inference.
12. **LoRA (Low-Rank Adaptation):** Parameter-efficient fine-tuning technique freezing base model weights and inserting low-rank decomposition matrices $B \times A$.
13. **Direct Preference Optimization (DPO):** Implicit reward optimization algorithm aligning model behavior directly on preferred vs. rejected response pairs without a separate reward model.

---

### Category 2: Prompt Engineering, Schemas & Injection Defenses
14. **Prompt Contract:** Formal prompt specification defining authoritative context boundaries, input/output schemas, and abstention rules.
15. **Chain-of-Thought (CoT):** Prompting technique encouraging the LLM to output intermediate reasoning steps before generating a final answer.
16. **ReAct (Reason + Act):** Interleaved reasoning pattern alternating between **Thought**, **Action (Tool Call)**, and **Observation**.
17. **Tree-of-Thoughts (ToT):** Search-based prompting framework exploring multiple reasoning paths concurrently using tree search (BFS/DFS).
18. **Constrained Decoding:** Logit-level masking technique using a Finite State Machine (FSM) to guarantee $100\%$ adherence to a target JSON Schema or context-free grammar.
19. **Direct Prompt Injection:** Attack vector where malicious user input overrides system prompt instructions (jailbreaking).
20. **Indirect Prompt Injection:** Attack vector where untrusted external data (retrieved RAG documents, web pages) contains hidden commands executed by the LLM.
21. **Dual-LLM Pattern:** Security architecture isolating untrusted data parsing in an unprivileged LLM, passing sanitized JSON to a privileged orchestrator LLM.

---

### Category 3: Embeddings, Vector Search & RAG
22. **Embedding:** Dense vector representation mapping semantic concepts into real-valued vector space $\mathbb{R}^d$.
23. **Cosine Similarity:** Normalized dot product measuring the angular similarity between two unit vectors: $\frac{A \cdot B}{\|A\| \|B\|}$.
24. **BM25:** Classical sparse lexical search algorithm calculating term frequency-inverse document frequency (TF-IDF) keyword matches.
25. **Hybrid Search:** Search architecture combining sparse lexical search (BM25) and dense vector search (Cosine) via Reciprocal Rank Fusion (RRF).
26. **HyDE (Hypothetical Document Embeddings):** Technique generating a hypothetical answer to a query first, then embedding that answer to retrieve matching chunks.
27. **Reciprocal Rank Fusion (RRF):** Algorithm merging multiple ranked retrieval lists using the formula $\sum \frac{1}{k + r(d)}$.
28. **Cross-Encoder Reranker:** Neural model scoring `(Query, Document)` pairs through full cross-attention layers to filter top candidate chunks.
29. **HNSW (Hierarchical Navigable Small World):** Multi-layer graph index achieving $O(\log N)$ approximate nearest neighbor (ANN) vector search.
30. **Product Quantization (PQ):** Compression technique dividing high-dimensional vectors into sub-vectors and quantizing them into compact byte codes.
31. **Parent-Child Chunking:** Strategy embedding small chunks for search precision while returning parent document chunks to the LLM for context.
32. **Semantic Chunking:** Splitting strategy detecting cosine similarity drops between consecutive sentences to form cohesive semantic paragraphs.
33. **RAG Triad:** Evaluation framework measuring Context Relevance, Groundedness (Faithfulness), and Answer Relevance.
34. **Groundedness / Faithfulness:** Proportion of claims in the generated response that are directly supported by retrieved context evidence.

---

### Category 4: Agentic Systems & Tools
35. **Agent:** Autonomous loop using an LLM to evaluate state, formulate plans, invoke tools, and observe environment outputs.
36. **Multi-Agent Topology:** Architectural layout (Router, Supervisor, Sequential, Parallel/Swarm) coordinating multiple specialized agents.
37. **Router Pattern:** Architecture using a classifier LLM to route incoming user queries to specialized downstream domain agents.
38. **Supervisor Pattern:** Hierarchical architecture where a central supervisor agent delegates sub-tasks to worker agents and aggregates results.
39. **Step Budget:** Hard safety limit specifying the maximum allowed tool execution iterations before halting an agent.
40. **Human-In-The-Loop (HITL):** Safety hook intercepting high-risk tool execution (e.g. database deletes, financial transfers) until human approval is granted.
41. **State Compaction:** Process of summarizing or truncating historical chat messages to keep context token count within limits.

---

### Category 5: Enterprise Architecture, Governance & Telemetry
42. **AI Gateway:** Reverse proxy handling semantic caching, dynamic model routing, rate limiting, and token cost quotas.
43. **Semantic Caching:** Caching mechanism returning pre-computed responses when query vector similarity exceeds a high threshold ($>0.96$).
44. **PII Redaction:** Automated data loss prevention (DLP) masking sensitive fields (SSN, credit cards) before sending prompts to external APIs.
45. **NeMo Guardrails:** Policy framework inspecting inputs and outputs to enforce domain boundaries, safety, and alignment rules.
46. **OpenTelemetry (OTel):** Observability standard assigning trace IDs and nested spans to monitor multi-step LLM application latency.
47. **Root Span:** Top-level OpenTelemetry span capturing total user request duration across all child steps.
48. **TTFT (Time to First Token):** Latency metric measuring the time required to process the input prompt and output the first completion token.
49. **TPOT (Time Per Output Token):** Inter-token generation latency during the decode phase.
50. **FinOps:** Financial management discipline optimizing token efficiency, model routing cascades, and quantization to reduce AI operating costs.

---

## ✅ Master Pre-Assessment Readiness Checklist

Review these 10 competency checkpoints before taking the Capstone Assessment:

- [ ] **1. Attention Math:** Can you write the Scaled Dot-Product Attention equation and explain the role of $\sqrt{d_k}$?
- [ ] **2. Decoding Controls:** Do you know how Temperature $T$, Top-$k$, and Top-$p$ alter token selection probability distributions?
- [ ] **3. Quantization:** Can you compare FP16, INT8, and INT4 regarding VRAM requirements and execution latency?
- [ ] **4. Prompt Contracts:** Can you write a production prompt template enforcing JSON schemas and explicit abstention policies?
- [ ] **5. Injection Defenses:** Can you explain Indirect Prompt Injection and design a Dual-LLM privilege separation workflow?
- [ ] **6. Hybrid Retrieval:** Do you know how to combine BM25 and Dense Vectors using Reciprocal Rank Fusion (RRF)?
- [ ] **7. RAG Evaluation:** Can you define all three pillars of the RAG Triad (Context Relevance, Groundedness, Answer Relevance)?
- [ ] **8. Agentic Loops:** Can you design a ReAct agent loop with step budgets, recursion limits, and HITL intercept hooks?
- [ ] **9. Enterprise Gateways:** Do you understand Semantic Caching, Dynamic Failover Routing, and PII DLP Redaction?
- [ ] **10. OpenTelemetry:** Can you trace a request from user input down to vector search and LLM completion spans?

---

## 📐 High-Yield Formula Cheat Sheet

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

$$P(x_i) = \frac{\exp(z_i / T)}{\sum_{j} \exp(z_j / T)}$$

$$W = W_0 + \frac{\alpha}{r} (B A) \quad \text{(LoRA Adaptation)}$$

$$\text{Cosine Similarity}(A, B) = \frac{A \cdot B}{\|A\| \|B\|}$$

$$RRF\_Score(d) = \sum_{m \in M} \frac{1}{k + r_m(d)}$$

$$\text{Groundedness} = \frac{\text{Number of Claims Supported by Context}}{\text{Total Claims Generated in Answer}}$$
