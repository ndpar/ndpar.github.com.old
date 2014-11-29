---
layout:     post
title:      "Two series"
date:       2014-11-29 12:00:00
categories: [Math]
tags:       [math]
published:  true
---

Cliff Pickover [twitted](https://twitter.com/pickover/status/538754166261555201) a fun puzzle: Which series is bigger?

$$
\sum_{n=0}^\infty \frac{1}{2^{n+1}} \quad \text{or} \quad \sum_{n=0}^\infty \frac{n}{2^{n+1}}
$$

The first one is the famous geometric series which sum is equal to 1. The second one seems to be bigger because 1 < n, except for the 0th term, but that 0th term makes a big difference.

<!-- more -->

First of all, the second series converges. You can prove it by D'Alembert's theorem

$$
\frac{a_{n+1}}{a_n} = \frac{n+1}{2^{n+2}}\cdot\frac{2^{n+1}}{n} = \frac{1}{2}\left(1 + \frac{1}{n}\right) \le \frac{3}{4} < 1, \text{for} \; n \ge 2
$$

Because the terms are positive, the series converges absolutely, therefore we can rearrange the terms.

$$
\begin{align*}
S & = \sum_{n=0}^\infty \frac{n}{2^{n+1}} = \sum_{n=0}^\infty \left(\frac{1}{2^{n+1}} + \frac{n-1}{2^{n+1}}\right) = \sum_{n=0}^\infty \frac{1}{2^{n+1}} + \sum_{n=0}^\infty \frac{n-1}{2^{n+1}} \\
  & = 1 + \frac{1}{2} \sum_{n=0}^\infty \frac{n-1}{2^n} = 1 + \frac{1}{2} \sum_{n=-1}^\infty \frac{n}{2^{n+1}} = 1 - \frac{1}{2} + \frac{1}{2} \sum_{n=0}^\infty \frac{n}{2^{n+1}} \\
  & = \frac{1}{2} + \frac{1}{2}S \implies S = 1
\end{align*}
$$

Both series are equal.
