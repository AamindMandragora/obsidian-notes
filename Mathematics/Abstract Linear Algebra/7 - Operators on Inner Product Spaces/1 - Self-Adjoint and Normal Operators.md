# Adjoints

Let $T\in\mathcal L(V,W)$, then we define the **adjoint** of $T$, $T^*$, to be a function $W\to V$ such that $\langle Tv,w\rangle =\langle v, T^*w\rangle$. If we consider the linear functional $v\mapsto\langle Tv,w\rangle$, the Riesz representation theorem states that there exists some vector $u$ such that $\langle Tv,w\rangle=\langle v,u\rangle$. We then simply define $T^*w=u$. The adjoint is a linear map as long as $T$ is, and the adjoint operation distributes across addition, conjugate scaling, and commuted multiplication. $(T^*)^*=T$ and $I^*=I$, and $(T^{-1})^*=(T^*)^{-1}$. We also know that the null space of $T$ is the orthogonal complement of the range of $T^*$, and the range of $T$ is the orthogonal complement of the null space of $T^*$. 

We define the **conjugate transpose** of an $m$-by-$n$ matrix $A$ to be the $n$-by-$m$ matrix $A^*$ defined by $(A^*)_{j,k}=\overline{A_{k,j}}$. For $\mathbb R$, $A^*=A^T$. Let some $e_k$ be an orthonormal basis of $V$ and $f_k$ an orthonormal basis of $W$, then $\mathcal M(T^*, (f_k), (e_k))$ is the conjugate transpose of $\mathcal M(T,(e_k),(f_k))$. This means that $\mathcal M(T^*)=\mathcal M(T)^*$.

We derived a bijection between $V$ and $V'$ using the map $v\mapsto \varphi_v$. Under this bijection, $U^\perp$ corresponds to the annihilator $U^0$ of $U$. Also, if $T:V\to W$ is a linear map, we can use the bijection to link $T^*:W\to V$ and $T':W'\to V'$.
# Self-Adjoint Operators

An operator $T$ on $V$ is **self-adjoint** if $T=T^*$, which only happens iff $\mathcal M(T,(e_k))=\mathcal M(T,(e_k))^*$ for some orthonormal basis $e_k$. The adjoint plays a similar role to the complex conjugate in $\mathbb C$, so a self-adjoint operator is similar to a real number. Every eigenvalue of a self-adjoint operator is therefore real. 

For complex vector spaces, $Tv$ is orthogonal to $v$ for all $v$ iff $T=0$. Also in complex vector spaces, $T$ is self-adjoint iff $\langle Tv,v\rangle\in\mathbb R$. For all vector spaces, self-adjoint $T$ satisfies $\langle Tv, v\rangle=0$ iff $T=0$.
# Normal Operators

An operator on an inner product space is **normal** if it commutes with its adjoint. That is, $TT^*=T^*T$. Every self-adjoint operator is normal. If $T$ is normal, $\|Tv\|=\|T^*v\|$, which means the null spaces and ranges of $T$ and $T^*$ are the same, and the null space and range have a direct sum of $V$. For every $\lambda\in\mathbb F$, $T-\lambda I$ is normal, and $Tv=\lambda v$ iff $T^*v=\overline\lambda v$.

If $T$ is normal, eigenvectors of $T$ corresponding to distinct eigenvalues are orthogonal, and $T$ is normal iff there exist commuting self-adjoint $A,B$ such that $T=A+iB$.