# 06 — Coding Challenge and AI-Assisted Engineering Playbook

## Five passes

1. **Restate:** input, output, constraints, assumptions, and edge cases.
2. **Example:** trace a normal case and a boundary case.
3. **Design:** choose data structures and complexity before coding.
4. **Implement:** small functions, clear names, explicit errors.
5. **Verify:** tests, complexity, security, and explanation of trade-offs.

## AI-assisted workflow

```text
specification → plan → small implementation → run tests → inspect diff
→ fix one failure → security review → final explanation
```

Ask an AI coding assistant for assumptions, tests, and a change summary. Review the diff. Plausible code is not verified code.

## Edge-case checklist

- Empty input, one item, duplicates, negative values, and large values.
- Null or missing API fields.
- Time and memory complexity.
- Deterministic ordering where tests expect it.
- Unicode, time zones, pagination, and numeric precision where relevant.
- Input validation, safe errors, and secret handling.
- SQL, command, path, and prompt injection.
- Retries, timeouts, idempotency, and partial failure for external calls.

## Mini implementation pattern

```python
def select_context(results, tenant_id, max_items=5, min_score=0.70):
    selected = []
    for item in results:
        if item.get("tenant_id") != tenant_id:
            continue
        if not item.get("text", "").strip():
            continue
        if item.get("score", 0.0) < min_score:
            continue
        selected.append(item)
        if len(selected) == max_items:
            break
    return selected
```

Assessment discussion: authorization is enforced before context reaches the model; thresholds require evaluation; limits protect context and cost; production behavior for missing scores and stale documents must be defined.

## Code-quality answer frame

Explain correctness, complexity, validation, failure behavior, security implications, test cases, and how the implementation would be observed in production.
