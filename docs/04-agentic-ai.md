# 04 — Agentic AI and Tool-Using Systems

## Core Definition & Architecture

An **Agentic AI System** is an autonomous control loop that utilizes an LLM to evaluate environment state, formulate multi-step execution plans, invoke external tools/APIs, observe tool outputs, and iteratively self-correct until a goal is achieved or a budget constraint is reached.

```
                  REACT AGENTIC CONTROL LOOP
                  
 User Goal ──> [LLM Planner] ──> Thought: "I need to query database"
                                       │
                                 Action: query_sql(sql="SELECT...")
                                       │
   [LLM Planner] <── Observation: "Query returned 14 rows" <── [Tool Execution]
         │
  Thought: "I have required data. Formulating final answer."
         │
   Final Answer
```

---

## Multi-Agent Topology & Orchestration Patterns

Enterprise workloads decompose complex tasks into multi-agent topologies to isolate system prompts, constrain tool access, and reduce context window noise.

```
1. ROUTER PATTERN                   2. SUPERVISOR (HIERARCHICAL)        3. PARALLEL (SWARM) PATTERN
   User Query                           Central Supervisor                 User Goal
       │                                     /     \                           /       \
   [Router]                               Worker1  Worker2                Agent A     Agent B
   /      \                                  \     /                          \       /
Sales    Support                              Aggregator                    [Consensus/Judge]
```

| Topology Pattern | Control Flow Mechanism | Ideal Application |
|---|---|---|
| **Single Agent (ReAct)** | Sequential loop of Thought $\to$ Tool Call $\to$ Observation. | Single-domain workflows (e.g. data lookup, simple calculator). |
| **Router Pattern** | Single LLM classifies user intent and routes execution to specialized sub-agent. | Customer support triage, multi-intent chatbots. |
| **Supervisor (Hierarchical)** | Central Supervisor decomposes task into sub-tasks, assigns to specialized worker agents, and aggregates results. | Complex software engineering, research report generation. |
| **Sequential (Pipeline)** | Output of Agent A feeds as context into Agent B. | Code generation $\to$ Code review $\to$ Unit test generation. |
| **Parallel / Swarm** | Multiple agents process the input concurrently; results merged by a Voting/Judge Agent. | Red Team / Blue Team security review, multi-perspective debate. |

---

## Reasoning Loops & Safety Control Mechanisms

### 1. ReAct Execution Loop Step-by-Step
1. **Input:** User Goal + Available Tool Schemas + Execution History.
2. **Thought:** LLM outputs reasoning tokens explaining next step.
3. **Action:** LLM selects tool name and generates valid JSON arguments.
4. **Tool Dispatcher:** Validates permissions and executes tool call.
5. **Observation:** Tool response string appended to context history.
6. **Evaluation:** LLM decides whether to loop back to Step 2 or return Final Answer.

### 2. Recursion Limits & Budget Enforcers
Unbounded agent loops lead to infinite execution loops, API rate-limit starvation, and financial cost explosions. Production agents enforce **Hard Safety Invariants**:

```python
# Production Agent Loop Safeguard Constraints
MAX_STEP_BUDGET = 10         # Maximum allowed tool call iterations
MAX_TIME_BUDGET_SEC = 30.0   # Hard wall-clock timeout
MAX_TOKEN_BUDGET = 16000     # Maximum cumulative token consumption
```

If `current_step >= MAX_STEP_BUDGET`, the runtime halts execution and invokes an **Abstention Fallback Handler**.

### 3. Human-In-The-Loop (HITL) Intercept Hooks
Tools are categorized by risk level:
- **Low Risk (Read-Only):** `search_docs()`, `calculate()` $\to$ Executed automatically.
- **High Risk (Side-Effects):** `execute_sql_delete()`, `transfer_funds()`, `send_email()` $\to$ Trigger an **Execution Pause Event** and notify a human supervisor. Execution resumes ONLY when a signed approval token is returned.

```
Agent Tool Request ──> [Risk Filter] ──> HIGH RISK? ──> Pause Execution & Send Alert
                                                                │
Action Executed <── Signed Human Token Approved <── [Human Review Portal]
```

---

## Tool Definition Schema Standard

Tools are exposed to the model via JSON Schema declarations matching the OpenAI / Anthropic function calling format:

```json
{
  "name": "get_customer_balance",
  "description": "Retrieves current account balance for a verified customer. Read-only operation.",
  "parameters": {
    "type": "object",
    "properties": {
      "customer_id": {
        "type": "string",
        "description": "Unique customer ID formatted as CUST_XXXXX"
      },
      "currency": {
        "type": "string",
        "enum": ["USD", "EUR", "GBP"],
        "default": "USD"
      }
    },
    "required": ["customer_id"]
  }
}
```

---

## State Management & Memory Architecture

```
                  AGENT MEMORY ARCHITECTURE
                  
  Short-Term Memory                     Long-Term Episodic Memory
  • Current Context Window               • Vector DB (Past user interactions)
  • Active Scratchpad History            • Semantic Knowledge Store
  • Ephemeral tool results               • Cross-session persistent preferences
```

### Context Window Compaction Strategies
As conversation history grows, the context window fills up. Production runtimes apply **State Compaction**:
- **Sliding Window:** Retains only the most recent $K$ message turns.
- **Summary Buffer:** Uses a background LLM to periodically compress older turns into a concise running summary paragraph, dropping raw message logs while keeping the summary.

---

## 🎯 High-Yield Assessment Recall Questions

1. **Q: How does a Supervisor Multi-Agent architecture prevent context window degradation?**
   - *A:* Worker agents execute in isolated sub-conversations with specialized context windows. Only concise summary outputs are returned to the Supervisor, keeping the Supervisor's context clean and uncluttered.
2. **Q: Why are Recursion Limits and Tool Risk Intercept Hooks essential in agentic deployment?**
   - *A:* Recursion limits prevent infinite tool-use loops and runaway API costs. Risk intercept hooks prevent agents from autonomously executing irreversible side-effects (e.g. database deletion or financial transfers) without human approval.
