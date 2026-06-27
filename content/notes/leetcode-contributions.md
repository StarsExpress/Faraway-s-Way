---
title: Open Source Contributions
weight: 3
toc: true
sidebar:
  hide: true
---

## Context

[doocs/leetcode](https://github.com/doocs/leetcode) is one of the most widely referenced open-source LeetCode repositories on GitHub — 36K+ stars, maintained with strict review standards. Getting a PR merged there isn't just about correctness; reviewers check time/space complexity, code style, and whether the solution is genuinely optimal. A merge is a third-party signal that the solution holds up.

## Merged PRs: doocs/leetcode

8 merged PRs, all Hard-difficulty problems, all with proven-optimal complexity.

### LC 2454 — Find the Nearest Fair Integer (Dual-Stack O(n))

The mainstream approach to this problem runs in O(n log n). I independently derived a dual-stack solution that brings it down to **O(n) time, O(n) space** — the theoretically optimal bound for this class of monotonic-stack problems.

I published the approach on LeetCode Discuss in November 2024 as a timestamped record, before submitting the PR. As of the submission date, no other public solution had reached O(n).

### LC 315 — Count of Smaller Numbers After Self (Merge Sort)

Solved using merge sort with index tracking — O(n log n) time, O(n) space. The merge-sort approach is cleaner than BIT/segment tree for this problem because it avoids coordinate compression and makes the inversion-count logic explicit.

### Other Hard Problems

Additional merged PRs cover a range of Hard problems, each submitted with full complexity analysis and clean implementations in Python/Java.

## In Progress: TheAlgorithms/Python

**PR #14728** — pending review at [TheAlgorithms/Python](https://github.com/TheAlgorithms/Python) (222K+ stars).

This PR introduces a **kth Next Greater Element** algorithm — a generalisation of the classic Next Greater Element problem. The approach uses k monotonic decreasing stacks in sequence, achieving **O(kn) average time, O(n) space**. To my knowledge, this is the first published implementation of this generalisation.

## Links

- [All merged PRs — doocs/leetcode](https://github.com/doocs/leetcode/pulls?q=is%3Apr+author%3AStarsExpress+is%3Amerged)
- [LC 2454 PR](https://github.com/doocs/leetcode/pull/2703)
- [TheAlgorithms/Python PR #14728](https://github.com/TheAlgorithms/Python/pull/14728)
- [LeetCode Discussion post — LC 2454 dual-stack O(n), Nov 2024](https://leetcode.com/problems/find-the-nearest-fair-integer/solutions/6107440/dual-stack-on-time-on-space-beat-100/)
