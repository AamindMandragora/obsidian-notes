# Dual Space and Dual Map

A **linear functional** on $V$ is a linear map from $V$ to $\mathbb F$, and the set of all linear functionals is called the **dual space** $V'=\mathcal L(V,\mathbb F)$. We can see that the dimension of the dual space is equal to the dimension of $V$, and we can define a **dual basis** of some basis of $V$ by finding functionals that map the $k\text{-th}$ basis vector of $V$ to $1$ and every other one to $0$ for each $k$.

Since functionals are linear maps, we can write any vector $v\in V$ as $\sum_{k=1}^n \phi_k(v)v_k$ for basis $(...,v_k,...)$ and dual basis $(...,\phi_k,...)$. We can use this to show that the dual basis is linearly independent, which means that the dual basis of a basis of $V$ is a basis of $V'$.

If we let some $T\in\mathcal L(V,W)$ exist, then the **dual map** of $T$ is the linear map $T'\in\mathcal L(W',V')$ defined for each $\phi\in W'$ by $T'(\phi)=\phi\circ T$. The dual operator distributes over addition and scalar multiplication of maps, and $(ST)'=T'S'$ for all $S\in\mathcal L(W,U)$.
# Null Space and Range of Dual of Linear Map

Define the **annihilator** of some subset $U\subseteq V$ by $U^0=\set{\phi\in V':(\phi(u)=0:u\in U)}$, containing linear functionals that send elements in the subset to zero. $U^0\subseteq V'$, and it turns out it's a subspace as well, with dimension equal to the difference of the dimensions of $V$ and subspace $U$. This means that if the annihilator is the trivial space, then $U=V$, and if the annihilator is the entire dual space, $U=\set{0}$.

Let $V,W$ be finite dimensional and $T$ be a linear map from $V$ to $W$, then the dimension of the range of the dual map $T'$ equals the dimension of the range of $T$, and the range of the dual map equals the annihilator of the null space of $T$.

If $T$ is injective, then it has no null space, so the annihilator of $T$ is the entire dual space, which means the range of the dual map is the entire dual space, making it surjective.
# Matrix of Dual of Linear Map

The matrix of the dual map $T'$ is equal to the transpose of the matrix of $T$. In other words, $\mathcal M(T')=(\mathcal M(T))^T$. We can use this to prove that the column rank of a matrix equals its row rank by saying the column rank of $\mathcal M(T)$ equals the column rank of $\mathcal M(T')$.