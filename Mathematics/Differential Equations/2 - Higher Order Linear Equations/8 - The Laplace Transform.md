The **Laplace transform** is defined as follows for all $f(t)$ defined for nonnegative $t$ that grows at most exponentially fast $$F(s)=\mathcal L(f)=\int_0^\infty e^{-st}f(t)dt$$The Laplace transform is invertible and derivatives transform nicely under it: $$\mathcal L(f')=s\mathcal L(f)-f(0)$$If we have the constant coefficient linear differential equation $$\mathbf My-f(t)$$then taking the Laplace transform will lead to an equation of the form $$P(s)Y(s)+Q(s)=F(s)$$where $P(s)$ is the characteristic polynomial, $Q(s)$ is a polynomial of degree $n$ or less that arises after applying the Laplace derivative identities, and $F(s)$ is the Laplace transform of $f(t)$. We can solve for $Y(s)$ to find $$Y(s)=\frac{F(s)}{P(s)}-\frac{Q(s)}{P(s)}$$the first term being the Laplace transform of the particular solution and the second term being the Laplace transform of the homogeneous solution.

The identity $$\mathcal L(t^ke^{at})=\frac{k!}{(s-a)^{k+1}}$$will be helpful to know.

| Function     | Laplace Transform    |
| ------------ | -------------------- |
| $f(t)$       | $F(s)$               |
| 1            | $\frac 1 s$          |
| $t^k$        | $\frac{k!}{s^{k+1}}$ |
| $\sin(bt)$   | $\frac{b}{b^2+s^2}$  |
| $\cos(bt)$   | $\frac{s}{b^2+s^2}$  |
| $e^{at}f(t)$ | $F(s-a)$             |
| $t^kf(t)$    | $(-1)^kF^{(k)}(s)$   |

The **Heaviside step function** is defined as $$H(t-a)=\begin{cases}0 & t<a\\1&t\geq a\end{cases}$$and represents a forcing or signal switched on at time $a$. The Laplace transform of $H(t-a)$ is $$\mathcal L(H(t-a))=\frac{1}{s}e^{-sa}$$and the Laplace transform of $f(t)H(t-a)$ is $$\mathcal L(f(t)H(t-a))=e^{-sa}\mathcal L(f(t+a))$$Notice that $\mathcal L(1)=\frac{1}{s}$.