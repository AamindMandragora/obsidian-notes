# Definition and Examples of Linear Maps

A **linear map** (or linear transformation) from $V$ to $W$ is a function $T: V\to W$ such that $T(u+v)=Tu+Tv$ and $T(\lambda v)=\lambda(Tv)$ for all $u,v\in V$ and $\lambda\in\mathbb F$. The set of all linear maps from $V$ to $W$ is denoted by $\mathcal L(V,W)$, and $\mathcal L(V)$ is defined to equal $\mathcal L(V,V)$.

Some examples of common linear maps are the zero map ($Tv=0$), the identity map ($Tv=v$), differentiation ($Tv=v'$) for polynomial $v$, integration ($Tv=\int_0^1 v$) for polynomial $v$, and a map from $\mathbb F^n$ to $\mathbb F^m$: $$T(x_1,\ldots,x_n)=(A_{1,1}x_1+\cdots+A_{1,n}x_n,\ldots,A_{m,1}x_1+\cdots+A_{m,n}x_n)$$for $A_{j,k}\in\mathbb F$.

The **linear map lemma** states that, for bases $v_1,\ldots,v_n$ of $V$ and $w_1,\ldots,w_n$ of $W$, there exists a unique linear map $T: V\to W$ such that $Tv_k=w_k$. The proof uses the homogeneity over addition and scalar multiplication property of linear maps as well as the basis properties.
# Algebraic Operations on $\mathcal L(V, W)$

Defining $(S+T)v=Sv+Tv$ and $(\lambda T)v=\lambda(Tv)$ for linear maps $S,T\in\mathcal L(V,W)$ turns it into a vector space with the zero map as an additive identity. We can also define the product of linear maps $ST$ by $(ST)u=S(Tu)$ for all $u\in U$, given that $S\in\mathcal L(V,W)$ and $T\in\mathcal (U,V)$. Products of linear maps satisfy associativity, distributivity over addition, and has the identity map as an identity. However, it isn't commutative.

Any linear map $T$ must satisfy $T(0)=0$ by $T(0)=T(0+0)=T(0)+T(0)$.