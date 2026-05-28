# Null Spaces of Powers of an Operator

Suppose $T\in\mathcal L(V)$, then $\set{0}$ is the null space of $T^0$, a subset of the null space of $T$, a subset of the null space of $T^2$, and so on and so forth. If two consecutive terms in this sequence are equal, then all later terms are equal as well. It turns out that this is guaranteed to happen at or before $T^{\dim V}$. This means that $V=\text{null } T^{\dim V}+\text{range }T^{\dim V}$, even if the same cannot be said for $T$.
# Generalized Eigenvectors

For some operator $T$, we'd like to find a nice direct sum decomposition $V=V_1\oplus \cdots\oplus V_n$ where each $V_k$ is $T$-invariant. A decomposition of this form containing only one-dimensional $V_k$ is only possible iff $V$ has a basis consisting of eigenvectors of $T$. The spectral theorem gives us this decomposition for self-adjoint operators in $\mathbb R$ and normal operators in $\mathbb C$, but we can't say the same for a general operator.

A vector $v\in V$ is a **generalized eigenvector** of $T$ if $v\neq 0$ and $(T-\lambda I)^kv-0$ for some positive $k$. Using our results from the previous section