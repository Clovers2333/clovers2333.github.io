# Chapter 1. An Introduction to Approximation Algorithms

!!! Theorem

    If $P \neq NP$, then we can't simultaneously have algorithms that:

    - (1) Find optimal solutions
    - (2) Run in polynomial time
    - (3) For any instance

我们可以对三个条件分别进行松弛：

- 松弛第二个可以得到启发式算法；
- 松弛第三个可以得到特定情况下的多项式解；
- 松弛第一个则是这本书的主题——近似算法。

!!! Definition

    An $\alpha$-approximation algorithm for an optimization problem is a polynomial-time algorithm that for all instances of the problem produces a solution whose value is within a factor of $\alpha$ of the value of the optimal solution.