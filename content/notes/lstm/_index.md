---
title: LSTM — Long Short-Term Memory
weight: 2
math: true
sidebar:
  hide: true
---


## 1. 🎯Objective: Live Up To The Name

When learning LSTM, I once wondered why we have so many gates, and how to interpret the so-called "cell candidate".

Then one day, my mind realized how to interpret many of LSTM designs.

For every sequential step $t$, we have to track two terms of memories:

- Long — denoted as $c_t$. __Cell state.__
- Short — denoted as $h_t$. __Hidden state.__

All the following designs are centered at such an objective.


## 2. 🚪Gates

- $i_t = \sigma(x_t \cdot W_x^{(i)} + h_{t - 1} \cdot W_h^{(i)} + b^{(i)})$

- $f_t = \sigma(x_t \cdot W_x^{(f)} + h_{t - 1} \cdot W_h^{(f)} + b^{(f)})$

- $o_t = \sigma(x_t \cdot W_x^{(o)} + h_{t - 1} \cdot W_h^{(o)} + b^{(o)})$

Gates, according to word definition — represent the __proportion of openness.__

Proportion is of course between 0 and 1. Sigmoid function has a range of $[0, 1]$.
That's why 3 gates all have Sigmoid as the final stage.

Let's see what do these gates work on.


## 3. 🅾️ Output Gate — Short Memory

### 🌳 Short Inevitably Becomes Long
Current step's short memory $h_t$ can also be long memory — __if you are standing at future steps__.

In other words, $h_t$ should directly or indirectly influence $c_{t + k}$ for $k \ge 1$.

Thus, $h_t$ is an intermediate **output** for the development of $c_{t + k}$.

### But Where Does $h_t$ Come From 🤔

Why not let current step's long memory $c_t$ deduce $h_t$ 😉

Of course, in case $c_t$ goes too extreme, we better normalize it into a smaller range.

$[-1, 1]$ is a quite small range. What function naturally has this range? Tanh.

However, __Tanh only does normalization. It tells nothing about extent.__

After all, we need **extent** to know how much should $c_t$ impact $h_t$.

### ⓢ Sigmoid Is Here Again

Speaking of extent, its idea is quite like that of proportion — represented by Sigmoid.

This is how we end up with $h_t = o_t \odot \text{Tanh}(c_t)$, as $o_t$ contains Sigmoid.


## 4. 🎡 Forget & Input Gates — Long Memory

### How To Build Up $c_t$ ⁇

$c_t$ is current step's long memory, so when establishing $c_t$, we might wanna ask:

What kind of things might be **long enough** for current step $t$?

- $c_{t - 1}$ is already considered long w.r.t. step $t - 1$,
  so it definitely is long for step $t$. __Memories can't grow younger.__

- But......$h_{t - 1}$ should have a chance, too. It is **short at previous step $t - 1$.**
  However, __short grows older into long.__

### 🪘Ingredients Set

OK, so now we can mainly classify $c_t$'s ingredients into two contents:

- Absolutely long — $c_{t - 1}$.

- Relatively young among the long — $h_{t - 1}$. It just got promoted into long.

But there is one more content to explain.

### 🌊 $x_t$ Shows Up As Content

Here comes a tricky part: for LSTM to learn from each step's input $x_t$,
we must let $x_t$ participate in calculations of long & short memories.

With $h_t = o_t \odot \text{Tanh}(c_t)$, $x_t$ can indirectly contribute to $h_t$ by being part of $c_t$ calculation.
So the $3^{rd}$ content is $x_t$.

Because it and $h_{t - 1}$ are much younger than $c_{t - 1}$,
we combine $x_t$ and $h_{t - 1}$ into __cell candidate, denoted as $\tilde{c_t}$:__

$\tilde{c_t} = \text{Tanh}(x_t \cdot W_x^{(c)} + h_{t - 1} \cdot W_h^{(c)} + b^{(c)})$

Why called "candidate"? **$\tilde{c_t}$ is a candidate to compete/collaborate** with $c_{t - 1}$ for determining $c_t$.

### 📊Learnable Weighted Combinations

Should $\tilde{c_t}$ and $c_{t - 1}$ compete or collaborate with each other for determining $c_t$?

This question is ought to be learnable. Naivest way is to consider 4 most extreme scenarios:

| Cases | Discard $\tilde{c_t}$ | Trust $\tilde{c_t}$ 100% |
|--------|-------------|----------|
| Discard $c_{t - 1}$ | **Reset**. | $\tilde{c_t}$ wins competition. |
| Trust $c_{t - 1}$ 100% |  $c_{t - 1}$ wins competition. | **Collaboration**. |

- Reset — $c_t$ believes neither $\tilde{c_t}$ nor $c_{t - 1}$ is helpful.

- Collaboration — $c_t$ believes both $\tilde{c_t}$ and $c_{t - 1}$ are 100% helpful.

- __Correct～this is not zero-sum game. Remember, both sides can contribute equally.__

Not hard to tell that, we should allow learning anywhere between reset and collaboration.

### ✚Back To Proportion Managements — Sigmoid

Now we can write $c_t = f_t \odot c_{t - 1} + i_t \odot \tilde{c_t}$.

$f_t$ and $i_t$ are gates for two sides, respectively.

Let's talk about the gate for $c_{t - 1}$ first:

- Notice that it is called "forget",
because we want to know how much to forget about long memory at previous step $t - 1$.

- Although such a naming is quite......counter-intuitive. __Higher $f_t$ means less forgetting__, according to $c_t$ formula.

- I would have called it the **"keep" gate 😏**

Turn to the gate for $\tilde{c_t}$. It is named "input":

- Since $\tilde{c_t}$ has the __input $x_t$__ at current step $t$.

- Also, $\tilde{c_t}$ contains $h_{t - 1}$, which is previous step $t - 1$ short memory.

- ☝️That **just promotes to an input for long memory.**


## 5. 🎬 Into Production

As of now, it's not hard to tell that, if we have current step's value/label $y_t$,

then we can just use $h_t$ to make a prediction called $\hat{y_t}$:

$\hat{y_t} = \text{f}(h_t \cdot W_h^{(y)} + b_h^{(y)})$ where $\text{f}$ is an activation function.

Some LSTM applications:

- Time series: $y_t$ is each timestep's value or label. Can be regression or classification.

- Word/sentence generation: $y_t$ is each slot's token. A token can represent a character, or even a word.

For word/sentence generation, it's required to left-shift input,
since only $\{ y_{t - k} \; \forall \; 1 \leq k \leq t \}$ can be part of $x_t$ to determine $y_t$.

**Rule of thumb: ensure everything inside $x_t$ happens earlier than $y_t$.**
