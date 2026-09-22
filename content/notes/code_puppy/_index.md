---
title: Code Puppy
weight: 1
toc: true
sidebar:
  hide: true
cover: "/notes-cover/code_puppy.png"
subtitle: "Internship➡️Agentic DevOps"
tags: ["Agentic AI", "OSS", "CI/CD"]
---


## 🌻 Summer 2026 Internship Time
**[Code Puppy](https://www.businessinsider.com/walmart-code-puppy-ai-anthropic-claude-code-openai-codex-2026-6) accompanied me through my camera metadata system project.**

[Its creator Michael Pfaffenberger left 11 days before my last day](https://www.businessinsider.com/walmarts-ai-coding-tool-code-puppy-creator-leaving-pydantic-2026-8).

The only Code Puppy team intern during that summer is my friend.

He told me he worked for two Code Puppy products: Walmart internal, and — [open source](https://code-puppy.dev).


## 🙏️ Memories Into Gratitude
Staring at [Code Puppy open source repo](https://github.com/mpfaffenberger/code_puppy) and finding my friend's work, I felt like:

"Let's help address some issues here as a way to show gratefulness."

[**And within two months, things showed up quickly.**](https://github.com/mpfaffenberger/code_puppy/pulls?q=is:pr+author:StarsExpress+is:merged)


## I Drill Deeper Than Issues 🪏
(1). Improved Agent—UI communications: [**reduce background CPU wake-ups by 88.5%**](https://github.com/mpfaffenberger/code_puppy/pull/859) via exponential backoff.

(2). Corrected several attributes and behaviors from **Gemini Code Assist**:
- [Restore silently-ignored model settings by users. Simplified thinking config handling, too.](https://github.com/mpfaffenberger/code_puppy/pull/948)
- [Add back dropped thinking parts during API responses.](https://github.com/mpfaffenberger/code_puppy/pull/934)
- [Secure complete JSON schema sanitization.](https://github.com/mpfaffenberger/code_puppy/pull/933)

(3). [Ensured **tools'** file-read process sanitize surrogates in the right way.](https://github.com/mpfaffenberger/code_puppy/pull/756)

(4). Inside big Agentic AI codebase, when aligning inconsistency across 20+ parser calls, [I'd like to:](https://github.com/mpfaffenberger/code_puppy/pull/808)
- **Split truthy & falsy cases into dedicated parsers**, although original issue suggested a shared parser.
- Enforce required function booleans for clearness with type safety.

(5). At times, [triage bot may mis-classify](https://github.com/mpfaffenberger/code_puppy/pull/711), **so I make corrections for it.**


## Keep Working 🍶

Seeing Code Puppy use Pydantic's AI core framework, [I sometimes contribute to Pydantic AI repo.](https://github.com/pydantic/pydantic-ai/pulls?q=is:pr+author:StarsExpress+is:merged)
