# 01 — LLM and Generative AI Foundations

## Core model

A large language model estimates a probability distribution over the next token given the preceding context. It is a powerful generative component, not a built-in database of verified truth.

```text
text → tokens → representations → token probabilities → decoded response
```

## Essential concepts

| Concept | What to know | Design implication |
|---|---|---|
| Token | A word, subword, punctuation unit, or other model token. | Affects context capacity, cost, and latency. |
| Context window | Maximum combined input and output context. | More context can add noise; select relevant context. |
| Inference | Running model weights to produce output. | Prompting and decoding occur at inference time. |
| Temperature | Randomness control in token selection. | Lower for deterministic extraction; higher for creative variation. |
| Top-p | Nucleus sampling threshold. | Use deliberately; avoid tuning every sampling control at once. |
| Hallucination | Unsupported or false generated claim. | Use grounding, constraints, verification, and abstention. |
| Fine-tuning | Updating weights with task-specific examples. | Useful for behavior/style; not ideal for rapidly changing facts. |
| Tool calling | Model emits a structured request for an external function. | Validate arguments and enforce permissions outside the model. |

## Choose the right technique

- **Prompt engineering:** improve instructions, format, constraints, and task framing.
- **RAG:** add private, current, source-attributable knowledge at query time.
- **Fine-tuning:** improve repeatable behavior, style, classification, or domain conventions.
- **Model routing:** send each task to an appropriate model based on quality, cost, latency, privacy, and reliability.
- **Agent workflow:** enable bounded action through tools, state, verification, and explicit stop conditions.

## Failure modes to explain in the assessment

1. Ambiguous objective.
2. Missing or stale context.
3. Conflicting instructions.
4. Long, noisy context.
5. Unsupported certainty.
6. Invalid structured output.
7. Data leakage through prompts, logs, or responses.
8. Unsafe or over-privileged tool use.
9. Non-deterministic behavior without regression tests.
10. Provider, quota, timeout, or dependency failure.

## Evaluation dimensions

A complete evaluation considers correctness, task success, groundedness, safety, robustness, latency, cost, and user experience. Automated metrics help scale testing, but high-impact uses need human review and representative edge cases.

## High-value answer frame

> Identify the business goal, define the model contract, control the supplied context, validate the output, measure the outcome, and define safe behavior when evidence or dependencies are unavailable.
