Xing's notes for algorithm complexity
=====================================

f(n) = O(g(n)) means there are positive constants c and n0, such that 0 ≤ f(n) ≤ cg(n) for all n ≥ n0. The values of c and n0 must not be depend on n. 代表最差情况。O(g(n))包含了比他好的所有情况。

有条件判断的时候，计算最耗时的分支。