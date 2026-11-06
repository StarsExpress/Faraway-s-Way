---
title: KV Cache
weight: 5
math: true
sidebar:
  hide: true
cover: "/notes-cover/kv_cache.png"
subtitle: "Cache Flow Statement"
tags: ["Gen AI", "LLMs", "Latency"]
---


## 1. KV Cache

### 🎞️One Decode Step

At each step, only the newest token gets computed:

- Its $K$ & $V$ get appended to the cache.
- Its $Q$ looks at all cached $K$ & $V$, then is dropped — later tokens never need an old $Q$.
- Old $K$ & $V$ never change, since masked attention only lets a token see the past.

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

<div class="dg-fig">
<svg viewBox="0 0 640 440" role="img" aria-label="One decode step in one layer. The KV cache already holds K and V of tokens t1 to t5. The new token t6 produces q6, k6 and v6. Its k6 and v6 are appended to the cache. Its q6 attends over K1 to K6 and V1 to V6 and is never stored.">
<defs><marker id="dg-arrow-kv" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="currentColor" stroke-opacity=".7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker></defs>
<text class="dg-s" x="320" y="14">One decode step, one layer shown</text>
<rect class="dg-cont" x="20" y="34" width="600" height="104" rx="12"/>
<text class="dg-t dg-l" x="38" y="54">KV cache of this layer</text>
<rect class="dg-kv" x="36" y="68" width="86" height="56" rx="6"/>
<text class="dg-t" x="79" y="87">t1</text>
<text class="dg-s" x="79" y="107">K1, V1</text>
<rect class="dg-kv" x="130" y="68" width="86" height="56" rx="6"/>
<text class="dg-t" x="173" y="87">t2</text>
<text class="dg-s" x="173" y="107">K2, V2</text>
<rect class="dg-kv" x="224" y="68" width="86" height="56" rx="6"/>
<text class="dg-t" x="267" y="87">t3</text>
<text class="dg-s" x="267" y="107">K3, V3</text>
<rect class="dg-kv" x="318" y="68" width="86" height="56" rx="6"/>
<text class="dg-t" x="361" y="87">t4</text>
<text class="dg-s" x="361" y="107">K4, V4</text>
<rect class="dg-kv" x="412" y="68" width="86" height="56" rx="6"/>
<text class="dg-t" x="455" y="87">t5</text>
<text class="dg-s" x="455" y="107">K5, V5</text>
<rect class="dg-kv dg-opt" x="506" y="68" width="86" height="56" rx="6"/>
<text class="dg-t" x="549" y="87">t6</text>
<text class="dg-s" x="549" y="107">K6, V6</text>
<line class="dg-line" x1="530" y1="188" x2="546" y2="140" marker-end="url(#dg-arrow-kv)"/>
<text class="dg-s dg-l" x="560" y="168">append</text>
<rect class="dg-box" x="420" y="190" width="170" height="44" rx="6"/>
<text class="dg-t" x="505" y="206">New token t6</text>
<text class="dg-s" x="505" y="223">only this one is computed</text>
<line class="dg-line" x1="490" y1="236" x2="412" y2="260" marker-end="url(#dg-arrow-kv)"/>
<line class="dg-line" x1="505" y1="236" x2="490" y2="260" marker-end="url(#dg-arrow-kv)"/>
<line class="dg-line" x1="520" y1="236" x2="568" y2="260" marker-end="url(#dg-arrow-kv)"/>
<rect class="dg-attn" x="380" y="262" width="64" height="34" rx="6"/>
<text class="dg-t" x="412" y="279">q6</text>
<rect class="dg-kv" x="458" y="262" width="64" height="34" rx="6"/>
<text class="dg-t" x="490" y="279">k6</text>
<rect class="dg-kv" x="536" y="262" width="64" height="34" rx="6"/>
<text class="dg-t" x="568" y="279">v6</text>
<text class="dg-s" x="505" y="318">q6 is used once</text>
<text class="dg-s" x="505" y="334">and never cached</text>
<path class="dg-line" d="M412 298 L412 368 L398 368" marker-end="url(#dg-arrow-kv)"/>
<line class="dg-line" x1="200" y1="140" x2="200" y2="340" marker-end="url(#dg-arrow-kv)"/>
<text class="dg-s dg-l" x="210" y="222">read K1 to K5 and</text>
<text class="dg-s dg-l" x="210" y="238">V1 to V5 from cache</text>
<rect class="dg-attn" x="36" y="342" width="362" height="54" rx="6"/>
<text class="dg-t" x="217" y="360">Attention for t6</text>
<text class="dg-s" x="217" y="380">q6 vs K1 to K6, then weighted sum of V1 to V6</text>
<line class="dg-line" x1="217" y1="398" x2="217" y2="420" marker-end="url(#dg-arrow-kv)"/>
<text class="dg-s" x="217" y="432">Output of t6, feeds next layer</text>
</svg>
</div>

### 🗜️Cache Size Formula

Actually, it is very clearly top-down layered:

- 2 Lines: K & V each.
- Layers per line.
- KV heads per layer.
- Dimensions per head.
- Bits per dimension, and 1 byte = 8 bits.

So cache size in bytes: Lines x Layers x Heads x Dimensions x Bits / 8.

Quite often we see terms like **FP16**, **INT8**, etc.

__Terms' numbers = how many bits needed for each value stored.__

### KV Cache's Roommate 🛌

Not all VRAM can be used by KV cache. In fact, somebody eats up lots of VRAM before KV cache has its share:

**Pre-trained model weights.**

We can know how much room is left for KV cache __only after deducting__ size of pre-trained model weights.

### 🔄Concurrent Conversations

Suppose we have an initial VRAM of $V$ GB. Pre-trained model weights eat away $W$ GB.

Now we have $V - W$ GB saved for KV cache. Let's assume our decoder has:
- $L$ layers.
- $H_{kv}$ KV heads per layer — query heads don't count. With GQA/MQA, $H_{kv} <<<$ number of query heads.
- $D$ dimensions per head.
- Precision of $P$.

Then each decoded token requires $C = \frac{2 \times L \times H_{kv} \times D \times P}{8} = \frac{L \times H_{kv} \times D \times P}{4}$ bytes of KV cache.

With each conversation reaching a maximum text length of $T$ tokens, each conversation takes away $CT$ bytes of KV cache.

**Thus, we can parallelize $\lfloor \underbrace{10^9}_{\text{1 GB} = 10^9 \text{ bytes}} \times \frac{V - W}{CT} \rfloor$ conversations at the same time.**

We must take floor since a conversation either fits entirely or doesn't — it has no fraction.

Note "at the same time" means these conversations' KV cache __stays resident__ in VRAM, not that all of them are computed in the same step.

Reminders:
- $V - W$ isn't all usable: activations & CUDA graphs take some too.
- $T$ is max length. Real conversations are often shorter, so more can fit in practice.

## 2. Prefix Cache

KV cache lives inside one request. Prefix cache asks: when a request ends, why throw its $K$ & $V$ away?

Keep them, indexed by the tokens they came from. Next request starting with same tokens skips prefill for those:
- **KV cache — within one request:** each decoded token skips recomputing earlier ones.
- **Prefix cache — across requests:** skip prefill for prompt tokens already seen.

### 🧬What Counts as a Prefix?

Leading tokens, same order from the very first token. No gaps allowed:

- A token's $K$ & $V$ depend on __every token before it, and on its position.__

- Same word after a different history means different $K$ & $V$.

- One different token causes everything after it to differ. Like a Trie.

In chat, current turn's prefix is the previous turn's finalized system prompt, user message and reply:

<div class="dg-fig">
<svg viewBox="0 0 640 372" role="img" aria-label="Two turns of a conversation, 18 tokens in total. Turn 1 prefills tokens 1 to 8 and decodes tokens 9 to 12. Turn 2 reuses tokens 1 to 12 from the prefix cache, prefills new tokens 13 to 15, then decodes tokens 16 to 18. Each decoded token appends to the KV cache.">
<defs><marker id="dg-arrow-pc" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="currentColor" stroke-opacity=".7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker></defs>
<text class="dg-t dg-l" x="24" y="20">Turn 1: cold start</text>
<rect class="dg-box" x="24" y="32" width="30" height="34" rx="5"/>
<text x="39.0" y="49">1</text>
<rect class="dg-box" x="57" y="32" width="30" height="34" rx="5"/>
<text x="72.0" y="49">2</text>
<rect class="dg-box" x="90" y="32" width="30" height="34" rx="5"/>
<text x="105.0" y="49">3</text>
<rect class="dg-box" x="123" y="32" width="30" height="34" rx="5"/>
<text x="138.0" y="49">4</text>
<rect class="dg-box" x="156" y="32" width="30" height="34" rx="5"/>
<text x="171.0" y="49">5</text>
<rect class="dg-box" x="189" y="32" width="30" height="34" rx="5"/>
<text x="204.0" y="49">6</text>
<rect class="dg-box" x="222" y="32" width="30" height="34" rx="5"/>
<text x="237.0" y="49">7</text>
<rect class="dg-box" x="255" y="32" width="30" height="34" rx="5"/>
<text x="270.0" y="49">8</text>
<rect class="dg-attn" x="288" y="32" width="30" height="34" rx="5"/>
<text x="303.0" y="49">9</text>
<rect class="dg-attn" x="321" y="32" width="30" height="34" rx="5"/>
<text x="336.0" y="49">10</text>
<rect class="dg-attn" x="354" y="32" width="30" height="34" rx="5"/>
<text x="369.0" y="49">11</text>
<rect class="dg-attn" x="387" y="32" width="30" height="34" rx="5"/>
<text x="402.0" y="49">12</text>
<text class="dg-s" x="154.5" y="84">system + user: prefill</text>
<text class="dg-s" x="352.5" y="84">reply: decode</text>
<line class="dg-line" x1="220.5" y1="98" x2="220.5" y2="156" marker-end="url(#dg-arrow-pc)"/>
<text class="dg-s dg-l" x="234.5" y="118">Prefix cache, across requests:</text>
<text class="dg-s dg-l" x="234.5" y="136">turn 2 starts with the same 12 tokens</text>
<text class="dg-t dg-l" x="24" y="176">Turn 2: prefix hit</text>
<rect class="dg-kv" x="24" y="188" width="30" height="34" rx="5"/>
<text x="39.0" y="205">1</text>
<rect class="dg-kv" x="57" y="188" width="30" height="34" rx="5"/>
<text x="72.0" y="205">2</text>
<rect class="dg-kv" x="90" y="188" width="30" height="34" rx="5"/>
<text x="105.0" y="205">3</text>
<rect class="dg-kv" x="123" y="188" width="30" height="34" rx="5"/>
<text x="138.0" y="205">4</text>
<rect class="dg-kv" x="156" y="188" width="30" height="34" rx="5"/>
<text x="171.0" y="205">5</text>
<rect class="dg-kv" x="189" y="188" width="30" height="34" rx="5"/>
<text x="204.0" y="205">6</text>
<rect class="dg-kv" x="222" y="188" width="30" height="34" rx="5"/>
<text x="237.0" y="205">7</text>
<rect class="dg-kv" x="255" y="188" width="30" height="34" rx="5"/>
<text x="270.0" y="205">8</text>
<rect class="dg-kv" x="288" y="188" width="30" height="34" rx="5"/>
<text x="303.0" y="205">9</text>
<rect class="dg-kv" x="321" y="188" width="30" height="34" rx="5"/>
<text x="336.0" y="205">10</text>
<rect class="dg-kv" x="354" y="188" width="30" height="34" rx="5"/>
<text x="369.0" y="205">11</text>
<rect class="dg-kv" x="387" y="188" width="30" height="34" rx="5"/>
<text x="402.0" y="205">12</text>
<rect class="dg-box" x="420" y="188" width="30" height="34" rx="5"/>
<text x="435.0" y="205">13</text>
<rect class="dg-box" x="453" y="188" width="30" height="34" rx="5"/>
<text x="468.0" y="205">14</text>
<rect class="dg-box" x="486" y="188" width="30" height="34" rx="5"/>
<text x="501.0" y="205">15</text>
<rect class="dg-attn" x="519" y="188" width="30" height="34" rx="5"/>
<text x="534.0" y="205">16</text>
<rect class="dg-attn" x="552" y="188" width="30" height="34" rx="5"/>
<text x="567.0" y="205">17</text>
<rect class="dg-attn" x="585" y="188" width="30" height="34" rx="5"/>
<text x="600.0" y="205">18</text>
<text class="dg-s" x="220.5" y="240">tokens 1 to 12: KV read from cache, no prefill</text>
<text class="dg-s" x="468.0" y="240">new user</text>
<text class="dg-s" x="567.0" y="240">decode</text>
<text class="dg-s" x="468.0" y="256">prefill</text>
<text class="dg-s" x="320" y="296">KV cache, within one request: every decoded token (9 to 12, then 16 to 18)</text>
<text class="dg-s" x="320" y="312">appends its own K and V, so no earlier token is ever recomputed.</text>
<rect class="dg-box" x="24" y="338" width="16" height="16" rx="3"/>
<text class="dg-s dg-l" x="48" y="346">computed in prefill</text>
<rect class="dg-attn" x="204" y="338" width="16" height="16" rx="3"/>
<text class="dg-s dg-l" x="228" y="346">generated by decode</text>
<rect class="dg-kv" x="404" y="338" width="16" height="16" rx="3"/>
<text class="dg-s dg-l" x="428" y="346">KV served from cache</text>
</svg>
</div>

Real systems cache in fixed-size blocks, so only full blocks of the shared prefix get hit.

### 🌳One Trunk & Many Branches

Many users share the same system prompt, tools or documents. That trunk's KV is computed once; every request just points at it.

<div class="dg-fig">
<svg viewBox="0 0 640 290" role="img" aria-label="Two requests share one trunk. The system prompt and tools have their KV computed once and both requests point at the same cached blocks. From the first differing token on, each request has its own branch with its own user message and reply.">
<defs><marker id="dg-arrow-tr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="currentColor" stroke-opacity=".7" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker></defs>
<rect class="dg-kv" x="190" y="20" width="260" height="48" rx="6"/>
<text class="dg-t" x="320" y="37">System prompt + tools</text>
<text class="dg-s" x="320" y="54">KV computed once, cached</text>
<text class="dg-s dg-l" x="466" y="36">Both requests point at</text>
<text class="dg-s dg-l" x="466" y="52">the same KV blocks</text>
<line class="dg-line" x1="270" y1="70" x2="170" y2="118" marker-end="url(#dg-arrow-tr)"/>
<line class="dg-line" x1="370" y1="70" x2="470" y2="118" marker-end="url(#dg-arrow-tr)"/>
<text class="dg-s" x="320" y="104">First differing token: branches apart</text>
<rect class="dg-box" x="60" y="120" width="220" height="44" rx="6"/>
<text class="dg-t" x="170" y="136">User A: question</text>
<text class="dg-s" x="170" y="153">prefill, own blocks</text>
<rect class="dg-box" x="360" y="120" width="220" height="44" rx="6"/>
<text class="dg-t" x="470" y="136">User B: question</text>
<text class="dg-s" x="470" y="153">prefill, own blocks</text>
<line class="dg-line" x1="170" y1="166" x2="170" y2="198" marker-end="url(#dg-arrow-tr)"/>
<line class="dg-line" x1="470" y1="166" x2="470" y2="198" marker-end="url(#dg-arrow-tr)"/>
<rect class="dg-attn" x="60" y="200" width="220" height="44" rx="6"/>
<text class="dg-t" x="170" y="216">Reply A</text>
<text class="dg-s" x="170" y="233">decode, own blocks</text>
<rect class="dg-attn" x="360" y="200" width="220" height="44" rx="6"/>
<text class="dg-t" x="470" y="216">Reply B</text>
<text class="dg-s" x="470" y="233">decode, own blocks</text>
<text class="dg-s" x="320" y="274">Same token after a different history has different KV, so nothing is shared past the fork.</text>
</svg>
</div>

### 🏏How Hits Are Found

- **vLLM:** KV lives in fixed-size blocks. Each full block is hashed together with the hash of the block before it, so a block's identity = its own tokens + everything before. Lookup walks blocks from the front until the first miss.
- **SGLang:** a radix tree over token sequences — longest matching path is the hit.
- **Both:** blocks being used carry a reference count. Unused ones linger until memory is needed, then the least recently used go first.
- Only full blocks count. A partial tail block gets recomputed.

### 🎯What It Saves

- Prefill for every hit token — lower TTFT: Time to First Token; less compute.
- VRAM: shared blocks are stored once, leaving more room for concurrent conversations.
- Not decode. Each new token still reads the whole KV cache, so that part stays memory-bound.

### ✍️Writing Prompts That Hit

- Place stable ones first: system prompt, tool definitions, few-shot examples, long documents.
- Then concat varying ones last: new user message.
- One changed token early — say, a timestamp on first line — and everything after it misses.
- Keep history append-only. Edit, truncate or reorder earlier turns, and prefix breaks right there.
- Routine agents are the best case: big fixed head, small changing tail.
