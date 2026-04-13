# Isometries

A linear map is an **isometry** if it preserves norms. In other words, $\|Sv\|=v$ for all $v\in V$. They must be injective as if $v\neq 0$, $\|Sv\|\neq 0$.

Let $S\in\mathcal L(V,W)$, $e_k$ is an orthonormal basis of $V$ and $f_k$ is an orthonormal basis of $W$. Then. $S$ is an isometry iff $S^*S=I$ iff $\langle Su,Sv\rangle=\langle u,v\rangle$ iff $Se_1,\ldots,Se_n$ is an orthonormal list in $W$ iff the columns of $\mathcal M(S,e_k,f_j)$ form an orthonormal list in $\mathbb F^m$ with respect to the Euclidean inner product.
# Unitary Operators

An operator $S\in\mathcal L(V)$ is called **unitary** if $S$ is an invertible isometry. Note that all isometries are invertible in finite-dimensional vector spaces.

Let $e_k$ be an orthonormal basis of $V$, then $S$ is unitary iff $S^*S=SS^*=I$ iff $S^{-1}=S^*$ iff $Se_k$ is an orthonormal basis of $V$ iff the rows of $\mathcal M(S,e_k)$ form an orthonormal basis of $\mathbb F^n$ with respect to the Euclidean inner product iff $S^*$ is unitary.

Recall that if an operator $S$ corresponds to a complex number $z$, then $S^*$ corresponds to $\overline z$. When $S$ is self-adjoint, $z$ is a real number, and when $S$ is unitary, then $|z|=1$, which means unitary operators are analogous to the unit circle. This means that their eigenvalues have absolute value $1$.

If $S$ is unitary on a complex inner product space, then there's an orthonormal basis of that space consisting of eigenvectors of $S$ whose corresponding eigenvalues all have absolute value $1$.
# QR Factorization

An $n$-by-$n$ matrix is called **unitary** if its columns form an orthonormal list in $\mathbb F^n$, which is a basis as well. $Q$ is unitary iff the rows of $Q$ form an orthonormal list in $\mathbb F^n$ iff $\|Qv\|=\|v\|$ iff $Q^*Q=QQ^*=I$.

Let $A$ be a square matrix with linearly independent columns, then there exist unique unitary $Q$ and upper triangular $R$ with positive eigenvalues such that $A=QR$. We can do this by using Gram-Schmidt to construct an orthonormal basis $e_k$ out of the columns $v_j$ of $A$. If we make these basis vectors the columns of $Q$ and define $R_{j,k}=\langle v_k,e_j\rangle$, then $A=QR$.

If we are trying to solve $Ax=b$, we can easily convert this to $Rx=Q^*b$, the right hand side being easy to calculate and allowing us to solve for $x$ element-by-element.
# Cholesky Factorization

Recall the definition of a positive operator. It's weird that the definition includes operators where $\langle Tv,v\rangle=0$. We will define a **positive definite** matrix to be one where $B^*=B$ and $\langle Bv,v\rangle>0$.

A matrix is upper triangular iff its conjugate transpose is lower triangular. Because $B$ is positive definite (and therefore positive), $B=A^*A$ for some invertible square matrix $A$. Then, let $A=QR$ be the QR factorization of $A$, which makes $A^*=R^*Q^*$. Since $Q$ is unitary, $B=A^*A=R^*Q^*QR=R^*R$, where $R$ is an unique upper-triangular matrix with positive eigenvalues.