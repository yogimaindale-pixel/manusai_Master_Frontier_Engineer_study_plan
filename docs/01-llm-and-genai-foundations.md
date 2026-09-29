# 01 — LLM and Generative AI Foundations

## Core Architecture & Mathematical Mechanics

A Large Language Model (LLM) is an autoregressive neural network that models the probability distribution of the next token $x_t$ given a sequence of preceding tokens $x_1, x_2, \dots, x_{t-1}$:

$$P(x_1, x_2, \dots, x_T) = \prod_{t=1}^{T} P(x_t \mid x_1, x_2, \dots, x_{t-1})$$

Models are generative components that estimate statistical likelihoods over learned data distributions—they are not authoritative databases of factual truth.

---

### Transformer Self-Attention Mechanics

The foundational building block of modern LLMs (Decoder-only architecture like Llama, Claude, GPT-4, Gemini) is the **Scaled Dot-Product Self-Attention**:

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

Where:
- $Q = X W_Q \in \mathbb{R}^{N \times d_k}$ (Query matrix: what token $t$ is looking for)
- $K = X W_K \in \mathbb{R}^{N \times d_k}$ (Key matrix: what information token $i$ contains)
- $V = X W_V \in \mathbb{R}^{N \times d_v}$ (Value matrix: the actual representation transferred)
- $d_k$ is the dimensionality of the key vectors (scaling by $\frac{1}{\sqrt{d_k}}$ prevents vanishing gradients in the softmax).

```
Input Tokens X ──> [W_Q, W_K, W_V Projections] ──> Q, K, V
                                                     │
   QK^T Matrix Multiply ──> Scale (1/√d_k) ──> Softmax (Attention Weights)
                                                     │
   Multiply by V ──> Output Contextual Embeddings ──> Feed-Forward Network (FFN)
```

#### Attention Variations Comparison

| Attention Pattern | Description | KV Cache Memory Impact | Production Use Case |
|---|---|---|---|
| **Multi-Head Attention (MHA)** | Every head has its own $Q, K, V$ projection matrices. | High memory footprint ($2 \times b \times s \times l \times h$). | Baseline Transformers (GPT-3, Original Transformer). |
| **Multi-Query Attention (MQA)** | All query heads share a single $K$ and $V$ head. | Up to $8\times$ reduction in KV cache footprint. | Extreme latency reduction, high concurrency serving. |
| **Grouped-Query Attention (GQA)** | Query heads are partitioned into groups (e.g. 8 query heads per 1 KV head). | Optimal trade-off between memory and quality. | Modern LLMs (Llama 3, Mistral, Gemma). |

---

## Tokenization & Context Window Dynamics

### Tokenization Mechanics
LLMs do not process raw strings; they process numeric token IDs via Byte Pair Encoding (BPE) or SentencePiece.

- **Token-to-Word Ratio:** $\approx 1.3 \text{ tokens per English word}$. Code and non-English languages often require significantly more tokens per word due to subword splitting.
- **Context Window Overhead:** Total Context = $\text{Input Prompt Tokens} + \text{Generated Completion Tokens}$. Exceeding context limits causes truncation errors or degradation of long-context recall (the "Needle in a Haystack" loss phenomenon).

---

## Sampling & Decoding Strategies

During inference, the final layer produces raw unnormalized scores called **logits** $z_i \in \mathbb{R}^{|V|}$. Sampling transforms these logits into a final output token choice.

```
Logits z_i ──> Temperature Scaling (z_i / T) ──> Softmax ──> Top-k / Top-p Filter ──> Sample Output Token
```

### 1. Temperature Scaling ($T$)
Modifies logit sharpness before applying Softmax:

$$P(x_i) = \frac{\exp(z_i / T)}{\sum_{j} \exp(z_j / T)}$$

- **$T \to 0$ (Greedy Decoding):** Forces selection of the single highest-probability token ($P(x_{\text{max}}) \to 1$). Used for code generation, JSON extraction, and deterministic tasks.
- **$T = 0.7 - 1.0$:** Introduces controlled diversity. Used for creative writing and open-ended brainstorming.
- **$T > 1.2$:** Increases entropy, leading to erratic output, grammar collapse, and repetitive loops.

### 2. Top-$k$ and Top-$p$ (Nucleus Sampling)
- **Top-$k$ Sampling:** Truncates the candidate token vocabulary to the top $k$ most probable tokens before sampling.
- **Top-$p$ (Nucleus) Sampling:** Dynamically selects the smallest cumulative set of tokens whose aggregate probability exceeds threshold $p$ ($\sum_{x \in S} P(x) \ge p$). Adapts automatically to sharp vs. flat distributions.

---

## Model Architectures, Quantization & Latency

### Dense vs. Mixture of Experts (MoE)
- **Dense Architecture:** Every model parameter is activated for every single token pass.
- **Mixture of Experts (MoE):** Replaces dense Feed-Forward Networks (FFN) with multiple sparse "Expert" sub-networks. A learned **Router Network (Gating Mechanism)** routes each token to the top $k$ experts (e.g. 2 out of 8 experts per token).
  - *Advantage:* Achieves the parameter capacity of a 47B model while spending the computational latency (FLOPs) of a 13B model per token.

### Quantization Frameworks
Quantization reduces weight precision from 32-bit/16-bit floating point down to lower bit representations (e.g. INT8, INT4), dramatically reducing VRAM requirements with minimal loss in perplexity.

| Quantization Type | Memory Reduction | Deployment Context | Popular Formats |
|---|---|---|---|
| **FP16 / BF16** | Baseline (2 Bytes/param) | High-precision cloud training/serving | PyTorch native |
| **INT8 (LLM.int8())** | $2\times$ VRAM saving | Balanced server inference | TensorRT-LLM, bitsandbytes |
| **INT4 (AWQ / GPTQ / GGUF)** | $4\times$ VRAM saving | Edge execution, local desktop, high throughput | Ollama, llama.cpp, vLLM |

### Inference Latency Metrics
- **Time to First Token (TTFT):** Time taken to process the input prompt (Prefill phase). Bound by **Compute / FLOPs**.
- **Time Per Output Token (TPOT):** Inter-token latency for generating sequential tokens (Decode phase). Bound by **Memory Bandwidth** (fetching model weights per token).

---

## Fine-Tuning & Alignment Spectrum

```
Raw Pre-Training ──> Supervised Fine-Tuning (SFT) ──> Preference Alignment (RLHF / DPO)
```

1. **Pre-Training:** Unsupervised training on massive web scale corpora to learn next-token language representations.
2. **Supervised Fine-Tuning (SFT):** Training on curated (Instruction, Response) pairs to teach conversational behavior.
3. **LoRA (Low-Rank Adaptation):** Parameter-efficient fine-tuning (PEFT) that freezes base weights $W_0$ and injects trainable rank-decomposition matrices $A$ and $B$:

$$W = W_0 + \Delta W = W_0 + \frac{\alpha}{r} (B A)$$

Where $A \in \mathbb{R}^{r \times d}$ and $B \in \mathbb{R}^{k \times r}$ with rank $r \ll \min(d, k)$, reducing trainable parameters by $>99\%$.

4. **Preference Alignment (RLHF vs. DPO):**
   - **RLHF (Reinforcement Learning from Human Feedback):** Trains a separate Reward Model, then uses PPO (Proximal Policy Optimization) to align responses.
   - **DPO (Direct Preference Optimization):** Mathematically optimizes the policy directly on preferred vs. rejected response pairs $(x, y_w, y_l)$ without training a separate reward model.

---

## 🎯 High-Yield Assessment Recall Questions

1. **Q: Why does scaling $QK^T$ by $\frac{1}{\sqrt{d_k}}$ matter in self-attention?**
   - *A:* For large projection dimensions $d_k$, dot products grow large in magnitude, pushing the Softmax function into regions with extremely small gradients (vanishing gradient problem).
2. **Q: Contrast TTFT and TPOT bottlenecks in enterprise inference.**
   - *A:* TTFT (Prefill phase) processes all prompt tokens in parallel and is compute-bound. TPOT (Decode phase) generates tokens sequentially and is memory-bandwidth bound.
