<!-- ---
title: 'What is convex optimization？common constraints and objectives'
date: 2024-10-25
permalink: /posts/2024/10/Convex optimization blogs/
tags:
  - cool posts
  - category1
  - category2
--- -->
What is convex optimization & building blocks through conic optimization
======

Convex optimization is a powerful tool that has been used in many areas (which will be listed in future posts). Compared to nonconvex optimization, Any $p$ th-order ($p \ge 1$) stationary point coincides with a global optimal solution, i.e. we can use any $p$ th-order ($p \ge 1$) algorithm to solve a convex problem to global optimum. This is the computational advantage of convex optimization over nonxonvex optimization. Moreover, convex optimization can be used within nonconvex optimization. For example, we solve a relaxed convex problem for each node of a mixed integer programming within branch-and-bound framework, or people usually solve a convex relaxation of a hard nonconvex problem and then try to recover a feasible solution (sometimes global optimum) of the original nonconvex problem.


Standard optimization formulation
------
An optimization can be formulated as:
```math
\begin{align}
\begin{aligned}
\text{min} \quad & f(x)\\[2ex]
\text{subject to} \quad & h(x) = 0, \\[1ex]
        & g(x) \le 0, 
\end{aligned}
\end{align}
```
with decision variables $x \in \mathbb{R}^n$, $h(x) \in \mathbb{R}^{p}$, $g(x) \in \mathbb{R}^{m}$.

Convexity
------
Since we will encounter convexity in both constraints and objectives. For a constraint $\mathcal{S}$ that is convex, it satisfies:
$$
\forall x, y \in \mathcal{S} \Rightarrow ax+(1-a)y \in \mathcal{S}, \ \forall a \in [0,1].
$$

For a function $f(x): X \rightarrow \mathbb{{R}}$ that is convex, it satisfies:
$$
\forall x_1, x_2 \in X, f(ax_1 + (1-a)x_2) \le af(x_1) + (1-a)f(x_2), \ \forall a \in [0,1],
$$
where $X$ is a convex subset. The convexity of constraints and functions can be unified by the use of epigraph, i.e. $f(x) \le t, \ t \in \mathbb{R}$. Then the set $\{(t,x) \in \mathbb{R} \times X | f(x) \le t\}$ is a convex set.

Convex optimization
------
When funtion $f(x), g(x)$ are convex and $h(x)$ is a linear map (the only choice makes $h(x)=0$ convex), then the problem (1) becomes a convex optimization problem, i.e. 
```math
\begin{align}
\begin{aligned}
\text{min} \quad & f(x)\\[2ex]
 \text{subject to} \quad & A x = b, \\[1ex]
        & g(x) \le 0, 
\end{aligned}
\end{align}
```
where $A \in \mathbb{R}^{p \times n}$.

Conic optimization
------
Since my background is in conic optimization, I will introduce convex optimization in the view of conic optimization. 

For problem (2), we can always transform the nonlinear objective $f(x)$ by the epigragh transformation, i.e.
```math
\begin{array}{r}
\text{min} \quad & t\\[2ex]
 \text{subject to} \quad & A x = b, \\[1ex]
        & g(x) \le 0, \\
        & f(x) - t \le 0.
\end{array}
```
The problem above yields the same optimal solution as problem (2), but now we can fix the objective function to be linear. Then, we can also restrict the constraint $g(x) \le 0$ to be a conic constraint. The conic constraint $x \in \mathcal{K}$ is a generalization of inequality constraints, which is mentioned in the book [Convex Optimization (Section 3.6)](https://web.stanford.edu/~boyd/cvxbook/).  

The conic problem is of the following formulation:
```math
\begin{align}
\begin{aligned}
\text{min} \quad & c^\top x\\[2ex]
 \text{subject to} \quad & A x = b, \\[1ex]
        & x \in \mathcal{K}.
\end{aligned}
\end{align}
```
There are many types of cones but almost all of them can be decomposed into the combination of following atomic cones: