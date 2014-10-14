---
layout:     post
title:      "GC visualization"
date:       2014-10-13 12:00:00
categories: [SICP, Lisp]
tags:       [sicp, lisp]
published:  true
---

While I was working through the chapter 5.3 of [SICP][1], I created small visualization of the *stop-and-copy* garbage collection algorithm.

I start with the following memory structure example.

{% img center /images/posts/gc-01.png %}

The content of the `root` register is a pointer `p4` to the list of registers `(x y z)`. The register `x` points to address 6 where improper list `(1 . 2)` saved. The register `y` points to address 8 where list `(x x)` starts. Finally, the register `z` points to address 10 where list `(3 4 5)` starts. Addresses 1, 3, 5, 9, 11, 13, 15 contain garbage.

After we ran the GC algorithm, we got the following memory structure.

{% img center /images/posts/gc-52.png %}

`root` now points to address 0, `x` to 1, `y` to 3, and `z` to 6.

<!-- more -->

Here is the visualization

{% img center https://raw.githubusercontent.com/CompSciCabal/SMRTYPRTY/master/experiments/ndpar/sicp-5.3-gc/gc.gif %}


## References

- Bigger picture on [GitHub](https://raw.githubusercontent.com/CompSciCabal/SMRTYPRTY/master/experiments/ndpar/sicp-5.3-gc/gc.gif).
- Animation frames for better view on [GitHub](https://github.com/CompSciCabal/SMRTYPRTY/tree/master/experiments/ndpar/sicp-5.3-gc).


[1]: http://mitpress.mit.edu/sicp/full-text/book/book-Z-H-33.html#%_sec_5.3.2
