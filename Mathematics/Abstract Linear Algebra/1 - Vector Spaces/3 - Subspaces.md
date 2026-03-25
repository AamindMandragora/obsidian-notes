A subset $U$ of $V$ is called a (linear) **subspace** of $V$ if $U$ is also a vector space with the same addition, identity, and scalar multiplication as $V$. The easiest way to check if this is true is by verifying $U$'s additive identity and closure under the two operations.
# Sums of Subspaces

We really don't care about subsets of vector spaces unless they're subspaces. The sum of subspaces $V_1,\ldots,V_m$ of $V$ is the set of all possible sums of their elements, which we can do as they all share an addition. In other words: $$V_1+\cdots+V_m=\set{v_1+\cdots+v_m:v_1\in V_1,\ldots,v_m\in V_m}$$Additionally, a sum of subspaces $\sum V_k$ turns out to be the smallest subspace containing every $V_k$.
# Direct Sums

Suppose $V_1,\ldots,V_m$ are subspaces of $V$. We know every element of their sum can be written in the form of the sum of one vector from each subspace. In the case where there's only one such way to write any given element in that form, we call that sum of subspaces a **direct sum** (with symbol $\oplus$).

If we have two sums $\sum u_k = \sum w_k$ that equals an element in the sum of the subspaces, then we can subtract one from the other to get an alternate way to write the zero vector in the sum of subspaces. Conversely, we can take an alternate sum for the zero vector and add it to the sum for any vector to create a new way to write that vector. Therefore, a sum is a direct sum if and only if there's only one way to write $0$ as a sum (which must necessarily be $\sum 0$).

If there's some common vector between two subspaces, then there's automatically an extra way to write $0$ as the sum of that vector and its inverse, so a direct sum can only happen between spaces with a trivial intersection ($\set{0}$).