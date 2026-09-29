# 07 — Mock Intermediate Assessment & Capstone Exam

> **Exam Duration:** 45 Minutes  
> **Total Points:** 100 Points  
> **Instructions:** Answer all questions concisely and professionally. For architecture design scenarios, draw component diagrams and specify failure handling mechanisms.

---

## 📝 Part A: Conceptual & Technical Short-Answer (45 Points)

1. *(3 pts)* **Why is scaling $QK^T$ by $\frac{1}{\sqrt{d_k}}$ required in Transformer Self-Attention?**
2. *(3 pts)* **Contrast Grouped-Query Attention (GQA) with Multi-Head Attention (MHA) regarding KV Cache memory footprint.**
3. *(3 pts)* **Define Time to First Token (TTFT) and Time Per Output Token (TPOT). Which is compute-bound, and which is memory-bandwidth bound?**
4. *(3 pts)* **How does Logit-Level Constrained Decoding guarantee $100\%$ JSON schema compliance?**
5. *(3 pts)* **Explain the difference between Direct Prompt Injection and Indirect Prompt Injection.**
6. *(3 pts)* **Describe the Dual-LLM Privilege Separation Pattern.**
7. *(3 pts)* **Why is Hybrid Search (BM25 + Dense Vectors) superior to Dense Vector Search alone for technical documentation?**
8. *(3 pts)* **Write the mathematical formula for Reciprocal Rank Fusion (RRF) scoring.**
9. *(3 pts)* **How does the HNSW algorithm achieve $O(\log N)$ search latency in vector databases?**
10. *(3 pts)* **Define Groundedness / Faithfulness in the RAG Triad. How is it calculated?**
11. *(3 pts)* **What is the purpose of a Cross-Encoder Reranker in an advanced RAG pipeline?**
12. *(3 pts)* **Why MUST an autonomous ReAct agent loop enforce hard recursion limits?**
13. *(3 pts)* **Explain how Human-In-The-Loop (HITL) Intercept Hooks prevent unauthorized agent actions.**
14. *(3 pts)* **How does Semantic Caching in an AI Gateway achieve $0\text{ms}$ latency and $\$0$ token cost for duplicate queries?**
15. *(3 pts)* **Why is OpenTelemetry (OTel) span tracing essential for debugging production LLM applications?**

---

## 🏗️ Part B: Enterprise Architecture Case Studies (30 Points)

### Scenario 1: Multi-Tenant Enterprise Policy RAG (10 Points)
Design a secure RAG system for a global enterprise with 10,000 employees. It must answer questions from versioned company policy documents, respect department-level Access Control Lists (ACLs), cite source documents, abstain when evidence is missing, and prevent indirect prompt injection from uploaded PDFs.

### Scenario 2: Autonomous Bug Fix Agent with Safety Controls (10 Points)
Design an autonomous software engineering agent that monitors GitHub repository issue queues, runs unit tests, generates bug fixes, and submits pull requests. Specify safety controls, step budgets, sandbox isolation, and human approval gates.

### Scenario 3: Enterprise AI Gateway with Cost & Failover Controls (10 Points)
Design a high-throughput AI Gateway serving 50 internal applications. Requirements include semantic caching, PII redaction, rate-limiting token quotas, and dynamic failover between primary cloud model APIs and a local self-hosted vLLM cluster.

---

## 💻 Part C: Hands-On Coding Challenge (25 Points)

**Task:** Implement a Python class `ReciprocalRankFusion` that takes two sorted retrieval lists—`bm25_results: List[str]` and `vector_results: List[str]`—and computes merged reciprocal rank fusion scores using constant $k=60$.

```python
from typing import List, Tuple, Dict
from collections import defaultdict

class ReciprocalRankFusion:
    def __init__(self, k: int = 60):
        self.k = k

    def fuse_rankings(
        self, 
        bm25_results: List[str], 
        vector_results: List[str], 
        top_n: int = 5
    ) -> List[Tuple[str, float]]:
        """
        Combines two ranked lists of document IDs using Reciprocal Rank Fusion (RRF).
        RRF_Score(d) = Σ 1 / (k + rank(d))
        Returns Top-N (doc_id, score) pairs sorted in descending score order.
        """
        # YOUR CODE HERE
        pass
```

---

# 📖 Exhaustive Answer Key & Solutions

## Part A — Answer Key

1. **Self-Attention Scaling:** Prevents dot products from growing excessively large in high dimensions $d_k$, which would push the Softmax function into saturated regions with near-zero gradients (vanishing gradient problem).
2. **GQA vs. MHA:** GQA groups query heads to share a single KV head (e.g. 8 query heads per 1 KV head), reducing KV cache memory footprint by up to $8\times$ compared to MHA where every query head has a dedicated KV head.
3. **TTFT vs. TPOT:** TTFT (Prefill phase) processes prompt tokens in parallel and is **compute-bound**. TPOT (Decode phase) generates tokens sequentially and is **memory-bandwidth bound**.
4. **Constrained Decoding:** Uses a Finite State Machine (FSM) built from a JSON schema to dynamically mask logits of non-compliant tokens to $-\infty$ during sampling, making syntax errors mathematically impossible.
5. **Direct vs. Indirect Injection:** Direct injection occurs when the user inputs malicious override commands in the prompt. Indirect injection occurs when untrusted data retrieved from external sources (PDFs, web pages) contains hidden malicious instructions.
6. **Dual-LLM Pattern:** Isolates untrusted data processing in an unprivileged LLM (no tools) that outputs clean JSON. A privileged LLM (with tool access) reads only the sanitized JSON, never raw un-sanitized web/RAG text.
7. **Hybrid Search Superiority:** BM25 handles exact keyword/code matches, while Dense Vectors capture abstract semantic meaning. Combining both covers vocabulary mismatch and exact reference lookups.
8. **RRF Formula:** $RRF\_Score(d) = \sum_{m \in M} \frac{1}{k + r_m(d)}$.
9. **HNSW $O(\log N)$ Latency:** Uses a multi-layer graph where upper layers have sparse, long-range skip links and lower layers have dense local links, enabling hierarchical routing to target vectors in logarithmic time.
10. **Groundedness Definition:** The ratio of claims in the generated answer directly supported by facts in the retrieved context chunks. Calculated as $\frac{\text{Supported Claims}}{\text{Total Claims Made}}$.
11. **Cross-Encoder Reranker:** Performs full cross-attention over `(Query, Chunk)` pairs to generate precise relevance scores, filtering top-50 vector matches down to the top-5 most relevant chunks.
12. **Recursion Limits:** Prevents agent loops from running infinitely when tools fail, preventing token context exhaustion and runaway API costs.
13. **HITL Intercept Hooks:** Pause agent execution before high-risk actions (e.g., database deletion or money transfers), requiring a signed human approval token before proceeding.
14. **Semantic Caching:** Converts incoming queries into vector embeddings. If similarity to a prior query exceeds a threshold ($>0.96$), the cached response is returned immediately ($0\text{ms}$ LLM latency, $\$0$ token cost).
15. **OpenTelemetry Spans:** Assigns unique trace IDs and nested span timers to every step in an LLM request (PII filtering, vector search, reranking, generation), isolating exact latency bottlenecks and token cost drivers.

---

## Part B — Architecture Solutions

### Scenario 1: Multi-Tenant Enterprise Policy RAG Architecture

```
User Query ──> [API Gateway + IAM Auth] ──> Metadata Pre-Filter (Tenant ID + Department ACL)
                                                      │
Answer <── [LLM Generator] <── [Cross-Encoder] <── [Hybrid Search (BM25 + Vector Index)]
```

- **Security & ACL Enforcement:** Metadata pre-filtering enforces `tenant_id` and user `security_clearance` at the database index level before running vector distance calculations.
- **Abstention & Injection Defense:** Encloses retrieved text in `<UNTRUSTED_DOCUMENT>` XML tags. Instructs model to return `"INSUFFICIENT_EVIDENCE"` if information is missing.

---

## Part C — Coding Challenge Solution

```python
from typing import List, Tuple, Dict
from collections import defaultdict

class ReciprocalRankFusion:
    def __init__(self, k: int = 60):
        self.k = k

    def fuse_rankings(
        self, 
        bm25_results: List[str], 
        vector_results: List[str], 
        top_n: int = 5
    ) -> List[Tuple[str, float]]:
        """
        Combines two ranked lists of document IDs using Reciprocal Rank Fusion (RRF).
        RRF_Score(d) = Σ 1 / (k + rank(d))
        """
        rrf_scores: Dict[str, float] = defaultdict(float)

        # Process BM25 ranking (1-indexed rank)
        for rank, doc_id in enumerate(bm25_results, start=1):
            rrf_scores[doc_id] += 1.0 / (self.k + rank)

        # Process Vector ranking (1-indexed rank)
        for rank, doc_id in enumerate(vector_results, start=1):
            rrf_scores[doc_id] += 1.0 / (self.k + rank)

        # Sort documents by descending RRF score
        sorted_docs = sorted(rrf_scores.items(), key=lambda item: item[1], reverse=True)

        return sorted_docs[:top_n]

# Unit Test Verification
if __name__ == "__main__":
    bm25 = ["doc_A", "doc_B", "doc_C"]
    vector = ["doc_B", "doc_A", "doc_D"]
    fusion = ReciprocalRankFusion(k=60)
    fused_results = fusion.fuse_rankings(bm25, vector, top_n=3)
    print("Fused Rankings:", fused_results)
    assert fused_results[0][0] == "doc_B" or fused_results[0][0] == "doc_A"
    print("Unit test passed successfully!")
```
