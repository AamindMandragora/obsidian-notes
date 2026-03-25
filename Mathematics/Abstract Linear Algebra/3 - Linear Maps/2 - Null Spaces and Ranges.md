# Null Space and Injectivity

For some linear map $T$ from $V$ to $W$, the **null space** (kernel) of $T$ is the subset of $V$ containing vectors that map to $0$ in $W$. In other words: $\text{null }T=\set{v\in V: Tv=0}$. By properties of linear maps, the null space of $T$ is a subspace of $V$.

A function is called **injective** (one-to-one) if $Tu=Tv$ implies $u=v$. However, consider some vector $z$ such that $Tz=0$. Then, $T(u+z)=Tu+Tz=Tv+0=Tv$, but $u+z\neq v$. This means that if any such $z$ exists, the map cannot be injective. Since all such $z$ live in the nullspace, injectivity is satisfied if and only if the null space is trivial (only contains zero).
# Range and Surjective

The range of a linear map $T$ is the subset of $W$ consisting of vectors equal to $Tv$ for some $v$. In other words: $\text{range }T=\set{Tv: v\in V}$. By properties of linear maps, the range of $T$ is a subspace of $W$.

A function is called **surjective** (onto) if its range equals $W$. The same function can be surjective or not depending on the choice of $W$.
# Fundamental Theorem of Linear Maps

The dimension of the linear map equals the sum of the dimensions of its null space and range. This should intuitively make sense: for every basis vector of $V$, it must either send to something in the range or to $0$, which means they are partitioned between the null space and range.

This means that no linear map to a lower-dimensional space can be injective, and no linear map to a higher-dimensional space can be surjective.

Let's reconsider the question of whether a homogeneous (RHS = 0) system of linear equations has a nonzero solution. If we have a system of the form $\begin{align}\sum_{k=1}^nA_{j,k}x_k=0\end{align}$ for $1\leq j \leq m$ and $A_{j,k}\in\mathbb F$, we can define some $T: \mathbb F^n\to\mathbb F^m$ by: $$T(x_1,\ldots,x_n)=\left(\sum_{k=1}^nA_{1,k}x_k, \ldots, \sum_{k=1}^nA_{m,k}x_k\right)$$then this question boils down to checking if the null space of $T$ isn't trivial, which is equivalent to $T$ not being injective. We know that linear maps have to send to a non-lower-dimensional space to be injective, so the number of variables ($n$) must be less than or equal to the number of equations ($m$).

Let $c_k$ be the corresponding nonhomogeneous RHS of the $k\text{-th}$ equation in the system, then checking when $T(x_1,\ldots,x_n)=(c_1,\ldots,c_m)$ for all choices of $c_k$ is equivalent to seeing if the range of $T$ equals $\mathbb F^m$, which is only possible when the number of equations ($m$) is less than or equal to the number of variables ($n$).