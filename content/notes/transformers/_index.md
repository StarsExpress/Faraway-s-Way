---
title: Transformers
weight: 2
math: true
sidebar:
  hide: true
cover: "/notes-cover/transformers.png"
subtitle: "Hood Under Attention"
tags: ["Gen AI", "LLMs", "Math"]
---


## 1. Attention

Some feelings about how can these attention-powered transformers work so well in Gen AI:

Natural language, or any sequence data, is literally like a more advanced Solitaire game.

Each position can — and should — look way beyond just the previous position.

### 🧱Encoder Diagram

Encoder reads the whole source at once, __without any mask__. Then it hands its output over as $K$ & $V$ for decoder to reference.

<div class="dg-fig">
<svg viewBox="0 0 640 460" role="img" aria-label="Encoder block from bottom to top: multi-head self-attention without a mask, add and norm, feed-forward, add and norm, repeated N times. The encoder output becomes K and V for cross-attention in every decoder block.">
<defs><marker id="dg-arrow-e" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="currentColor" stroke-opacity=".7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker></defs>
<rect class="dg-kv" x="110" y="20" width="280" height="48" rx="6"/>
<text class="dg-t" x="250" y="37">Encoder output</text>
<text class="dg-s" x="250" y="54">K, V for cross-attention</text>
<line class="dg-line" x1="392" y1="44" x2="458" y2="44" marker-end="url(#dg-arrow-e)"/>
<text class="dg-s dg-l" x="466" y="36">To cross-attention of</text>
<text class="dg-s dg-l" x="466" y="52">every decoder block</text>
<line class="dg-line" x1="250" y1="130" x2="250" y2="70" marker-end="url(#dg-arrow-e)"/>
<rect class="dg-cont" x="70" y="96" width="360" height="246" rx="12"/>
<text class="dg-t dg-l" x="88" y="116">Encoder block × N</text>
<rect class="dg-box" x="110" y="130" width="280" height="26" rx="6"/>
<text x="250" y="143">Add &amp; Norm</text>
<line class="dg-line" x1="250" y1="174" x2="250" y2="158" marker-end="url(#dg-arrow-e)"/>
<rect class="dg-box" x="110" y="174" width="280" height="40" rx="6"/>
<text class="dg-t" x="250" y="194">Feed-forward</text>
<line class="dg-line" x1="250" y1="232" x2="250" y2="216" marker-end="url(#dg-arrow-e)"/>
<rect class="dg-box" x="110" y="232" width="280" height="26" rx="6"/>
<text x="250" y="245">Add &amp; Norm</text>
<line class="dg-line" x1="250" y1="276" x2="250" y2="260" marker-end="url(#dg-arrow-e)"/>
<rect class="dg-attn" x="110" y="276" width="280" height="48" rx="6"/>
<text class="dg-t" x="250" y="293">Multi-head self-attention</text>
<text class="dg-s" x="250" y="310">Q, K, V: all from input · no mask</text>
<line class="dg-line" x1="250" y1="364" x2="250" y2="326" marker-end="url(#dg-arrow-e)"/>
<rect class="dg-box" x="110" y="364" width="280" height="34" rx="6"/>
<text class="dg-t" x="250" y="381">Embedding + positional encoding</text>
<line class="dg-line" x1="250" y1="426" x2="250" y2="400" marker-end="url(#dg-arrow-e)"/>
<text class="dg-s" x="250" y="438">Input tokens, all at once</text>
<text class="dg-s dg-l" x="460" y="186">Runs once, all</text>
<text class="dg-s dg-l" x="460" y="202">positions in parallel</text>
<text class="dg-s dg-l" x="460" y="292">No mask: each token</text>
<text class="dg-s dg-l" x="460" y="308">sees every position</text>
</svg>
</div>

### 3️⃣ Roles Inside Context

To better look deeper beneath context, each word is treated in 3 ways:

- Query $Q$: which topic is searched for.
- Key $K$: how each candidate is labeled.
- Value $V$: what content each candidate holds.

So — if each $K$ pairs with a $V$, then how about we use $Q$ & $K$
matching results to decide how much each $V$ plays?

### Stage ⓵: Scaled Dot-Product

- $Q = R^{n \times d_k}, K = R^{n \times d_k}, V = R^{n \times d_v}.$

- $\text{Attention}(Q, K, V) = \text{Softmax} (\frac{ QK^T }{ \sqrt{d_k} }) V$.

- $(QK^T)_{i, j} = Q_{i, :} \cdot (k_{j, :})^T$.

#### 🤿Mask If Required

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

#### 🗞️__Dot product__ Comes

- $P = \text{Softmax}(\frac{ QK^T }{ \sqrt{d_k} }) = R^{n \times n}$: with $\sum_{j = 1}^n P_{i, j} = 1$, $P_{i, j}$ is $i^{th}$ token's __relative proportion of attention__ to $j^{th}$ token.

- Be sure to set `axis=-1` when doing Softmax. Along columns 😉😏

- $V = R^{n \times d_v}:$ each $V_{i, :} = R^{d_v}$ means $i^{th}$ token's value representation.

- $PV$ yields a matrix of $R^{n \times d_v}$,
where each $i^{th}$ row vector is a **weighted sum of token vectors.**

- Weights are based on **$i^{th}$ token's attention to all $n$ tokens.**

#### 🧮More Softmax Details: [Visit](/notes/softmax_saturation/)

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

<style>
.dg-fig { max-width: 640px; margin: 1.5rem auto; }
.dg-fig svg { width: 100%; height: auto; display: block; }
.dg-fig .dg-box { fill: rgba(127,127,127,.10); stroke: currentColor; stroke-opacity: .45; stroke-width: 1; }
.dg-fig .dg-attn { fill: rgba(139,92,246,.16); stroke: #8b5cf6; stroke-width: 1.2; }
.dg-fig .dg-kv { fill: rgba(20,184,166,.16); stroke: #14b8a6; stroke-width: 1.2; }
.dg-fig .dg-opt { stroke-dasharray: 5 4; }
.dg-fig .dg-cont { fill: none; stroke: currentColor; stroke-opacity: .35; stroke-dasharray: 5 4; }
.dg-fig .dg-line { stroke: currentColor; stroke-opacity: .55; stroke-width: 1.4; fill: none; }
.dg-fig .dg-leader { stroke: #14b8a6; stroke-width: 1.2; stroke-dasharray: 3 3; fill: none; }
.dg-fig text { fill: currentColor; font-size: 12px; text-anchor: middle; dominant-baseline: central; }
.dg-fig .dg-t { font-size: 14px; font-weight: 500; }
.dg-fig .dg-s { opacity: .7; }
.dg-fig .dg-l { text-anchor: start; }
</style>


### 🧱Decoder Diagram

Decoder writes one token at a time:
- Dashed parts — cross attention related things — exist only in encoder-decoder models.
- Decoder-only LLMs keep just masked self-attention.

Teal boxes mark where $K$ & $V$ come from or get stored.

<div class="dg-fig">
<svg viewBox="0 0 640 576" role="img" aria-label="Decoder block from bottom to top: masked self-attention, add and norm, cross-attention, add and norm, feed-forward, add and norm. Cross-attention takes K and V from the encoder stack output. Masked self-attention keys and values are stored in the KV cache.">
<defs><marker id="dg-arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="currentColor" stroke-opacity=".7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker></defs>
<text class="dg-s" x="250" y="14">Next-token probabilities</text>
<line class="dg-line" x1="250" y1="48" x2="250" y2="24" marker-end="url(#dg-arrow)"/>
<rect class="dg-box" x="110" y="48" width="280" height="34" rx="6"/>
<text class="dg-t" x="250" y="65">Linear + softmax</text>
<line class="dg-line" x1="250" y1="138" x2="250" y2="84" marker-end="url(#dg-arrow)"/>
<rect class="dg-cont" x="70" y="104" width="360" height="356" rx="12"/>
<text class="dg-t dg-l" x="88" y="124">Decoder block × N</text>
<rect class="dg-box" x="110" y="138" width="280" height="26" rx="6"/>
<text x="250" y="151">Add &amp; Norm</text>
<line class="dg-line" x1="250" y1="182" x2="250" y2="166" marker-end="url(#dg-arrow)"/>
<rect class="dg-box" x="110" y="182" width="280" height="40" rx="6"/>
<text class="dg-t" x="250" y="202">Feed-forward</text>
<line class="dg-line" x1="250" y1="240" x2="250" y2="224" marker-end="url(#dg-arrow)"/>
<rect class="dg-box dg-opt" x="110" y="240" width="280" height="26" rx="6"/>
<text x="250" y="253">Add &amp; Norm</text>
<line class="dg-line" x1="250" y1="284" x2="250" y2="268" marker-end="url(#dg-arrow)"/>
<rect class="dg-attn dg-opt" x="110" y="284" width="280" height="48" rx="6"/>
<text class="dg-t" x="250" y="301">Cross-attention</text>
<text class="dg-s" x="250" y="318">Q: decoder · K, V: encoder</text>
<line class="dg-line" x1="250" y1="350" x2="250" y2="334" marker-end="url(#dg-arrow)"/>
<rect class="dg-box" x="110" y="350" width="280" height="26" rx="6"/>
<text x="250" y="363">Add &amp; Norm</text>
<line class="dg-line" x1="250" y1="394" x2="250" y2="378" marker-end="url(#dg-arrow)"/>
<rect class="dg-attn" x="110" y="394" width="280" height="48" rx="6"/>
<text class="dg-t" x="250" y="411">Masked self-attention</text>
<text class="dg-s" x="250" y="428">Q, K, V: all from output so far</text>
<line class="dg-line" x1="250" y1="482" x2="250" y2="444" marker-end="url(#dg-arrow)"/>
<rect class="dg-box" x="110" y="482" width="280" height="34" rx="6"/>
<text class="dg-t" x="250" y="499">Embedding + positional encoding</text>
<line class="dg-line" x1="250" y1="544" x2="250" y2="520" marker-end="url(#dg-arrow)"/>
<text class="dg-s" x="250" y="556">Output tokens generated so far</text>
<rect class="dg-kv" x="460" y="284" width="160" height="48" rx="6"/>
<text class="dg-t" x="540" y="301">Encoder output</text>
<text class="dg-s" x="540" y="318">K, V computed once</text>
<line class="dg-line" x1="458" y1="308" x2="392" y2="308" marker-end="url(#dg-arrow)"/>
<text class="dg-s" x="410" y="296">K, V</text>
<text class="dg-s dg-l" x="460" y="352">Dashed: dropped in</text>
<text class="dg-s dg-l" x="460" y="368">decoder-only LLMs</text>
<rect class="dg-kv" x="460" y="394" width="160" height="48" rx="6"/>
<text class="dg-t" x="540" y="411">KV cache</text>
<text class="dg-s" x="540" y="428">grows each step</text>
<line class="dg-leader" x1="458" y1="418" x2="392" y2="418"/>
</svg>
</div>

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

This brings up a quite fundamental topic indeed: KV Cache.

It's mentioned in decoder diagram above. For details, please see: [KV Cache](/notes/kv_cache/).


## 4. Positional Encoding

> We chose this function because we hypothesized it would allow the model to
easily learn to attend by relative positions, since for any fixed offset
$k$, $PE_{pos + k}$ can be represented as a linear function of $PE_{pos}$.

"Attention Is All You Need" uses $sin$ and $cos$ to encode positions.

But there's another interesting way to do so as well.

Second thought — decided to make an exclusive page for positional encoding only.
