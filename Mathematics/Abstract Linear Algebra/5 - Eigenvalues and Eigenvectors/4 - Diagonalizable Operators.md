# Diagonal Matrices

A **diagonal matrix** is a square matrix that is 0 everywhere except possibly on the diagonal, and the values on that diagonal are the eigenvalues of the associated operator. If an operator has a diagonal matrix with respect to some basis, then it's called **diagonalizable**.

 We will define the **eigenspace** $E(\lambda, T)$ to be the subspace of $V$ defined by $\set{v\in V: Tv=\lambda v}$, the set of all eigenvectors of $T$ corresponding to $\lambda$. Restricting $T$ to $E(\lambda, T)$ makes it just the operator of multiplication by $\lambda$. Since eigenspaces are classified by their eigenvectors, the sum of eigenspaces is direct.
# Conditions for Diagonalizability

Let $V$ be finite-dimensional and $T$ be an operator on $V$ with eigenvalues $\lambda_1,\ldots,\lambda_m$. Then the following are equivalent: $T$ is diagonalizable; $V$ has a basis consisting of eigenvectors of $T$; $V$ equals the direct sum of eigenspaces; the dimension of $V$ is the sum of the dimensions of the eigenspaces.

We know that every operator on a nonzero finite-dimensional complex vector space has an eigenvalue, but not all of them have enough to be diagonalizable. We know that if there are a number of distinct eigenvalues equal to the dimension of $V$, then $T$ is diagonalizable, but it isn't a necessary condition. Also, we know that $T^kv=\lambda^kv$ for eigenvectors $\lambda$.

Recall that an operator $T$ has an upper-triangular matrix iff the minimal polynomial is of the form $(z-\lambda_1)\cdots(z-\lambda_m)$. The operator is only diagonalizable if those eigenvalues are distinct.

Since the minimal polynomial of $T$ is a polynomial multiple of $T|_U$, $T$ being diagonalizable implies $T|_U$ is a diagonalizable operator on $U$.