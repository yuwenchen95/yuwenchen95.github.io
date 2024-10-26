<!-- ---
title: 'What is convex optimization？common constraints and objectives'
date: 2024-10-25
permalink: /posts/2024/10/Convex optimization blogs/
tags:
  - cool posts
  - category1
  - category2
--- -->
What is convex optimization？common constraints and objectives
======

Convex optimization is a powerful tool that has been used in many areas (which will be listed in future posts). Compared to nonconvex optimization, Any $p$th-order ($p \ge 1$) stationary point coincides with a global optimal solution, i.e. we can use any $p$th-order ($p \ge 1$) algorithm to solve a convex problem to global optimum. 

Standard formulation
------
An optimization can be formulated as:
```math
\begin{array}{r}
\text{minimize} & f(x)\\[2ex]
 \text{subject to} & h(x) = 0, \\[1ex]
        & g(x) \le 0, 
 \end{array}
```
with decision variables ``x \in \mathbb{R}^n``, ``h(x) \in \mathbb{R}^{p}``, ``g(x) \in \mathbb{R}^{m}``. When funtion $f(x), g(x)$ are convex and $h(x)$ is a linear map, then the problem above is a convex optimization problem, i.e. 
```math
\begin{array}{r}
\text{minimize} & f(x)\\[2ex]
 \text{subject to} & A x = b, \\[1ex]
        & g(x) \le 0, 
 \end{array}
```
where ``A \in \mathbb{R}^{p \times n}``.