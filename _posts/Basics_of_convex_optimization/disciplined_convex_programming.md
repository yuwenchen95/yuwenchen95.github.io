<!-- ---
title: 'Disciplined Convex Programming: how to model a complex convex problem with basic building blocks?'
date: 2024-11-11
--- -->
Disciplined Convex Programming: how to model a complex convex problem with basic cone constraints
======

The first post ([What is convex optimization？common constraints and objectives](https://github.com/yuwenchen95/yuwenchen95.github.io/blob/master/_posts/Basics_of_convex_optimization/common_constraints_and_objectives.md)) introduced several basic cones supported in a conic optimization solver: zero cone, nonnegative cone, second-order cone, positive semidefinite cone, exponential cone and power cone. This blog will show how to check if your model is convex and a related package that can transform your convex optimization problem to a standard formulation with those basic cones above. 

<!-- ## Some commonly used convex terms
There are some commonly used functions that are convex in modelling a optimization problem:

###### Norms
For $x \in \mathbb{R}^n$, we have
- $L_1$-norm: $\| \mathbf{x} \|_1 = \sum_{i=1}^n |x_i|$
- $L_2$-norm: $\| \mathbf{x} \|_2 = \sqrt{\sum_{i=1}^n x_i^2}$
- Infinity norm: $\| \mathbf{x} \|_\infty = \max_{1 \leq i \leq n} |x_i|$
- $p$-norm: $\| \mathbf{x} \|_p = \left( \sum_{i=1}^n |x_i|^p \right)^{\frac{1}{p}}$
- Largest $k$-norm: $\| \mathbf{x} \|_{(k)} = \sum_{i=1}^{k} |x|_{[i]}$, where $|x|_{[i]}$ is the $i$-th largest absolute value of the components of $x$.
For a matrix $A \in \mathbb{R}^{m \times n}$, we have
- Frobenius norm: $\| A \|_F = \sqrt{\sum_{i=1}^m \sum_{j=1}^n |a_{ij}|^2}$
- Nuclear norm: $\| A \|_* = \sum_{i=1}^{\min\{m,n\}} \sigma_i$, $\sigma_i$ is the $i$-th singular value of $A$. -->

#### Gap between modellers and solver developers
For most people who are using optimization as a modelling tool, they are more interested in how to transform their application into an optimization problem instead of writting their own algorithm to solve it, especially when some complex constraints are used within the problem. They will model a problem and then let a solver (e.g. Gurobi, COPT, Mosek) to solve it.

However, from the view of solver developers, it will become really painful if we have to support every possible type of constraints internally within a solver, since the number of constraint types could be infinite. 

Is there any way we can alleviate the burden for solver developers but also support numerous choices of convex constraints in the meantime? The answer is yes, which relies on the idea called ***disciplined convex programming***. Based on rules of disciplined convex programming, we can (sometimes) easily check whether our problem is convex or not. Moreover, we can model a convex problem with elaborated constraints on a modelling platform, like *cvxpy* or *Convex.jl*, which helps to reformulate the problem into a one that can be solved by a standard conic optimization solver.

### Disciplined convex programming (DCP)
There are two key components for DCP as mentioned in the [original paper](https://stanford.edu/~boyd/papers/disc_cvx_prog.html):

- ***An atom library**: an extensible collection of functions and sets, or atoms, whose properties of curvature/shape (convex/concave/affine), monotonicity, and range are explicitly declared.*
- ***A convexity ruleset**: drawn from basic principles of convex analysis, that governs how atoms, variables, parameters, and numeric values can be combined to produce convex results.*

You can see that the DCP works similar to LEGO: **atoms** can be regarded as building blocks that we can assemble them under certain rules from the **convexity ruleset**, which preserves convexity of the new term after the combination. Thus, we can generate arbitrary convex functions and sets following [DCP rules](https://dcp.stanford.edu/rules). 
#### Convexity ruleset
DCP rules are based on operations that preserve convexity, which are detailed in [Section 3.2 of the book: Convex optimization](https://web.stanford.edu/~boyd/cvxbook/). We briefly summarize verification rules that are provided in DCP:

###### Sign
Each experession is flagged as  positive (non-negative), negative (non-positive), or unknown. Suppose we already know the signes of $f_1(x), f_2(x)$. For the experssion $f(x) = f_1(x)*f_2(x)$, $f(x)$ is *positive*, if $f_1(x)$ and $f_2(x)$ have the same sign, or *negative*, if $f_1(x)$ and $f_2(x)$ have opposite signs. Otherwise, we mark $f(x)$ is *unknown*.

###### Curvature
DCP can classify an expression as one of the four curvatures: *constant*, *affine*, *convex* and *concave*. Note that the affine curvature keeps both convex and concave properties. Otherwise, we mark $f(x)$ as unknown.

###### Curvature Rules
Curvature rules are based on the composition theorem from convex analysis to each (sub)expression. See Section 3.2.4 in the book.

#### Atomic functions
The atomic function is another important notion in DCP. It is an extensible list of functions and sets whose properties of curvature/shape, monotonicity, and range are known. 

An atomic function can be the composition of a class of atomic functions. For example, cvxpy support the $L_1$-norm 
```math
\begin{align}
\begin{aligned}
\| \mathbf{x} \|_1 = \sum_{i=1}^n |x_i| 
\end{aligned}
\end{align}
```
as an atomic function. If we are going to use the $L_1$-norm for regulariation in an optimization problem, we don't need to reformulate the $L_1$-norm as
```math
\begin{align}
\begin{aligned}
& \sum_{i=1}^n t_i \\
s.t. & -t_i \le x_i \le t_i, i = \{1, \dots, n\} 
\end{aligned}
\end{align}
```
explicitly but can input $|| \mathbf{x} ||_1$ directly in cvxpy. 

The DCP makes modelling a convex optimization problem very convenient to users who want to do a quick test for their models without transforming the model into the standard formulation of the solver they want to use. Modellers can now input a complex constraint via atomic functions insdead of building it from basic cones. In the end, all atomic functions will be automatically decomposed to basic cones that I have shown in the previous [post](https://github.com/yuwenchen95/yuwenchen95.github.io/blob/master/_posts/Basics_of_convex_optimization/common_constraints_and_objectives.md). You can find the current list of atom functions you can use in the [link](https://www.cvxpy.org/tutorial/functions/index.html#scalar-functions). Moreover, people can test performance of different solvers on their models by simply setting the *solver* in function *solve()*, e.g.
```
problem.solve(solver=<solver_name>, **kwargs)
```

#### From basic cones to atomic functions (To developer)
For modellers, we have shown that DCP is a user-friendly and effective tool for modelling a complex convex problem if sufficient atomic expressions are given. The last issue is how can we support those atoms by basic cones such as linear cones, second-order cones, exponential cones, power cones and positive semidefinite cones. This is the main work done by developers of a DCP platform, like cvxpy and Convex.jl. Information for atoms with the corresponding basic cones can be found on [this page](https://jump.dev/Convex.jl/stable/manual/operations/) provided by Convex.jl.

Thanks to the development of DCP platforms, the solver developers can focus on maintaining the support of several basic cones. When a new convex term becomes popular in an industry and needs to be frequently used, we only need to provide the corresponding atomic function on the DCP platform along with a paradigm that can automatically transform it into basic cones, without modifying the underlying code of the solver.

#### Limitation of DCP: 

###### DCP can't identify all convex problems
Following DCP rules, you can know check whether your function is convex or not using the [analyzer](https://dcp.stanford.edu/analyzer) from the DCP website. However, DCP will not be able to check the convexity of a problem, even it is a well-known convex problem, e.g. the entropy maximization problem:
```math
\begin{align}
\begin{aligned}
\max & \sum_{i=1}^n -x_i \log(x_i)\\
s.t. & \quad Ax=b, \\
    & \quad 1^\top x = 1, \\
    & x_i \ge 0, i = \{1, \dots, n\}.
\end{aligned}
\end{align}
```
DCP is not able to verify that a general product $a*b$ is convex, even if $a$ and $b$ are convex terms. 

However, we know the entropy function $-x \log(x)$ is a concave function for $x > 0$. By adding the new concave expression $-x \log(x)$ to the atom library, the DCP rules can verify the problem above is convex. In summary, adding more (nonlinear) atoms into the atom library makes the DCP modelling more powerful. 

<!-- ###### DCP can not achieve the best performance -->


#### Example
We will show how to use 