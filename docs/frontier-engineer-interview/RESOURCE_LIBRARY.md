# Free Resource Library

Use **Start here** first. Optional resources are marked so the candidate can prioritize. Links were checked during preparation, but product pages, URLs, pricing, model catalogs, and previews change. Recheck current status before relying on a product-specific claim.

## Start here

1. [Google: Introduction to Large Language Models](https://developers.google.com/machine-learning/crash-course/llm) — Google for Developers; free official module; foundation/intermediate; tokens, Transformers, context, fine-tuning, limitations; useful for a structured 45-minute refresh.
2. [Microsoft: RAG solution design and evaluation guide](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/rag/rag-solution-design-and-evaluation-guide) — official architecture guide; free; intermediate/advanced; ingestion, hybrid retrieval, agentic RAG, evaluation, and reproducible experiments.
3. [Anthropic: Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) — official engineering article; free; intermediate; workflows versus agents and orchestration patterns. Treat dated tooling details as historical and verify current APIs.
4. [NIST: AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — government framework; free; all levels; Govern, Map, Measure, Manage, profiles, and Playbook.
5. [OWASP: 2025 LLM and GenAI risks](https://genai.owasp.org/llm-top-10/) — free security guidance; intermediate; injection, disclosure, supply chain, poisoning, unsafe output, excessive agency, vectors, misinformation, and unbounded use.

## Official foundations

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — Vaswani et al.; free paper; advanced reference for Transformer attention.
- [OpenAI: Vector embeddings](https://developers.openai.com/api/docs/guides/embeddings) — official API docs; free; embeddings, similarity, search, clustering, recommendations, classification.
- [AWS: What is RAG?](https://aws.amazon.com/what-is/retrieval-augmented-generation/) — free official explainer; ingestion, retrieval, augmentation, freshness, citations, access controls.
- [Hugging Face LLM Course](https://huggingface.co/learn/llm-course/en/chapter1/1) — free course; registration not required to read; Transformers, datasets, tokenizers, fine-tuning, reasoning.
- [Python Tutorial](https://docs.python.org/3/tutorial/index.html) — official Python documentation; free; language and standard-library fundamentals.

## Microsoft platform and architecture

- [Agents in Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/overview) — free documentation; deployments require Azure usage; managed agents, prompt/hosted agents, tools/MCP, identity, observability, publishing.
- [Microsoft Foundry Models overview](https://learn.microsoft.com/en-us/azure/foundry/concepts/foundry-models-overview) — free documentation; model catalog, lifecycle, regions, deployment, safety, pricing, and support considerations.
- [AI agent orchestration patterns](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns) — free architecture guide; sequential, concurrent, handoff, group chat, orchestrator-workers, and adaptive patterns.
- [Microsoft Copilot Studio RAG guidance](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/retrieval-augmented-generation) — free documentation; platform-specific RAG patterns; verify current product behavior.
- [Microsoft governance/security across organization](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai-agents/governance-security-across-organization) — free Cloud Adoption Framework guidance; governance, identity, security, and operating model.
- [Microsoft AI Red Teaming Agent](https://learn.microsoft.com/en-us/azure/foundry/concepts/ai-red-teaming-agent) — free documentation; preview/support limitations apply; agent red teaming and indirect injection.

## MCP and A2A

- [MCP Specification](https://modelcontextprotocol.io/specification/2026-07-28) — free official protocol specification; host/client/server, JSON-RPC, tools/resources/prompts, capabilities.
- [MCP Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices) — free; confused deputy, OAuth state, token passthrough, scopes, consent.
- [A2A Protocol repository](https://github.com/a2aproject/A2A) — free open-source project; Agent Cards, tasks, artifacts, streaming, push, security, versioning.
- [A2A samples](https://github.com/a2aproject/a2a-samples) — free runnable samples; local quick starts and SDK examples.
- [A2A and MCP comparison](https://github.com/a2aproject/A2A/blob/main/docs/topics/a2a-and-mcp.md) — free; vertical tool/context integration versus horizontal agent collaboration.
- [A2A purchasing concierge codelab](https://codelabs.developers.google.com/intro-a2a-purchasing-concierge) — free lab content; cloud deployment needs a Google Cloud billing project.

## Knowledge graphs and Graph RAG

- [W3C RDF](https://www.w3.org/RDF/) — free standard overview; triples, identifiers, graph merging.
- [W3C OWL](https://www.w3.org/OWL/) — free standard overview; ontology semantics, inference, and reasoning.
- [Microsoft GraphRAG documentation](https://microsoft.github.io/graphrag/) — free open-source docs; indexing, communities, local/global/DRIFT/baseline search.
- [Microsoft Research GraphRAG](https://www.microsoft.com/en-us/research/project/graphrag/) — free research project page; links to paper and current project context.
- [From Local to Global: Graph RAG](https://arxiv.org/abs/2404.16130) — free paper; community summaries and global sensemaking results for a specific setting.
- [Google Enterprise Knowledge Graph overview](https://docs.cloud.google.com/enterprise-knowledge-graph/docs/overview) — free documentation; reconciliation, RDF, standardization; Preview caveats apply.

## Evaluation, security, and governance

- [NIST GenAI Profile](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence) — free official profile/PDF; lifecycle risks and trustworthiness.
- [NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/) — free practical reference; functions, categories, and stakeholder use.
- [OWASP LLM Top 10](https://genai.owasp.org/llm-top-10/) — free security risks and mitigations.
- [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/) — free API security; object/function authorization, resource consumption, SSRF, inventory.
- [Agentic AI Red Teaming Guide](https://cloudsecurityalliance.org/artifacts/agentic-ai-red-teaming-guide) — free landing page; downloadable guide may require free login; permission escalation, memory, orchestration, supply chain.
- [Microsoft PyRIT](https://github.com/microsoft/PyRIT) — free open-source red-team framework; API usage may cost; risk identification and adversarial testing.
- [Ragas testset generation](https://docs.ragas.io/en/stable/getstarted/rag_testset_generation/) — free docs/open source; RAG test generation and multi-hop scenarios.
- [ISO/IEC 42001 explained](https://www.iso.org/home/insights-news/resources/iso-42001-explained-what-it-is.html) — free official explainer; full standard is not free; AI management systems and continual improvement.

## Observability and operations

- [OpenTelemetry overview](https://opentelemetry.io/docs/what-is-opentelemetry/) — free official docs; traces, metrics, logs, Collector, semantic conventions.
- [OpenTelemetry Logs specification](https://opentelemetry.io/docs/specs/otel/logs/) — free specification; log data model and trace correlation.
- [Google SRE: Implementing SLOs](https://sre.google/workbook/implementing-slos/) — free workbook; SLO process and error budgets.
- [Google SRE: Error budget policy](https://sre.google/workbook/error-budget-policy/) — free example; release/reliability policy patterns.
- [Kubernetes HPA](https://kubernetes.io/docs/concepts/workloads/autoscaling/horizontal-pod-autoscale/) — free docs; metrics, control loop, missing metrics, stabilization.
- [Kubernetes probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) — free docs; startup/readiness/liveness behavior.

## AI-assisted development and delivery

- [GitHub Copilot best practices](https://docs.github.com/en/copilot/get-started/best-practices) — free docs; prompting, review, tests, code scanning, public-code controls.
- [GitHub Actions security](https://docs.github.com/actions/security-for-github-actions) — free docs; OIDC, attestations, provenance, permissions.
- [FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/) — free docs; typed API, validation, security, testing, deployment.
- [Pro Git: branching workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows) — free book; topic and long-running branches, merges, workflow trade-offs.
- [Docker overview](https://docs.docker.com/get-started/docker-overview/) — free docs; images, containers, registries, layers, CI/CD.
- [NIST SSDF](https://csrc.nist.gov/projects/ssdf) — free framework; secure software lifecycle and GenAI SSDF profile links.

## Client advisory, adoption, and ROI

- [Azure Cloud Adoption Framework strategy](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/strategy/) — free; motivation, mission, objectives, sponsorship, strategy iteration.
- [AWS adoption management](https://docs.aws.amazon.com/prescriptive-guidance/latest/oca-framework-make-culture-change-stick/adoption.html) — free; leadership, champions, communication, training, readiness, usage, feedback.
- [Prosci ADKAR](https://www.prosci.com/methodology/adkar) — free overview; Awareness, Desire, Knowledge, Ability, Reinforcement; not a full enterprise methodology.
- [FinOps Unit Economics](https://www.finops.org/framework/capabilities/unit-economics/) — free; cost per transaction/case/request/token and value linkage.
- [Google Cloud: Align cloud spending with business value](https://docs.cloud.google.com/architecture/framework/cost-optimization/align-cloud-spending-business-value) — free; TCO, FinOps, DORA/SRE, unit economics.

## YouTube and conference searches

Use official channels and verify the exact video title, date, and duration before scheduling it. Recommended searches are safer than copying unverified timestamps:

- [Microsoft Reactor: AI agents, RAG, and MCP search](https://www.youtube.com/@MicrosoftReactor/search?query=AI%20agents%20RAG%20MCP) — free video library; platform and architecture talks.
- [Microsoft Azure Developers search for AI agents](https://www.youtube.com/@MicrosoftAzure/search?query=AI%20agents) — free; Foundry and Azure implementation demonstrations.
- [GitHub official Copilot search](https://www.youtube.com/@GitHub/search?query=Copilot) — free; AI-assisted development and workflow examples.
- [Google Cloud Tech A2A search](https://www.youtube.com/@googlecloudtech/search?query=A2A%20agent) — free; A2A and agent interoperability talks.

## Resource use rule

For every resource, write one sentence: **“I will use this to explain/design/test…”**. Stop collecting links once you can explain the concept, draw the architecture, implement a small proof, and defend one trade-off.
