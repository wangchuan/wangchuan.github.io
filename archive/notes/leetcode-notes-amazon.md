---
layout: post
title: "LeetCode Notes for Amazon"
categories: hidden
tags: leetcode
use_math: true
---

* TOC
{:toc}

## LeetCode Notes for Amazon

### Time Complexity Analysis
1. $f(n) = n + 2 f(\frac{n}{2}) \rightarrow f(n) = n\log(n)$
2. $f(n) = n + f(\frac{n}{2}) \rightarrow f(n) = 2n$
3. $f(n) = 1 + f(\frac{n}{2}) \rightarrow f(n) = \log(n) $
4. Two pointers: `i` and `j` moving forward, but in `for-for` loop. It's `O(2n)`.



