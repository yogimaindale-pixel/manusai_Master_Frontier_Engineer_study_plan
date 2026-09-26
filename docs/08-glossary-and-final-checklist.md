# 08 — Glossary and Final Checklist

## Glossary

- **Agent:** model-driven system that selects or sequences actions toward a goal.
- **Embedding:** vector representation used to compare relatedness.
- **Fine-tuning:** updating model weights with task-specific examples.
- **Groundedness:** degree to which a response is supported by supplied evidence.
- **Hallucination:** unsupported or false generated content.
- **Hybrid search:** lexical and semantic retrieval combined.
- **Inference:** running a trained model to produce output.
- **LLM:** large language model that predicts token sequences.
- **Metadata filter:** retrieval constraint based on attributes such as tenant, date, or permission.
- **Prompt injection:** malicious or conflicting instruction intended to change system behavior.
- **RAG:** retrieval-augmented generation using external knowledge at query time.
- **Reranking:** stronger relevance scoring applied to a retrieved candidate set.
- **Top-k:** number of results selected or returned.
- **Tool:** callable external function or service used by a model or workflow.
- **Vector database:** system for storing and searching vector representations.

## Final checklist

Before submitting an assessment answer, ask:

- Did I state the goal, users, assumptions, and risk tier?
- Did I distinguish prompting, RAG, fine-tuning, and agents?
- Did I show both ingestion-time and query-time paths for RAG?
- Did I enforce authorization before model context?
- Did I treat external content as untrusted?
- Did I include structured output validation?
- Did I define abstention and human review?
- Did I bound agent steps, tools, permissions, cost, and latency?
- Did I name evaluation metrics and representative test cases?
- Did I cover monitoring, versioning, rollback, and incident response?
- Did I explain at least one trade-off?

> Build the smallest system that can prove what it did, explain why it did it, and stop safely when it does not know.

## References

[1]: https://developers.openai.com/api/docs/guides/embeddings "OpenAI Developers: Vector embeddings"
[2]: https://aws.amazon.com/what-is/retrieval-augmented-generation/ "AWS: What is Retrieval-Augmented Generation?"
[3]: https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence "NIST: Generative AI Profile"
[4]: https://owasp.org/projects/top-10-for-large-language-model-applications "OWASP: Top 10 for Large Language Model Applications"
[5]: https://kiro.dev/topics/frontier-engineering/ "Kiro: Frontier engineering principles"
