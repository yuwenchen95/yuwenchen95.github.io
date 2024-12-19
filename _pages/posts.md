---
layout: archive
title: "Blogs"
permalink: /posts/
author_profile: true
redirect_from:

---

{% include base_path %}

<!-- Here will be my lists of blogs. [here](https://github.com/yuwenchen95/yuwenchen95.github.io/blob/master/_posts/Convex_optimization_blogs.md).
   -->

This is the starting page of my own blogs on convex optimization after 5 years of study of optimization theory since my master program at ETH Zurich. I started my optimization career by mistakes: I initially thought it is a compulsory course for master students back in China and start to learn it during the summer vacation, but then realize it is a really fascinating topic and shift my study into it.

In this blog, I will focus on the introduction to a variety of optimization algorithms.

Convex optimization
======

Blogs
------

- Basics of convex optimization
   1. [What is convex optimization？common constraints and objectives](https://github.com/yuwenchen95/yuwenchen95.github.io/blob/master/_posts/Basics_of_convex_optimization/common_constraints_and_objectives.md).
   2. [Disciplined Convex Programming](https://github.com/yuwenchen95/yuwenchen95.github.io/blob/master/_posts/Basics_of_convex_optimization/disciplined_convex_programming.md)

- Some notes
   1. [Which problems a solver can not solve?](https://github.com/yuwenchen95/yuwenchen95.github.io/blob/master/_posts/Some_notes/failed_problem.md).

- 凸优化算法
   1. 零阶算法
   2. 一阶算法
      1. Feasible methods
        - 梯度下降
        - 投影梯度下降
        - proximal algorithm
        - simplex (active-set) method
      2. Infeasible method
        - 增广拉格朗日法
        - ADMM
        - 算子分裂法
   3. 二阶算法
      1. 内点法
      2. 拟牛顿法

- 凸优化案例
  - 模块预测控制 (MPC)
  - 支持向量机 (SVM)
  - Portfolio optimization
  - 最优潮流 (Optimal power flow)
  - 最优传输 (Optimal transport)
  - 航天器软着陆控制

- 凸优化求解器
  1. Gurobi
  2. Mosek
  3. Clarabel
  4. PDLP
  5. OSQP
  6. SCS

- 凸优化建模平台
  1. Python
     - cvxpy
  2. Julia
     - JuMP
  
- 非凸优化


其他推荐学习网站
------