# 05 — Enterprise AI Architecture and Governance

## Enterprise Reference Architecture

Production enterprise AI implementations require a layered, decoupled architecture separating client experience, governance gateways, orchestration logic, model endpoints, knowledge stores, and platform security.

```
+-----------------------------------------------------------------------------------+
| 1. EXPERIENCE LAYER: Chat UI, IDE Copilot, Mobile App, Webhook Workflows          |
+-----------------------------------------------------------------------------------+
                                          │
+-----------------------------------------------------------------------------------+
| 2. ENTERPRISE AI GATEWAY LAYER                                                    |
| • Semantic Cache   • Dynamic Router/Fallback   • Rate Limiter   • PII Masking/DLP |
+-----------------------------------------------------------------------------------+
                                          │
+-----------------------------------------------------------------------------------+
| 3. ORCHESTRATION & AGENTIC LAYER                                                  |
| • Prompt Templates   • RAG Retrieval Engine   • Tool Dispatcher   • State Compactor|
+-----------------------------------------------------------------------------------+
                   │                                       │
+-------------------------------------+   +-----------------------------------------+
| 4. MODEL SERVING LAYER              |   | 5. KNOWLEDGE & VECTOR LAYER             |
| • Frontier APIs (Gemini, Claude)    |   | • Vector Database (HNSW Index)          |
| • Local vLLM / TensorRT Clusters    |   | • Relational SQL (PostgreSQL/SQLite)    |
+-------------------------------------+   +-----------------------------------------+
                                          │
+-----------------------------------------------------------------------------------+
| 6. CROSS-CUTTING TELEMETRY & GOVERNANCE LAYER                                     |
| • OpenTelemetry Spans   • Token Cost Quotas   • NeMo Guardrails   • KMS Secrets   |
+-----------------------------------------------------------------------------------+
```

---

## The Enterprise AI Gateway Pattern

Directly coupling application frontends to model provider APIs creates security vulnerabilities, vendor lock-in, and unpredictable API cost spikes. An **AI Gateway** acts as a reverse proxy managing all model traffic.

```
Client App ──> [ Enterprise AI Gateway ] ──> Semantic Cache Match? ──> YES: Return (0ms, $0)
                      │
                   NO Match
                      │
              PII Redaction / DLP
                      │
              [Dynamic Model Router]
             /                      \
   Primary API (Gemini/Claude)    Fallback API / Local vLLM Cluster
```

### Core AI Gateway Responsibilities

1. **Semantic Caching:** Embeds incoming query string. If vector cosine similarity to a previously answered query exceeds $0.96$, returns the cached response instantly ($0\text{ms}$ LLM latency, $\$0.00$ API cost).
2. **Dynamic Model Routing & Fallback:** Routes low-complexity classification queries to lightweight local models (e.g. Llama 8B) and routes complex multi-step reasoning queries to frontier models (e.g. Gemini 1.5 Pro). Automatically fails over to a backup provider if the primary API returns `503 Service Unavailable` or `429 Rate Limit`.
3. **Token Quota & FinOps Management:** Tracks per-user, per-department dollar spend and token consumption in real-time, enforcing hard monthly budget caps.
4. **Unified Schema Translation:** Translates standard internal JSON payloads into provider-specific API formats (OpenAI, Anthropic, Gemini, Ollama), preventing vendor lock-in.

---

## Security, Privacy & Guardrails Architecture

```
User Input ──> [PII / DLP Filter] ──> [Input Guardrails] ──> LLM ──> [Output Guardrails] ──> Sanitized Response
```

### 1. PII Redaction & Data Loss Prevention (DLP)
Before a prompt leaves the enterprise perimeter, regex classifiers and Named Entity Recognition (NER) models (e.g. Microsoft Presidio) replace sensitive fields:
- `Social Security Number` $\to$ `[REDACTED_SSN]`
- `Credit Card` $\to$ `[REDACTED_CC]`
- `Email Address` $\to$ `[REDACTED_EMAIL]`

### 2. Guardrails Frameworks (NeMo Guardrails / Llama Guard)
Guardrail layers inspect inputs and outputs for compliance:
- **Input Guardrail:** Rejects jailbreak prompts, prompt injections, and off-topic domain queries before passing them to the LLM.
- **Output Guardrail:** Scans model completions for hallucinated PII, profanity, toxic speech, or non-compliant advice (e.g. unauthorized financial recommendations).

---

## Observability & OpenTelemetry Standard

Enterprise AI systems must be fully traceable using the **OpenTelemetry (OTel)** tracing standard. Every user request generates a **Root Trace** composed of nested **Spans**:

```
[Trace ID: 8f3a9b2c] User Query Session (Total: 450ms)
 ├── [Span 1] Input PII Redaction Filter (12ms)
 ├── [Span 2] HyDE Query Transformation (85ms, 120 Tokens)
 ├── [Span 3] Hybrid Vector Search HNSW (28ms, Retrieved 20 Chunks)
 ├── [Span 4] Cohere Cross-Encoder Rerank (42ms, Selected 5 Chunks)
 └── [Span 5] LLM Final Generation (283ms, TTFT: 110ms, 450 Completion Tokens)
```

### Key Production Metrics to Monitor
- **P95 / P99 Latency:** 95th and 99th percentile end-to-end user response times.
- **TTFT (Time to First Token):** Measures responsiveness of streaming outputs.
- **Token Efficiency Ratio:** $\frac{\text{Completion Tokens}}{\text{Prompt Tokens}}$ (Signals prompt verbosity and context efficiency).
- **Cost Per Query (CPQ):** Direct financial cost attributed per API call.

---

## Deployment Targets & FinOps Optimization

### Deployment Strategy Matrix

| Target Platform | Pros | Cons | Best Suited For |
|---|---|---|---|
| **Managed APIs (Vertex AI / Bedrock)** | Zero infra maintenance, instant frontier model access. | High per-token costs, external network egress. | High-complexity reasoning, low-volume enterprise apps. |
| **Serverless Containers (Cloud Run / ECS)** | Auto-scales to zero, easy CI/CD deployment. | Cold-start latency, limited GPU acceleration. | AI Gateways, RAG Orchestrators, Lightweight APIs. |
| **Kubernetes (GKE / EKS) + vLLM** | Lowest cost at high throughput, data stays in VPC. | High operational complexity, GPU node management. | High-volume internal enterprise LLM serving. |

### FinOps Cost-Reduction Playbook
1. **Prompt Compression:** Strip redundant whitespace, system instructions, and stop-words from RAG chunks ($\sim 20-30\%$ token saving).
2. **Model Cascading:** Run an ultra-fast 3B parameter model as a triage classifier. Only cascade to a 70B/Frontier model if confidence score $< 0.85$.
3. **Quantized Self-Hosting:** Deploy INT4/INT8 AWQ quantized models on vLLM to double throughput per GPU.

---

## 🎯 High-Yield Assessment Recall Questions

1. **Q: Explain how Semantic Caching in an AI Gateway reduces latency and cost.**
   - *A:* Semantic caching converts incoming queries into vector embeddings. If the cosine distance to a previously answered query is below a threshold (e.g., similarity $> 0.96$), the cached completion is returned immediately, bypassing LLM generation ($0\text{ms}$ LLM latency, $\$0$ token cost).
2. **Q: Why is OpenTelemetry span tracing critical for debugging RAG pipelines?**
   - *A:* RAG pipelines combine multiple decoupled steps (PII filtering, embedding, vector search, reranking, LLM generation). OpenTelemetry assigns nested spans to each step, allowing engineers to isolate exact latency bottlenecks and token cost drivers.
