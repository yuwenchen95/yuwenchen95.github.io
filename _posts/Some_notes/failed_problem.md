<!-- ---
title: 'Which problems a conic solver can not solve?'
date: 2024-12-18
--- -->
Which problems a conic solver can not solve?
======

Conic solvers, like Mosek and CLarabel, claim to solve general conic optimization problems. We usually encounter numerically hard problems that can not be solved to the correct solution under Float64 data type. An straightforward solution is to increase the precision of data, like Float128 or more, such that we can avoid numerical issues caused by overflow or rounding error. However, there exist some subtle (but simple) problems that can not be solved a conic solver in theory. The issue comes from the break of strong duality assumption for a feasible problem, which is the foundation for conic solvers based on primal-dual algorithms. 


#### KKT optimality condition
