# Products of Vector Spaces

For vector spaces $V_1,\ldots,V_m$ over $\mathbb F$, we define their **product** by $V_1\times\cdots\times V_m = \set{(v_1,\ldots,v_m):v_1\in V_1,\ldots,v_m\in V_m}$, which is also a vector space over $\mathbb F$. The addition and scalar multiplication operations follow naturally, and the dimension of the product is the sum of the dimensions. This means that, while $\mathbb R^2\times \mathbb R^3\neq \mathbb R^5$, the two spaces are isomorphic.

If we define a linear map $\Gamma$ from the product of vector spaces to the sum where $\Gamma(v_1,\ldots,v_m)=v_1+\cdots+v_m$, the sum of vector spaces is a direct sum if and only if $\Gamma$ is injective (or equivalently, invertible, as it's surjective by definition). This also means that a sum is a direct sum if and only if the dimension of the sum is the sum of the dimensions.
# Quotient Spaces

Let $v\in V$ and $U\subseteq V$, then we define $v+U=\set{v+u:u\in U}$, which is called a **translate** of $U$. We define the set of all translates of a subspace $U$ to be $V/U=\set{v+U:v\in V}$, the **quotient space**.

We need to make $V/U$ a vector space, which we can do by considering $v,w\in V$. If $v-w\in U$, then $v+u=(w-w)+(v+u)=w+((v-w)+u)\in w+U$, which leads to $v+U=w+U$. Then, their intersection is non-empty. If it was nonempty, then that implies $v+u_1=w+u_2$ for some $u_1,u_2$, which means $v-w=u_2-u_1\in U$ as desired.

We can now define addition and scalar multiplication on $V/U$ by: $$\begin{align}(v+U)+(w+U)&=(v+w)+U \\ \lambda(v+U)&=(\lambda v)+U\end{align}$$for all $v,w\in V$ and $\lambda\in\mathbb F$.

We can now define the quotient map $\pi: V\to V/U$ by $\pi(v)=v+U$. Even though $\pi$ depends on both $U$ and $V$, we omit them from the notation as they are clear from the context.

Note that $\pi$ sends to $0+U$ only when $v\in U$, so the null space of $V$ is $U$ and $\dim V/U=\dim V-\dim U$.

If we have a linear map $T: V\to W$, we can mod out by the nullspace to get $\widetilde T: V/\text{null }T\to W$ where $\widetilde T(v+\text{null }T)=Tv$. Notice that $\widetilde T\circ\pi= T$, for $\pi$ defined on the subspace $\text{null }T$. This means that $\widetilde T$ is injective and preserves the range of $T$, so $\widetilde T$ is an isomorphism from $V/\text{null }T\to\text{range }T$. This result is called the first isomorphism theorem.