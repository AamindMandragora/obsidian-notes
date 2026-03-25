# Invertible Linear Maps

A linear map $T\in\mathcal L(V, W)$ is called **invertible** if there exists some map $T^{-1}\in\mathcal L(W, V)$ such that $TT^{-1}=I_W$ and $T^{-1}T=I_V$. In this case, $T^{-1}$ is called the **inverse** of $T$. A map is invertible if and only if it's injective and surjective.

While we still need both conditions to prove invertibility for a map from an infinite-dimensional vector space to itself, it turns out that we only need either injectivity or surjectivity to prove the other two for maps between two finite-dimensional vector spaces of the same dimension.
# Isomorphic Vector Spaces

An **isomorphism** is an invertible linear map, and vector spaces between which exist an isomorphism are called **isomorphic**. For finite-dimensional vector spaces, an isomorphism exists if and only if they have the same dimension, in which case we can just create a $T$ mapping each basis vector in the first space to a basis vector in the second. This means that each finite-dimensional $V$ is isomorphic to $\mathbb F^n$ for $n=\dim V$.

We knew that $\mathcal M$ formed a linear map between $\mathcal L(V,W)$ and $\mathbb F^{m,n}$, and since their dimensions are equal, $\mathcal M$ is an isomorphism too.
# Linear Maps Thought of as Matrix Multiplication

The matrix of a vector $v$ is simply the column matrix where the $k\text{-th}$ entry is the coefficient of the $k\text{-th}$ basis vector in the linear combination equaling $v$. Therefore, elements of $V$ are simply $n$-by-$1$ matrices, and the function $\mathcal M$ that sends $v$ to its matrix is an isomorphism of $V$ onto $\mathbb F^{n,1}$. Additionally, the $k\text{-th}$ column of $\mathcal M(T)$ for linear transformation $T\in\mathcal L(V,W)$, denoted $\mathcal M(T)_{\cdot,k}$, equals $\mathcal M(Tv_k)$ where $v_k$ is the $k\text{-th}$ basis vector of $V$.

$\mathcal M(Tv)=\mathcal M(T)\mathcal M(v)$ shows that applying a linear map to a vector acts like matrix multiplication, and each $m$-by-$n$ matrix $A$ induces a linear map from $\mathbb F^{n,1}$ to $\mathbb F^{m,1}$. The specific $A$ will be determined by both the actual mapping and the choice of bases, but the properties of $A$ do not. For example, $\dim \text{range }T$ equals the column rank of $\mathcal M(T)$.
# Change of Basis

Usually, when mapping from a vector space to itself, we use the same basis for both the domain and the range.

A square matrix $A$ is **invertible** if there exists some square matrix $B$ such that $AB=BA=I$, the identity matrix. Notice that this definition forces $B$ to be the same size as $A$. In this case, we denote $B$ as $A^{-1}$, the inverse of $A$.

Our definition of matrix multiplication allows the matrix of product of linear maps to be the product of matrices of linear maps.

If we have two bases $u_1,\ldots,u_n$ and $v_1,\ldots,v_n$ of $V$, then the following two matrices are inverses of each other: $$\mathcal M(I,(u_1,\ldots,u_n),(v_1,\ldots,v_n))\text{ and }\mathcal M(I,(v_1,\ldots,v_n),(u_1,\ldots,u_n))$$If we then choose some linear transformation $T\in\mathcal L(V)$, then let $A$ be the matrix representation of $T$ with respect to the $u$ basis and $B$ be the matrix representation of $T$ with respect to the $v$ basis. Let $C$ and $C^{-1}$ be the two inverse matrices above, then $A=C^{-1}BC$.

We also have $\mathcal M(T^{-1})=\mathcal M(T)^{-1}$.