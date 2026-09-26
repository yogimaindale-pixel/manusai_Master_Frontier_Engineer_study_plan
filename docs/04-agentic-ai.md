# 04 — Agentic AI and Tool-Using Systems

## Definition

An agentic system uses a model to select or sequence actions toward a goal. It may plan, invoke tools, observe results, maintain state, and revise its next step.

```text
goal → plan → choose tool → validate action → execute → observe
     → verify → continue, request approval, or stop
```

## Components

- **Planner:** decomposes the goal.
- **Model:** interprets state and selects the next action.
- **Tools:** APIs, search, code execution, databases, browsers, and business systems.
- **State:** current task state; distinguish temporary context from durable memory.
- **Policy layer:** permissions, tool allow-list, approval rules, and data boundaries.
- **Executor:** runs validated actions with timeout and retry policy.
- **Verifier:** checks invariants, tests, citations, and business rules.
- **Stop condition:** prevents loops, runaway cost, and uncontrolled activity.
- **Observability:** records traces, tool calls, errors, latency, tokens, and outcomes.

## Safe loop

```python
for step in range(MAX_STEPS):
    plan = model(state, tools=ALLOW_LIST)
    if plan.requires_approval:
        return request_human_approval(plan.summary)
    action = validate_arguments(plan.action, policy=POLICY)
    if not action.allowed:
        return safe_stop("policy violation")
    result = execute(action, timeout=TIMEOUT)
    state = update(state, result)
    if not verify(state):
        return safe_stop("verification failed")
    if complete(state):
        return finalize(state)
return safe_stop("budget exhausted")
```

## Common patterns

- Single agent with a small tool set.
- Planner–executor separation.
- Router to specialist workflows.
- Parallel workers plus synthesizer.
- Critic–generator with independent tests.
- Human-in-the-loop for high-impact actions.
- Event-driven workflow with retries and idempotency.

## Safety controls

- Least privilege and separate read/write tools.
- Structured argument schemas.
- Allow-listed destinations and operations.
- Timeouts, quotas, budgets, and circuit breakers.
- Approval gates and environment separation.
- Idempotency keys and rollback.
- Prompt-injection defenses for tool results and external content.
- Audit logs and reproducible traces.

## Agent evaluation

Test task success, tool-selection accuracy, argument correctness, recovery from tool failure, loop termination, policy compliance, cost, latency, and human-approval behavior. Include adversarial and ambiguous tasks.

## Strong answer

> Trust the boundaries, not the agent. The model can propose an action; the application must decide whether that action is allowed.
