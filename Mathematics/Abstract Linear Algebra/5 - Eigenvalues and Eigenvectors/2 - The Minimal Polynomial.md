# Existence of Eigenvalues on Complex Vector Spaces

Every operator $T$ on a finite-dimensional nonzero complex vector space has an eigenvalue, as if we take the list $v,Tv,T^2v,\ldots,T^nv$, it cannot be linearly independent as it has length $n+1$, which means there's a nonconstant polynomial $p$ of smallest degree such that $p(T)v=0$. By the fundamental theorem of algebra, there exists $\lambda\in\mathbb C$ such that $p(\lambda)=0$, which means there exists a $q\in\mathcal P(\mathbb C)$ such that $p(z)=(z-\lambda)q(z)$, implying that $0=p(T)v=(T-\lambda I)(q(T)v)$. All this was to say that, since the degree of $q$ is less than that of $p$, $q(T)v\neq 0$, so $\lambda$ is an eigenvalue of $T$ with eigenvector $q(T)v$.

Unfortunately, that process only works when the field is the complex numbers and the vector space has finite dimension.
# Eigenvalues and the Minimal Polynomial

A **monic** polynomial has its highest-degree coefficient equal one. For finite dimensional $V$ and linear map $T$, there is a unique monic polynomial $p\in\mathcal P(\mathbb F)$ of smallest degree (less than the dimension of $V$) such that $p(T)=0$. We call this the **minimal polynomial** of $T$. To compute it, we must find the smallest positive $m$ such that there exists a solution for $c_k$ in $$c_0I+c_1T+\cdots+c_{m-1}T^{m-1}=-T^m$$We can pick a basis of $V$ and replace $T$ with its matrix to get a system of $(\dim V)^2$ linear equations in the $m$ unknowns. We can also usually say $m=\dim V$, although there are some exceptions.

The zeroes of the minimal polynomial are the eigenvalues, and for complex vector spaces, the minimal polynomial can be completely factored into $(z-\lambda_k)$ terms, where $\lambda_k$ is an eigenvalue. Every monic polynomial is the minimal polynomial of some operator, and since come can't be factored, it's not possible to find eigenvalues for some operators.

If $q(T)=0$, then $q$ is a polynomial multiple of the minimal polynomial. This also means that if $U$ is a subspace of $V$ invariant under $T$, then the minimal polynomial of $T$ is a polynomial multiple of the minimal polynomial of $T\mid_U$.

If zero is an eigenvalue of $T$, then $T$ isn't invertible as $A-0I=A$. This means that if the constant term of the minimal polynomial of $T$ is zero, then $T$ isn't invertible either.
# Eigenvalues on Odd-Dimensional Real Vector Spaces

