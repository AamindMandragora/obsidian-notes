# Inner Products

We define the **norm** of $x=(x_1,\ldots,x_n)\in\mathbb R^n$ by $\|x\|=\sqrt{x_1^2+\cdots+x_n^2}$, which isn't linear until we introduce the dot product: $x\cdot y=x_1y_1+\cdots+x_ny_n$. We know $x\cdot x=\|x\|^2$, $x\cdot x\geq 0$, $f(x)=x\cdot y$ for fixed $y$ is linear, and $x\cdot y = y\cdot x$.

The norm of $y=(y_1,\ldots,y_n)\in\mathbb C^n$ is defined by $\|y\|=\sqrt{|y_1|^2+\cdots+|y_n|^2}$, which means $\|y\|^2=y_1\overline{y_1}+\cdots+y_n\overline{y_n}$, and the dot product is defined accordingly. Note that $\lambda\in\mathbb C\geq 0$ means $\lambda$ is real and nonnegative.

We define an inner product (a generalization of the dot product) by a function that takes each ordered pair $(u,v)\mapsto \left<u,v\right>\in\mathbb F$ that is nonnegative for all $(v,v)$, equals zero iff $v=0$, has additivity and homogeneity in the first slot, and has conjugate symmetry.

An **inner product space** is a vector space $V$ along with an inner product on $V$, like $\mathbb F^n$ with the Euclidean inner product. We can derive additivity and conjugate homogeneity in the second slot, and that $\left<0,v\right>=\left<v,0\right>=0$.
# Norms

The **norm** of $v$ is defined by $\|v\|=\sqrt{\left<v,v\right>}$, equals zero iff $v=0$, and satisfies $\|\lambda v\|=|\lambda|\|v\|$. 

Two vectors are called **orthogonal** if $\left<u,v\right>=0$. The order of the vectors doesn't matter. Then, $0$ is orthogonal to every vector and is the only vector orthogonal to itself.

The Pythagorean theorem states that if $u$ and $v$ are orthogonal, then $\|u+v\|^2=\|u\|^2+\|v\|^2$.

If we had some vector $u$ and we wanted to decompose it into a multiple $c$ of a scalar vector $v$ plus a vector $w$ orthogonal to $v$, we know that we want $u=cv+(u-cv)$, which means $0=\left<u-cv,v\right>=\left<u,v\right>-c\|v\|^2$, so $c=\left<u,v\right>/\|v\|^2$. Plugging this in gives us an **orthogonal decomposition** of $u$. We can use this to prove the Cauchy-Schwarz inequality: $|\langle u,v\rangle|\leq\|u\|\|v\|$. It's an equality iff $u$ is a scalar multiple of $v$.

The triangle inequality states that $\|v+v\|\leq\|u\|+\|v\|$, and is an equality iff $u$ is a nonnegative real multiple of $v$. The parallelogram equality states that $\|u+v\|^2+\|u-v\|^2=2(\|u\|^2+\|v\|^2)$. It's named like that because a geometric interpretation is that the sum of the squared lengths of the diagonals of a parallelogram is the sum of the squared lengths of the four sides.