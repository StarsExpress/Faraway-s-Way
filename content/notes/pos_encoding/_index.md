---
title: Positional Encoding
weight: 7
math: true
sidebar:
  hide: true
subtitle: "Sin & Cos & i everywhere"
tags: ["Gen AI", "LLMs", "Math"]
---


## Positional Encoding

### Sine Linear Combination

- $sin(x + y) = sin(x)cos(y) + cos(x)sin(y).$

- $sin(x - y) = sin(x)cos(y) - cos(x)sin(y).$

- ⬆️So: $sin(x + y) - sin(x - y) = 2cos(x)sin(y).$

Let $a = x + y, b = x - y.$ We have derived:

$sin(a) - sin(b) = sin(x + y) - sin(x - y) = 2cos(x)sin(y) = 2cos(\frac{a + b}{2})sin(\frac{a - b}{2}).$

### Cosine Linear Combination

- $cos(x + y) = cos(x)cos(y) - sin(x)sin(y).$

- $cos(x - y) = cos(x)cos(y) + sin(x)sin(y).$

- ⬆️So: $cos(x - y) - cos(x + y) = 2sin(x)sin(y).$

Still, let $a = x + y, b = x - y.$ We can easily get:

$cos(b) - cos(a) = cos(x - y) - cos(x + y) = 2sin(x)sin(y) = 2sin(\frac{a + b}{2})sin(\frac{a - b}{2}).$
