# Singular Values

Let $T\in\mathcal L(V,W)$, then $T^*T$ is a positive operator on $V$, the null space and range of $T^*T$ is the null space of $T$ and range of $T^*$, and the dimensions of the ranges of $T$, $T^*$, and $T^*T$ are all equal. We define the **singular values** of $T$ to be the nonnegative square roots of the eigenvalues of $T^*T$, listed in decreasing order, included once for every corresponding eigenvector.

$T$ is injective iff $0$ is not a singular value of $T$, $T$ is surjective iff the number of positive singular values of $T$ equals the dimension of $W$, and the number of positive singular values of $T$ equals the dimension of the range of $T$. $T$ is an isometry iff all the singular values of $T$ are 1.
# SVD for Linear Maps and Matrices

Let $T\in\mathcal L(V,W)$ and the positive singular values of $T$ are $s_k$, then there exist orthonormal lists $e_k$ in $V$ and $f_k$ in $W$ such that $Tv=\sum s_k\langle v,e_k\rangle f_k$ for every $v\in V$. 

If we define a **diagonal matrix** to be one where all the entries of the matrix are zero except possibly $A_{k,k}$ for $k=1,\ldots,\min(M,N)$, then the matrix of $T$ with respect to those orthonormal bases $e_k$ and $f_k$ equaling $s_k$ if on the diagonal and $0$ everywhere else means that it is diagonal as well.

We can now define the adjoint $T^*w=\sum s_k\langle w,f_k\rangle e_k$$ and the pseudoinverse $T^\dagger w=\sum\frac{\langle w,f_k\rangle}{s_k}e_k$.

Let $A$ be a $p$-by-$n$ matrix of rank $m\geq 1$, then there exists a $p$-by-$m$ $B$ and $n$-by-$m$ $C$ with orthonormal columns and an $m$-by-$m$ diagonal matrix $D$ with positive numbers on the diagonal such that $A=BDC^*$. We can construct this by making the columns of $B$ $f_k$, the diagonal entries of $D$ $s_k$, and the columns of $C$ $e_k$.