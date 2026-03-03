Say we observe a sequence of independent Bernoulli trials until the first success occurs, then if $X$ is the number of trials needed to observe that success, then $X$ follows a **geometric distribution** with parameter $p$.

$X\sim \text{Geom}(p)$ means that $f(x)=p(1-p)^{x-1}$. $E[x]=\frac{1}{p}$, and $Var[X]=\frac{1-p}{p^2}$. We also know that $P(X\leq k)=1-P(X>k)$, where $$P(X>k)=\sum_{x=k+1}^\infty(1-p)^{x-1}p=\frac{(1-p)^kp}{1-(1-p)}=(1-p)^k$$Recall that the cdf $F(k)$ equals $P(X\leq K)=1-(1-p)^k$.

More generally, suppose we observe a sequence of independent Bernoulli trials until the $r^\text{th}$ success occurs, then if $X$ is the number of trials needed to observe that, then $X% follows a **negative binomial** distribution with parameters $r,p$.

Then, $f(x)=\binom{x-1}{r-1}p^r(1-p)^{x-r}$ for $x\geq r$, $E[X]=\frac{r}{p}$, and $Var[X]=\frac{r(1-p)}{p^2}$.