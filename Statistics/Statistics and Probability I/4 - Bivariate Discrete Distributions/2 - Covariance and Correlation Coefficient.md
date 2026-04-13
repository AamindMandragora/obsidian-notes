Recall that, for a bivariate distribution, $\mu_X=E(X)$, $\mu_Y=E(Y)$, $\sigma^2_X=E[(X-\mu_X^2)]$, and $\sigma^2_Y=E[(Y-\mu_Y)^2]$. We define the **covariance** of $X$ and $Y$ to be $$\begin{align}\text{Cov}[X,Y]&=E[(X-\mu_x)(Y-\mu_Y)]=\sigma_{XY} \\ &= E(XY - \mu_XY-\mu_YX+\mu_X\mu_Y) \\ &= E(XY)-\mu_XE(Y)-\mu_YE(X)+\mu_x\mu_y \\ &= E(XY)-\mu_X\mu_y \\ &=E(XY)-E(X)E(Y)\end{align}$$Covariance measures the direction of the linear relationship between the two variables. In other words, it represents the slope of the second variable relative to the first. To normalize it, we can divide by $\sigma_X\sigma_Y$. This gives us $\rho=\text{Cor}(X,Y)$, the correlation coefficient.

To calculate $E[X]$, we can use the following methods:

Univariate Discrete: $\sum_xxf_X(x)$
Bivariate Discrete: $\sum_x\sum_yxf(x,y)$
Univariate Continuous: $\int_xxf(x)dx$
Bivariate Continuous: $\int_x\int_yxf(x,y)dydx$

To calculate $E[XY]$, we can use the following methods:

Bivariate Discrete: $\sum_x\sum_yxyf(x,y)$
Bivariate Continuous: $\int_x\int_yxyf(x,y)dydx$