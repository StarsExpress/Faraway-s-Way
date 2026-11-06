---
title: Softmax Saturation
weight: 3
math: true
sidebar:
  hide: true
cover: "/notes-cover/softmax_saturation.png"
subtitle: "How Softmax Behaves"
tags: ["Gen AI", "LLMs", "Math"]
---


## 🦦Continued From [Transformers Notes](/notes/transformers/)

Why does attention divide $QK^T$ by $\sqrt{d_k}$? This page covers:
- What saturation is.
- Why $QK^T$ spreads out as $d_k$ grows.
- Why $\frac{1}{\sqrt{d_k}}$ cancels that spread.


## ☠️What Saturation Looks Like

- Softmax: $p_i = \frac{e^{z_i}}{\sum_{j} e^{z_j}}$.

- Its derivative: $\frac{\partial p_i}{\partial z_j} = p_i (\delta_{ij} - p_j)$; $\delta_{ij} = 1$ if $i = j$, else $0$.

When inputs are far apart, one $p_k \approx 1$ and the rest $\approx 0$ — literally a **one-hot vector**.

Then, $p_i (1 - p_i) \approx 0$ and $-p_i p_j \approx 0$ for every entry, so almost **no gradient flows back** through Softmax. __This is saturation.__

Example — scores with spread 8 (what $d_k = 64$ gives, as derived below), before and after dividing by 8:

| Logits $z$ | Softmax Output Weights | Max Gradient $p_i (1 - p_i)$ |
|--------|-------------|----------|
| $(8, 0, -8)$ | $(0.99966, 0.00034, 0.0000001)$ | $3.4 \times 10^{-4}$ |
| $(1, 0, -1)$ | $(0.665, 0.245, 0.090)$ | $0.22$ |

Same ordering, but gradient signal is about 664× larger after scaling.


## 📏Why $QK^T$ Spreads Out

Each entry $(QK^T)_{i, j}$ is a sum of $d_k$ products. More terms, more spread. How much exactly?

### ☢️HIM — Heavy Incoming Math

Suppose all entries of $Q$ and $K$ are i.i.d. from a distribution of mean 0 and variance 1, implying several things:

- $E[Q_{i, h} \times K_{j, h}] = Cov(Q_{i, h}, K_{j, h}) + E[Q_{i, h}] \times E[K_{j, h}] = 0 + 0 \times 0 = 0.$

- $\forall a, b \in R, E[Q_{i, h}^a \times K_{j, h}^b] = \int \int Q_{i, h}^a K_{j, h}^b f(q, k) \; dq \; dk = \int \int Q_{i, h}^a K_{j, h}^b f_Q(q) f_K(k) \; dq \; dk = \int Q_{i, h}^a f_Q(q) dq \times \int K_{j, h}^b f_K(k) dk = E[Q_{i, h}^a] \times E[K_{j, h}^b]$.

- ☝️$f(q, k) = f_Q(q) f_K(k)$ decouples into an implied fact — $Q^2$ and $K^2$ are independent.

And this heaviest one:

$Var(Q_{i, h} \times K_{j, h}) = E[(Q_{i, h} \times K_{j, h})^2] - E[Q_{i, h} \times K_{j, h}]^2$

= $E[Q_{i, h}^2 \times K_{j, h}^2] - 0^2 = E[Q_{i, h}^2] \times E[K_{j, h}^2] = 1 \times 1 = 1.$

So we can see that our $(QK^T)_{i, j}$ distribution has:

### I. Expectation: 0

$E[(QK^T)_{i, j}] = E[Q_{i, :} \cdot (k_{j, :})^T] = E[\sum_{h = 1}^{d_k} Q_{i, h} \times K_{j ,h}]$

= $\underbrace{ \sum_{h = 1}^{d_k} E[Q_{i, h} \times K_{j ,h}] = \sum_{h = 1}^{d_k} E[Q_{i, h}] \times E[K_{j ,h}] }_{\text{All entries of Q and K are } \textbf{independent.}}$

= $\sum_{h = 1}^{d_k} 0 \times 0 = 0$, so $E[QK^T] = 0$.

### II. Variance: $d_k$

$Var((QK^T)_{i, j}) = Var(Q_{i, :} \cdot (k_{j, :})^T)$

= $\underbrace{ Var(\sum_{h = 1}^{d_k} Q_{i, h} \times K_{j ,h}) = \sum_{h = 1}^{d_k} Var(Q_{i, h} \times K_{j ,h}) }_{ \text{Each } h \text{ uses its own entries of Q and K, so } d_k \text{ products are } \textbf{independent} \text{ and variances add.}}$

= $\sum_{h = 1}^{d_k} 1 = d_k$, so $Var(QK^T) = d_k$.

### III. Assembly

**Scale** $QK^T$ by $\frac{1}{\sqrt{d_k}}$ to ensure $\text{Var}(\frac{QK^T}{\sqrt{d_k}}) = \frac{\text{Var}(QK^T)}{d_k} = \frac{d_k}{d_k} = 1$:

- When input follows a less extreme distribution, Softmax output tends to be less spiky.

- Less spiky means less one-hot encoding — __away from saturated vanishing-gradients areas.__


## 🧪Simple Trials

One query against 16 keys, entries i.i.d. standard normal, 20,000 trials:

```python
import numpy as np

def softmax(logits: np.ndarray) -> np.ndarray:
    max_col_entries = logits.max(axis=-1, keepdims=True)  # Shape: (N, 1).
    logits = logits - max_col_entries  # For overflow prevention. Shape: (N, D).
    
    softmax_output = np.exp(logits)
    softmax_output /= softmax_output.sum(axis=-1, keepdims=True)
    return softmax_output

rng = np.random.default_rng(0)
n_keys, trials = 16, 20000

for d_k in (16, 64, 256, 1024):
    q = rng.standard_normal((trials, 1, d_k))
    K = rng.standard_normal((trials, n_keys, d_k))
    
    scores = (q * K).sum(-1)  # Shape: (trials, n_keys).
    scaled_scores = scores / np.sqrt(d_k)

    print(
        d_k, scores.var(), scaled_scores.var(),
        softmax(scores).max(-1).mean(), softmax(scaled_scores).max(-1).mean()
    )
```

| $d_k$ | Var($QK^T$) | Var($\frac{QK^T}{\sqrt{d_k}}$) | Avg. Max $p$: Unscaled | Avg. Max $p$: Scaled |
|----|------|------|------|------|
| 16 | 16.07 | 1.00 | 0.685 | 0.244 |
| 64 | 63.94 | 1.00 | 0.840 | 0.244 |
| 256 | 256.57 | 1.00 | 0.923 | 0.246 |
| 1024 | 1023.13 | 1.00 | 0.962 | 0.246 |

Avg. max $p$: how much weight for the biggest Softmax entry:
- Uniform over 16 keys would be $0.0625$.
- Unscaled weights drift toward one-hot as $d_k$ grows.
- Scaled weights aren't affected by $d_k$.
