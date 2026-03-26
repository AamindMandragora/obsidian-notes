# Eigenvalues

A linear map from a vector space to itself is called an **operator**. If we have some $T\in\mathcal L(V)$ and we can write $V$ as a direct sum of more than two subspaces, then we only need to analyze $T\mid_{V_k}$, the restriction of $T$ to $V_k$, to understand the behavior of $T$. However, $T\mid_{V_k}$ may not map $V_k$ to itself, and thus might not be an operator.

A subspace $U$ of $V$ that gets mapped to itself by $T$ ($Tu\in U$ for all $u\in U$) is called **invariant**. $U$ is invariant under $T$ if $T\mid_U$ is an operator on $U$. The trivial subspace, $V$, the null space of $T$, and the range of $T$ are all invariant.

If we create a one-dimensional subspace $U$ of $V$ such that $U=\set{\lambda v:\lambda\in\mathbb F}$ for some predetermined nonzero $v\in V$, then if $U$ is invariant under some operator $T$, then $Tv\in U$, which means that there exists some $\lambda$ such that $Tv=\lambda v$. The converse says that if that lambda exists, then the span of $v$ is a one-dimensional subspace of $V$ invariant under $T$. This $\lambda$ is called an **eigenvalue**, and $v$ is its corresponding **eigenvector**.

We can use these properties to say that if $\lambda$ is an eigenvalue of $T$, then $T-\lambda I$ isn't injective, surjective, or invertible. Also, every list of eigenvectors of $T$ corresponding to distinct eigenvalues is linearly independent and $T$ has at most $\dim V$ distinct eigenvalues.
# Polynomials Applied to Operators

We define $T^0=I$ for any operator $T$, and $T^m$ to be the operator $T$ applied $m$ times. $T^{-m}=(T^{-1})^m$ for any invertible $T$. Now that we have powers of operators, let $p(z)=a_0+a_1z+\cdots+a_mz^m$, then $p(T)=a_0I+a_1T+a_2T^2+\cdots+a_mT^m$.

If we fix some operator $T$, then the function on $\mathcal P(\mathbb F)\to \mathcal L(V)$ defined by $p\mapsto p(T)$ is linear. We can commutatively multiply polynomials applied to an operator.

We already knew the null space and range of $T$ are invariant under $T$, but now we know that the null space and range of every polynomial of $T$ is invariant as well.