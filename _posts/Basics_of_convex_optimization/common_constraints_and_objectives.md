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
\begin{align*}
\begin{aligned}
\text{min} \quad & f(x)\\[2ex]
\text{subject to} \quad & h(x) = 0, \\[1ex]
        & g(x) \le 0, 
\end{aligned}
\end{align*}
```
with decision variables $x \in \mathbb{R}^n$, $h(x) \in \mathbb{R}^{p}$, $g(x) \in \mathbb{R}^{m}$.

Convexity: set $\textit{or}$ objective?
------
The convexity has been defined for both constraints and objectives. For a constraint $\mathcal{S}$ that is convex, it satisfies:
```math
\begin{align*}
\begin{aligned}
\forall x, y \in \mathcal{S} \Rightarrow ax+(1-a)y \in \mathcal{S}, \ \forall a \in [0,1].
\end{aligned}
\end{align*}
```

For a function $f(x): X \rightarrow \mathbb{{R}}$ that is convex, it satisfies:
```math
\begin{align*}
\begin{aligned}
\forall x_1, x_2 \in X, f(ax_1 + (1-a)x_2) \le af(x_1) + (1-a)f(x_2), \ \forall a \in [0,1],
\end{aligned}
\end{align*}
```
where $X$ is a convex subset. 

***Unified views of convex sets and objectives***

The convexity of constraints and functions can be unified by the use of epigraph, i.e. $f(x) \le t, \ t \in \mathbb{R}$. Then the set $\{(t,x) \in \mathbb{R} \times X | f(x) \le t\}$ is a convex set for variable $(t,x)$.

Convex optimization
------
When funtion $f(x), g(x)$ are convex and $h(x)$ is a linear map (the only choice makes $h(x)=0$ convex), then the problem (1) becomes a convex optimization problem, i.e. 
```math
\begin{align*}
\begin{aligned}
\text{min} \quad & f(x)\\[2ex]
 \text{subject to} \quad & A x = b, \\[1ex]
        & g(x) \le 0, 
\end{aligned}   \qquad \qquad (1)
\end{align*}
```
where $A \in \mathbb{R}^{p \times n}$.

Conic optimization
------
Since my background is in conic optimization, I will introduce convex optimization in the view of conic optimization. 

For problem (1), we can always transform the nonlinear objective $f(x)$ by the epigragh transformation, i.e.
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
\begin{align*}
\begin{aligned}
\text{min} \quad & c^\top x\\[2ex]
 \text{subject to} \quad & A x = b, \\[1ex]
        & x \in \mathcal{K}.
\end{aligned}
\end{align*}
```
There are many types of convex cones but almost all of them can be decomposed into the combination of following atomic cones:
- **Zero cone:**  
  $\{0\}^n := \left\{ x \in \mathbb{R}^n \ \middle| \ x_i = 0, \forall i = 1, \ldots, n \right\}$.

- **Nonnegative cone:**  
  $\mathbb{R}_{+}^n := \left\{ x \in \mathbb{R}^n \ \middle| \ x_i \geq 0, \forall i = 1, \ldots, n \right\}$.

- **Second-order cone:**  
  $\mathcal{K}_{\mathrm{soc}}^n := \left\{ (t, x) \ \middle| \ x \in \mathbb{R}^{n-1}, t \in \mathbb{R}_{+}, \|x\|_2 \leq t \right\}$.

- **Positive semidefinite cone:**  
  $\mathbb{S}_{+}^n := \left\{ X \in \mathbb{S}^n \ \middle| \ v^{\top} X v \geq 0 \quad \forall v \in \mathbb{R}^n \right\}$.

- **Exponential cone:**  
  $\mathcal{K}_{\exp} := \left\{ (x, y, z) \ \middle| \ y > 0, y \exp \left( \frac{x}{y} \right) \leq z \right\} \cup \left\{ (x, 0, z) \mid x \leq 0, z \geq 0 \right\}$.

- **Power cone:**  
  \(\mathcal{K}_{\text{pow},\alpha} = \left\{ (x, y, z) \ \middle| \ x^\alpha y^{1-\alpha} \geq |z|, x \geq 0, y \geq 0, \alpha \in (0,1) \right\}\).

These are basic cones that are supported by conic solvers, e.g. [Mosek](https://www.mosek.com/) and [Clarabel](https://clarabel.org/stable/). (Note that the zero cone and the nonnegative cone correspond to linear equality and inequality constraints respectively.)

There are also other solvers more classes of convex cones, e.g. [DDS](https://link.springer.com/article/10.1007/s12532-023-00248-2) and [Hypatia](https://jump.dev/Hypatia.jl/stable/), but almost all convex problems we have encountered in real-world applications can be formulated by those basic cones above.

Code examples
------
Here I will show how to model a second-order cone optimization via [JuMP](https://jump.dev/JuMP.jl/stable/), which is a domain-specific modeling language for mathematical optimization in Julia language. Suppose we want to build up the robust support vector machine (SVM) model in machine learning, where the training process is framed as solving a convex optimization problem. Given a set of training samples
 ```math
 \begin{align*}
 \left\{x_i, y_i\right\}_{i=1}^m \subseteq \mathbb{R}^n \times\{-1,+1\},
 \end{align*}
 ``` 
 for a standard binary classification task, the resulting classifier is defined as: 
 ```math
 \begin{align*}
 h_{w, b}(x)=\operatorname{sgn}(\langle w, x\rangle+b),
 \end{align*}
 ```
 where the parameters $w, b$ are the solution of the following convex optimization problem:
```math
\begin{align*}
\begin{aligned}
	\min_{w, w_0, t}: \quad& \frac{1}{m}\sum_{i=1}^m t_i + R(w, w_0) \\
	\text{ s.t.} \quad& t_i \geq 1-y_i\left(\left\langle w, x_i\right\rangle+ w_0\right), \ \forall i = 1, \dots, m \\
	\quad & t_i \geq 0.
\end{aligned}
\end{align*}
```
Here, $\zeta \in \mathbb{R}^m$ represents the slack variables, which quantify the prediction error, and $R(w,b)$ denotes the regularization term for the parameters $w$ and $b$. 

Choosing the norm-2 regularization, i.e. $R(w,w_0) = \lambda \|w\|_2, \lambda > 0$, yields the [robust version of SVM](https://www.jmlr.org/papers/v10/xu09b.html) that can be reformulated as a second-order cone programming, where we introduce $\alpha$ as an upper bound for $\|w\|_2$ that can be set as a second-order cone constraint $\|w\|_2 \le \alpha$: 

```
using MLDatasets        # Machine learning dataset
using Clarabel          # Clarabel solver
using JuMP              # JuMP modelling 

trainset = MNIST(:train)        # MNIST data
length(trainset)
X_train, Y_train = trainset[:]
(a,b,c) = size(X_train)
X_train = reshape(X_train,(a*b,c))      # parse training data

#take a subset of points
X = X_train[:,1:end] 
Y = Y_train[1:end]
#Suppose we are going to train a binary classifier for the number 1
Y = map(x -> x == 1 ? 1 : -1, Y)

#n is the feature number and m is the number of data points
(n,m) = size(X)
# regularization term
λ = 0.01
# create JuMP model
model = Model(Clarabel.Optimizer)
# coefficients w,b in a SVM
@variable(model, w[1:n])                
@variable(model, w0)     
@variable(model, α)
# the slack variable t for the predictor error
@variable(model, t[1:m])

@constraint(model, 1.0 .- Y.*(X'*w .- w0) .<= t)
@constraint(model, t .>= 0)             
# L-2 regularization (robust interpretation)
@constraint(model, [α; w] in MOI.SecondOrderCone(length(w) + 1))        
# set objective function
@objective(model, Min, sum(t)/m + λ*α)
optimize!(model)
```

Other examples like logistic regression and experimental design can also be found on the [JuMP page](https://jump.dev/JuMP.jl/stable/tutorials/conic/introduction/).