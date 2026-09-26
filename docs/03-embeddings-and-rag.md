# 03 — Embeddings, Vector Search, and RAG

## Embeddings

An embedding is a vector representation of text or another object. Vector distance provides a similarity signal. It does not prove that a document is correct, current, or authorized.

Common uses include semantic search, clustering, recommendations, classification, and anomaly detection.

## Indexing pipeline

```text
data sources → parse/OCR → clean and redact → chunk → attach metadata
→ embed → store vector + source text + metadata → build index → monitor freshness
```

## Query pipeline

```text
user question → authenticate → safety check → optional query rewrite
→ embed query → vector and keyword retrieval → metadata/permission filter
→ rerank → pack context under token budget → grounded prompt → generate
→ citation/claim validation → response and audit trace
```

## Terms and trade-offs

- **Cosine similarity:** compares vector direction.
- **Top-k:** candidate count; too low misses evidence, too high adds noise and cost.
- **Chunk size:** small improves precision; large preserves more context.
- **Overlap:** preserves boundary meaning but increases storage and tokens.
- **Metadata filtering:** restricts by tenant, permission, date, product, or type.
- **Hybrid search:** combines lexical matching with semantic search for exact IDs and names.
- **Reranking:** applies stronger relevance scoring to a smaller candidate set.
- **Freshness:** changed data requires re-ingestion or embedding updates.
- **Access control:** authorization must happen before context reaches the model.

## Diagnose a bad RAG answer

| Symptom | Investigate |
|---|---|
| No relevant evidence | Parsing, chunking, query formulation, filters, index, embedding model |
| Relevant evidence but wrong answer | Context order, prompt, model behavior, output validation |
| Correct but expensive or slow | Candidate count, reranking, caching, model routing, batching |
| Unauthorized content | Permission filter, tenant isolation, document ACL synchronization |
| Stale answer | Source update pipeline, index freshness, version tracking |

## Evaluation

Measure retrieval and generation separately:

- Recall@k and precision@k.
- MRR or nDCG for ranking.
- Context relevance and coverage.
- Groundedness or faithfulness.
- Answer relevance.
- Citation correctness.
- Abstention quality.
- End-to-end task success.
- Latency, cost, and freshness.

## RAG versus fine-tuning

RAG is usually preferred for changing, private, or citeable facts. Fine-tuning is usually preferred for stable task behavior, style, or classification patterns. They can be combined.

## Key assessment insight

RAG is not merely a vector database. It is a data pipeline, retrieval layer, prompt contract, generation step, evaluation system, access-control boundary, and operations surface.
