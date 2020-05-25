+++
draft = false
date = 2020-04-10
title = "Algorithms and Data Structures"
description = "Some notes for algorithms and data structures"
tags = ["Computer Science"]
+++

* [Dynamic Programming](#dynamic-programming)
* [Greedy Algorithms](#greedy-algorithms)
* [References](#references)

## Dynamic Programming

1. The two key ingredients:
    * optimal substructure: The optimal solution contains the optimal solutions to subproblems.
        * The subproblems need to be independent, which means the solution to one subproblem does not affect the solution to another subproblem.
    * overlapping subproblems

2. The other two steps:
    * Reconstruct an optimal solution
    * Memoization: When the subproblem is first encountered as the recursive algorithm unfolds, its solution is computed and then stored in the stable. Each subsequent time that we encounter this subproblem, we simply look up the value and return it.

## Greedy Algorithms

1. For many optimization problems, using dynamic programming is overkill. Even though there is almost always a more cumbersome dynamic-programming solution beneath every greedy algorithm.

2. A greedy algorithm always makes the choice that looks best at the moment. That is, it makes a locally optimal choice in the hope that this choice will lead to a globally optimal solution.

3. The two key ingredients:
    * greedy-choice property: we can assemble a globally optimal solution by making locally optimal (greedy) choices.
        * Unlike dynamic programming, which solves the subproblems before making the first choice (bottom up), a greedy algorithm makes its first choice before solving any subproblems (top down.)
    * optimal substructure

## References

* [Introduction to Algorithms](https://www.amazon.com/Introduction-Algorithms-Leiserson-published-Hardcover-dp-B008F1DKXU/dp/B008F1DKXU/ref=mt_paperback?_encoding=UTF8&me=&qid=1586534178)
