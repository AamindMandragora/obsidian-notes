# Orthogonal Complements

If $U$ is a subset of $V$, then the **orthogonal complement** $U^\perp$ is the set of all vectors in $V$ that are orthogonal to every vector in $U$. In other words, $$U^\perp=\set{v\in V:\langle u, v\rangle=0: u\in U}$$Then, $U^\perp$ is a subspace of $V$, $\set{0}^\perp=V$, $V^\perp=\set{0}$, $U\cap U^\perp\subseteq \set{0}$, and if $G\subseteq H$, then $H^\perp\subseteq G^\perp$. 

If $U$ is a subspace, then $U\oplus U^\perp=V$, and $\dim U^\perp=\dim V-\dim U$. This implies that every vector has an **orthogonal decomposition** into the sum of a vector in $U$ and a vector in $U^\perp$. We also know that $(U^\perp)^\perp=U$.

Define the orthogonal projection $P_U$ of some vector space $V$ onto a subspace $U$ by $P_Uv=u$, where $v=u+w$ for $u\in U$ and $w\in U^\perp$. Then, $P_U$ is an operator on $V$ where $P_Uu=u$ and $P_Uw=0$, which means $U$ and $U^\perp$ are invariant under $P_U$. We also know that $v-P_Uv\in U^\perp$, $P_U^2=P_U$, and $\|P_Uv\|\leq\|v\|$.

Recall the Riesz representation theorem from last section. We can use the converse to define, for each $v\in V$, $\varphi_v(u)=\langle u,v\rangle$ for each $u\in V$, which makes $v\mapsto \varphi_v$ a bijection from $V$ onto $V'$. This mapping is only linear if the field is $\mathbb R$.
# Minimization Problems

Given a subspace $U$ of $V$ and a point $v\in V$, find a point $u\in U$ such that $\|v-u\|$ is as small as possible. Then, $u=P_Uv$ is the unique solution of this minimization problem. We can prove this by: $$\begin{align}\|v-P_Uv\|^2&\leq\|v-P_Uv\|^2+\|P_Uv-u\|^2 \\ &=\|(v-P_Uv)+(P_Uv-u)\|^2 \\ &=\|v-u\|^2\end{align}$$
# Pseudoinverse

Let $T\in\mathcal L(V, W)$ and $w\in W$, then how can we find a $v\in V$ such that $Tv=w$? If $T$ is invertible, then $v=T^{-1}w$, but if not, there may be any number of vectors that can be $v$. If the equation has no solutions, perhaps we can find a vector that's "close enough". More rigorously, find some $v$ to minimize $\|Tv-w\|$. We can restrict $T$'s domain to the orthogonal complement of the null space of $T$ to get a bijective map onto the range. 

Let's define the pseudoinverse $T^\dagger$ such that $T^\dagger w=(T|_{(\text{null }T)^\perp})^{-1}P_{\text{range }T}w$ for each $w\in W$. We take $w$ and projecting it onto the range of $T$, then feeding it back through the inverse of the bijection to get our desired $v$. We can see that $T^\dagger =T^{-1}$ if $T$ is invertible, $TT^\dagger$ is the projection of $W$ onto the range, and $T^\dagger T$ is the projection of $V$ onto the orthogonal complement of the null space.

Taking $v=T^\dagger w$ makes $Tv$ as close to $w$ as possible, making it the **best fit**. $T^\dagger w$ also has the smallest norm.