# Orthonormal Lists and the Gram-Schmidt Procedure

A list of vectors is called **orthonormal** if each vector in the list has norm 1 and is orthogonal to all the other vectors in the list. Let $e_1,\ldots,e_m$ be an orthonormal list of vectors in $V$, then $\|a_1e_1+\cdots+a_me_m\|^2=|a_1|^2+\cdots+|a_m|^2$ and the list is linearly independent. Bessel's inequality states that for some $v$, $|\langle v,e_1\rangle|^2+\cdots+|\langle v,e_m\rangle|^2\leq \|v\|^2$.

An **orthonormal basis** of $V$ is an orthonormal list of vectors in $V$ that also forms a basis, and any orthonormal list of the right length is a basis. We can construct a linear combination of basis vectors for any vector in $V$ by taking $a_k=\langle v,e_k\rangle$. This also means that $\|v\|^2=|\langle v,e_1\rangle|+\cdots+|\langle v,e_n\rangle|^2$ and $\langle u,v\rangle=\sum{k=1}^n\langle u,e_k\rangle\overline{\langle v,e_k\rangle}$.

The **Gram-Schmidt procedure** is a way to turn a linearly independent list $v_1,\ldots,v_m$ into an equivalent orthonormal list. If we take $f_1=v_1$ and $f_k=v_k-\sum_{i=1}^{k-1}\frac{\langle v_k,f_i\rangle}{\|f_i\|^2}f_i$, we can define $e_k=f_k/\|f_k\|$ to get an orthonormal list of vectors in $V$. This procedure implies that every finite-dimensional inner product space has an orthonormal basis and every orthonormal list of vectors can be extended to an orthonormal basis.

An operator $T$ on $V$ only has an upper-triangular matrix with respect to some orthonormal basis iff the minimal polynomial is of the form $(z-\lambda_1)\cdots(z-\lambda_m)$. This implies **Schur's theorem**, which states that every operator on a finite-dimensional complex inner product space has an upper-triangular matrix with respect to some orthonormal basis.
# Linear Functionals on Inner Product Spaces

Recall that a **linear functional** on $V$ is a linear map from $V$ to $\mathbb F$, and the **dual space** $V'$ is the vector space of all such linear functionals, $\mathcal L(V,\mathbb F)$.

If we fix some $v$, then $\varphi(u)\mapsto\langle u,v\rangle$ is a linear functional. In fact, every linear functional is of this form. The **Riesz representation theorem** states that for a linear functional $\varphi$, there's a unique $v\in V$ such that $\varphi(u)=\langle u,v\rangle$ for every $u\in V$. More specifically, $v=\sum_{k=1}^n\overline{\varphi(e_k)}e_k$ for some orthonormal basis $e_k$.