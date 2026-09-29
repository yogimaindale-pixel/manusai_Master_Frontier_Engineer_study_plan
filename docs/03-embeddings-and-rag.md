# 03 — Embeddings, Vector Search, and RAG

## Embeddings & Similarity Mathematics

An **embedding** maps unstructured text into a dense real-valued vector space $\mathbb{R}^d$ (typically $d \in [768, 1536, 3072]$) such that semantically similar concepts are located near each other.

```
                    VECTOR DISTANCE METRICS MATHEMATICS
                    
   Cosine Similarity                     Dot Product                    Euclidean (L2) Distance
   cos(θ) = (A · B) / (||A|| ||B||)     A · B = Σ (A_i * B_i)           ||A - B||_2 = √(Σ (A_i - B_i)^2)
   Measures angle orientation           Measures angle + magnitude      Measures spatial distance
```

*Key Property:* When vector embeddings are unit-normalized ($\|A\| = \|B\| = 1$), Cosine Similarity equals the Dot Product, enabling ultra-fast matrix multiplication GPU operations.

---

## Chunking & Metadata Strategies

```
Raw Document ──> Semantic Chunking ──> Metadata Attachment ──> Vector Index + Payload
```

### Chunking Taxonomy

| Chunking Strategy | Mechanism | Advantage | Disadvantage |
|---|---|---|---|
| **Fixed-Size Overlapping** | Splits text every $N$ tokens with $K$ token overlap (e.g., 512/64). | Simple, consistent memory footprint. | Context truncation across sentence boundaries. |
| **Semantic Chunking** | Computes cosine distance between consecutive sentences; splits where similarity drops below threshold. | Preserves cohesive semantic thoughts. | Variable chunk sizes, higher processing overhead. |
| **Parent-Child (Small-to-Big)** | Embeds small 128-token chunks for vector retrieval match, but returns the parent 1024-token chunk to the LLM. | Maximum search precision + full context preservation. | Requires dual-index mapping storage. |

### Metadata Filtering
Vectors alone cannot enforce access control. Enterprise systems attach metadata filters:
`{"tenant_id": "org_42", "department": "finance", "security_clearance": "level_3"}`

Vector databases execute **Filtered Vector Search**:
- **Pre-filtering:** Filters by metadata FIRST, then runs vector search on the subset (Prevents security leaks).
- **Post-filtering:** Runs vector search, then discards unauthorized results (Risk of returning fewer than top-$k$ results if many matches are filtered out).

---

## Modern RAG Architecture Evolution

```
                           MODULAR RAG PIPELINE
                           
User Query ──> [Query Rewriter / HyDE] ──> Hybrid Retrieval (BM25 + Dense)
                                                       │
Answer <── [LLM Generation] <── [Cross-Encoder] <── [Reciprocal Rank Fusion]
```

### 1. Advanced Query Transformations
- **HyDE (Hypothetical Document Embeddings):** Uses an LLM to generate a hypothetical answer to the query first, embeds that hypothetical answer, and uses its vector to search the database.
- **Multi-Query Expansion:** Generates 3-5 variations of the user's prompt to overcome vocabulary mismatch and retrieves chunks for all queries.

### 2. Hybrid Search & Reciprocal Rank Fusion (RRF)
Combines **Sparse Keyword Search (BM25)** for exact keyword match (part numbers, proper nouns) with **Dense Vector Search (Cosine)** for semantic meaning.

Outputs are combined using **Reciprocal Rank Fusion (RRF)**:

$$RRF\_Score(d \in D) = \sum_{m \in M} \frac{1}{k + r_m(d)}$$

Where $M$ is the set of retrieval systems, $r_m(d)$ is document $d$'s rank position in model $m$, and $k$ is a constant (typically $k=60$).

### 3. Cross-Encoder Reranking
Vector search narrows millions of chunks down to Top-50 candidates using fast approximate dot products. A **Cross-Encoder Model** (e.g. Cohere Rerank, BGE-Reranker) then processes `(Query, Candidate Chunk)` pairs through full cross-attention layers to output a precise relevance score $[0.0, 1.0]$, trimming candidates to Top-5 for the LLM context window.

---

## Vector Indexing Algorithms (HNSW vs. IVF)

```
      Inverted File Index (IVF)                     Hierarchical Navigable Small World (HNSW)
  Partitions space into Voronoi cells.          Multi-layer graph navigation.
  Fast, lower RAM, requires retraining.        Ultra-fast ANN search, high memory consumption.
```

- **HNSW (Hierarchical Navigable Small World):** Builds a multi-layer skip-list graph. Top layers contain sparse long-range links; bottom layers contain dense local links. Achieves $O(\log N)$ Approximate Nearest Neighbor (ANN) search latency.
- **Product Quantization (PQ):** Compresses high-dimensional floating-point vectors into small byte codes (e.g., $1024 \times \text{FP32} = 4096\text{B} \to 64\text{B}$), allowing massive vector indexes to fit in RAM.

---

## RAG Evaluation Framework (The RAG Triad)

Evaluating RAG systems requires disaggregating **Retrieval Performance** from **Generation Performance** using the **RAG Triad**:

```
                       User Query
                       /        \
    [Context Relevance]          [Answer Relevance]
                     /            \
             Retrieved Context ─── Generated Answer
                    [Groundedness / Faithfulness]
```

| Metric | Measured Relationship | What Failure Indicates |
|---|---|---|
| **Context Relevance** | `(Query, Retrieved Context)` | Poor retrieval, bad chunking, missing documents, low vector quality. |
| **Groundedness / Faithfulness**| `(Retrieved Context, Answer)` | **Hallucination!** The LLM generated claims not present in the context. |
| **Answer Relevance** | `(Query, Answer)` | The LLM drifted off-topic or failed to follow prompt instructions. |

---

## 🎯 High-Yield Assessment Recall Questions

1. **Q: Why combine BM25 and Dense Vector Retrieval in a Hybrid Search?**
   - *A:* Dense vectors excel at capturing abstract semantic meaning but struggle with exact string lookups (e.g., serial numbers, error codes). BM25 handles exact keyword matches. Combining both via RRF provides high recall across all query types.
2. **Q: Define Groundedness in the RAG Triad.**
   - *A:* Groundedness measures the mathematical proportion of claims in the generated answer that are directly supported by evidence in the retrieved context chunks. Low groundedness signals hallucination.
