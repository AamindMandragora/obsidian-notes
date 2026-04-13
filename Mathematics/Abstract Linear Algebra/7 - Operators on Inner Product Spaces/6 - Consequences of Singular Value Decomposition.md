# Norms of Linear Maps

Let $s_1$ be the largest singular value of $T$, then $\|Tv\|\leq s_1\|v\|$. This means that we can define the **norm** of $T$ to be the maximum possible $\|Tv\|$ such that $\|v\|\leq 1$, which means it equals $s_1$. $\|T\|\geq 0$, $\|T\|=0$ iff $T=0$, norm distributes over scalar multiplication, and the triangle inequality holds: $\|S+T\|\leq\|S\|+\|T\|$. This definition also implies $\|T\|\|v\|\geq\|Tv\|$.

The quantity $\|S-T\|$ can be thought of as the distance between $S$ and $T$, and for any $c\in\mathbb F$, we can find some $S$ such that $S$ is invertible and $\|S-T\|\leq c$. Any linear map has the same norm as its adjoint.
# Approximation by Linear Maps with Lower-Dimensional Range

Let $T\in\mathcal L(V,W)$ and $s_1\geq\cdots\geq s_m$ are the positive singular values of $T$. Let $1\leq k\leq m$, then $\min\set{\lVert T-S\rVert :S\in\mathcal L(V,W),\dim\text{range } S\leq k}=s_{k+1}$. Furthermore, if $Tv=\sum s_j\langle v,e_j\rangle f_j$ and $T_k v=\sum^k s_k\langle v,e_j\rangle f_j$, then $\dim\text{range } T_k=k$ and $\|T-T_k\|=s_{k+1}$. This means that we can approximate the SVD of a linear map by taking the first $k$ singular values of $T$ and constructing a new linear map using those.
# Polar Decomposition

We know that every nonzero complex number can be written in the form $$z=\left(\frac{z}{|z|}\right)\sqrt{\overline zz}$$which implies that every operator $T$ can be written as a unitary operator $S$ times $\sqrt{T^*T}$. $S$ will satisfy $Sv=\sum\langle v,e_k\rangle f_k$.
# Operators Applied to Ellipsoids and Parallelepipeds

The **ball** in $V$ of radius $1$ centered at $0$ is defined by $B=\set{v\in V:\lVert v\rVert<1}$. The **ellipsoid** $E(s_1f_1,\ldots,s_nf_n)$ with **principal axes** $s_1f_1,\ldots,s_nf_n$ is defined by $\set{v\in V:\sum\frac{|\langle v,f_k\rangle|^2}{s_k^2}<1}$ for orthonormal basis $f_k$ and positive numbers $s_k$. Therefore, $E(f_1,\ldots,f_n)=B$.

For a function $T$ defined on $V$ and $\Omega\subseteq V$, define $T(\Omega)=\set{Tv:v\in\Omega}$. We can use this to show that every $T$ maps $B$ onto some ellipsoid in $V$ defined by $T$'s singular values. $T(E)$ is also an ellipsoid for every ellipsoid $E$.

We define a **parallelepiped** to be the set of the form $u+P(v_k)$ for some $u\in V$ and $P(v_k)=\set{a_kv_k:a_k\in(0,1)}$. The vectors $v_k$ are then called the **edges** of the parallelepiped. Then, $T(u+P(v_k))=Tu+TP(v_k)$. A **box** is a set of the form $u+P(r_ke_k)$ where $u\in V$, $r_k$ are positive numbers, $e_k$ is an orthonormal basis of $V$.

If the operator $T$ is invertible and has singular values $s_k$, then $T(u+P(r_ke_k))=Tu+TP(r_ks_kf_k)$.
# Volume via Singular Values

We define the **volume** of a box $u+P(r_ke_k)$ to be $\prod r_k$. The volume of a subset $\Omega$ of $V$ is approximately the sum of the volumes of a collection of disjoint boxes that approximate $\Omega$. The volume of $T(\Omega)$ equals the product of singular values of $T$ times the volume of $\Omega$.
# Properties of an Operator as Determined by Its Eigenvalues

A normal operator is invertible if its eigenvalues are nonzero, self-adjoint if they're real, skew if they're imaginary, an orthogonal projection if they're either zero or one, positive if they're nonnegative, unitary if they have absolute value one, and has norm less than one if they all have absolute value less than one.