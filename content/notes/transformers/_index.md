---
title: Transformers
weight: 2
math: true
sidebar:
  hide: true
cover: "/notes-cover/transformers.png"
subtitle: "Hood under attention"
tags: ["Gen AI", "LLMs", "Math"]
---


## 1. Attention

Some feelings about how can these attention-powered transformers work so well in Gen AI:

Natural language, or any sequence data, is literally like a more advanced Solitaire game.

Each position can — and should — look way beyond just the previous position.

### 3️⃣ Roles Inside Context

To better look deeper beneath context, each word is treated in 3 ways:

- Query $Q$: which topic is searched for.
- Key $K$: how each candidate is labeled.
- Value $V$: what content each candidate holds.

So — if each $K$ pairs with a $V$, then how about we use $Q$ & $K$
matching results to decide how much each $V$ plays?

### Stage ⓵: Scaled Dot-Product

- $Q = R^{n \times d_k}, K = R^{n \times d_k}, V = R^{n \times d_v}.$

- $\text{Attention}(Q, K, V) = \text{Softmax} \frac{ QK^T }{ \sqrt{d_k} } V$.

- $(QK^T)_{i, j} = Q_{i, :} \cdot (k_{j, :})^T$.

#### ☢️HIM — Heavy Incoming Math

Suppose all entries of $Q$ and $K$ are i.i.d. from a distribution of mean 0 and variance 1, implying several things:

- $E[Q_{i, h} \times K_{j, h}] = Cov(Q_{i, h}, K_{j, h}) + E[Q_{i, h}] \times E[K_{j, h}] = 0 + 0 \times 0 = 0.$

- $\forall a, b \in R, E[Q_{i, h}^a \times K_{j, h}^b] = \int \int Q_{i, h}^a K_{j, h}^b f(q, k) \; dq \; dk = \int \int Q_{i, h}^a K_{j, h}^b f_Q(q) f_K(k) \; dq \; dk = \int Q_{i, h}^a f_Q(q) dq \times \int K_{j, h}^b f_K(k) dk = E[Q_{i, h}^a] \times E[K_{j, h}^b]$.

- ☝️$f(q, k) = f_Q(q) f_K(k)$ decouples into an implied fact — $Q^2$ and $K^2$ are independent.

And this heaviest one:

$Var(Q_{i, h} \times K_{j, h}) = E[(Q_{i, h} \times K_{j, h})^2] - E[Q_{i, h} \times K_{j, h}]^2$

= $E[Q_{i, h}^2 \times K_{j, h}^2] - 0^2 = E[Q_{i, h}^2] \times E[K_{j, h}^2] = 1 \times 1 = 1.$

So we can see that our $(QK^T)_{i, j}$ distribution has:

#### I. Expectation: 0

$E[(QK^T)_{i, j}] = E[Q_{i, :} \cdot (k_{j, :})^T] = E[\sum_{h = 1}^{d_k} Q_{i, h} \times K_{j ,h}]$

= $\underbrace{ \sum_{h = 1}^{d_k} E[Q_{i, h} \times K_{j ,h}] = \sum_{h = 1}^{d_k} E[Q_{i, h}] \times E[K_{j ,h}] }_{\text{All entries of Q and K are } \textbf{independent.}}$

= $\sum_{h = 1}^{d_k} 0 \times 0 = 0$, so $E[QK^T] = 0$.

#### II. Variance: $d_k$

$Var((QK^T)_{i, j}) = Var(Q_{i, :} \cdot (k_{j, :})^T)$

= $\underbrace{ Var(\sum_{h = 1}^{d_k} Q_{i, h} \times K_{j ,h}) = \sum_{h = 1}^{d_k} Var(Q_{i, h} \times K_{j ,h}) }_{ \text{All entries of Q and K are } \textbf{independent so } Cov(Q_{i, h}, K_{j ,h}) = 0.}$

= $\sum_{h = 1}^{d_k} 1 = d_k$, so $Var(QK^T) = d_k$.

#### III. Assembly

**Scale** $QK^T$ by $\frac{1}{\sqrt{d_k}}$ to ensure $\text{Var}(\frac{ QK^T }{ \sqrt{d_k} }) = 1$:

- When input follows a less extreme distribution, Softmax output tends to be less spiky.

- Less spiky means less one-hot encoding — __away from saturated vanishing-gradients areas.__

__Dot product__ comes afterward:

- $P = \text{Softmax}(\frac{ QK^T }{ \sqrt{d_k} }) = R^{n \times n}$: with $\sum_{j = 1}^n P_{i, j} = 1$, $P_{i, j}$ is $i^{th}$ token's __relative proportion of attention__ to $j^{th}$ token.

- $V = R^{n \times d_v}:$ each $V_{i, :} = R^{d_v}$ means $i^{th}$ token's value representation.

- $PV$ yields a matrix of $R^{n \times d_v}$,
where each $i^{th}$ row vector is a **weighted sum of token vectors.**

- Weights are based on **$i^{th}$ token's attention to all $n$ tokens.**

Be sure to set `axis=-1` when doing Softmax. Along columns 😉😏

#### IV. 🤿Mask If Required

After doing $\frac{ QK^T }{ \sqrt{d_k} }$, you have a square matrix $M = R^{n \times n}$,
showing how much relative attention each token pays to other tokens.

But.....what if at times you aren't totally free to pay attention?

Like, you can only pay attention to past and present. Not future.

Straightforward: given any $i$, set $M_{i, j} = -\infty \; \forall \; i < j$ to have $e^{-\infty} = 0$.

Which is just to let Softmax produce 0, representing **no attention**, on disabled entries.

```
ones_matrix = torch.ones_like(qk_product)  # Shape: (n, n).

# Diagonal = 1: don't mask entire diagonal as well, cuz present can be referenced.
mask = torch.triu(ones_matrix, diagonal=1).bool()

qk_product.masked_fill_(mask, float("-inf"))
```

⬆️This code helps us mask out all upper triangular entries before entering Softmax.

### Stage ⓶: MHA — Multi-Head

Simple intro: $h$ sets of 3 linear projections before scaled dot-product.

Philosophy behind: have more matrices **track how each token behaves in a certain semantic space.**

#### Detailed Maneuvers 🥘

Now $Q = K = V = R^{n \times d_{model}}.$ Each $l^{th}$ head owns 3 weight matrices called:

- $W_l^Q = R^{d_{model} \times d_k}$ — Help produce $Q_l = Q W_l^Q = R^{n \times d_k}$.

- $W_l^K = R^{d_{model} \times d_k}$ — Help produce $K_l = K W_l^K = R^{n \times d_k}$.

- $W_l^V = R^{d_{model} \times d_v}$ — Help produce $V_l = V W_l^V = R^{n \times d_v}$.

So $Q_l, K_l, V_l$ all fit into expected input shapes of scaled dot-product.

$l^{th}$ scaled dot-product attention is $A_l = R^{n \times d_v}$. We will have $h$ such matrices.

With each row referring to a token, we of course can only concatenate attention along columns.

By doing so, we obtain $O = \text{Concat}(A_1, ..., A_h) = R^{n \times h d_v}$.

Using another weight matrix $W^O = R^{h d_v \times d_{model}}$, we can reach $O W^O = R^{n \times d_{model}}$.

Yes, back to original shape of $Q, K, V$, but carrying learned attention.

#### 🤔Subtle & Vital Guideline

For MHA, we need extra $3h + 1$ weight matrices:

- First $2h$ matrices — all $R^{d_{model} \times d_k}$ that deal with $Q, K$.

- Middle $h$ matrices — all $R^{d_{model} \times d_v}$ that deal with $V$.

- Last 1 matrix — $R^{h d_v \times d_{model}}$ that brings back to original shape.

All these $3h + 1$ matrices share a common: dimensions contain no $n$.

#### At the End of the Day

Attention is to grasp semantic relationship among $n$ tokens.

$d_{model}, d_k, d_v, h d_v$ are all semantic spaces' dimensions. $n$ isn't.

__These $3h + 1$ weight matrices are born to travel around semantic spaces. 😉😏__

### 🕰️Time Complexity

$QK^T: O(n^2 \times d_k)$

- $R^{n \times d_k} \cdot R^{d_k \times n}$ — **$n^2$ pairs** of $R^{d_k}$ doing inner product, with $O(d_k)$ per pair.

$\sqrt{d_k}$ Scaler & Softmax: $O(n^2)$

- They both perform constantly on all $n^2$ entries inside $QK^T$.

V Multiplication: $O(n^2 \times d_k)$

- $R^{n \times n} \cdot R^{n \times d_k}$ — **$n \times d_k$ pairs** of $R^n$ doing inner product, with $O(n)$ per pair.

So total is $O(n^2 \times d_k) + O(n^2) + O(n^2 \times d_k) = O(n^2 \times d_k).$


## 2. Generation 🏳️‍🌈

How to make good use of these learned attention?

Like writing a research paper:

- **Ensure no topic deviation** — look at already written parts.
- **Extend relevant content** — make reference to other papers.

Although I've never written a research paper 😎

### 🎭Masked Attention: No Deviation

Like I said, to prevent topic deviation, we must look at already written parts.

So in each step, already generated output is naturally the sources for our $Q$, $K$ and $V$.

The adjective "masked" means __only previous and current steps' output is eligible.__

### ⚔️Cross-Attention: Make Reference

Where can we let generated output make reference to incorporate relevant content?

Encoder's learned attention, aka encoder stack. Thus, this time:

- Encoder stack attention serves as sources of $K$ & $V$, since it gets referenced.

- We are allowed to look at every step. So no masks at all.

### Tip: Output Is Topic Regardless

| Attention | $Q$ source | $K$ & $V$ sources | Masked |
|--------|-------------|----------|----------|
| Masked |  Output. | **Output**. | Yes. |
| Cross | Output. | __Encoder stack attention__. | No. |


## 3. KV Cache

Now comes a discussion: can we optimize time or space complexities?

This brings up a quite fundamental topic indeed. Need a whole new page.


## 4. Positional Encoding

> We chose this function because we hypothesized it would allow the model to
easily learn to attend by relative positions, since for any fixed offset
$k$, $PE_{pos + k}$ can be represented as a linear function of $PE_{pos}$.

"Attention Is All You Need" uses $sin$ and $cos$ to encode positions.

But there's another interesting way to do so as well.

Second thought — decided to make an exclusive page for positional encoding only.
