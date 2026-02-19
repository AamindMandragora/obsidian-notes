A **Bernoulli experiment** is a random experiment where the outcome can be classified as one of two mutually exclusive ways, and is called a **Bernoulli trial** when performed multiple independent times, so the probability of success $p$ stays constant.

If random variable $X$ has a Bernoulli distribution, then $f(x)=p^x(1-p)^{1-x}$ for $x\in\set{0,1}$, $E[X]=\sum_{x=0}^1f(x)=0(1-p)+1(p)=p$, $Var[x]=\sum_{x=0}^1(x-p)^2f(x)=p(1-p)$, and $SD[X]=\sqrt{p(1-p)}$.

An observed sequence of $n$ Bernoulli trials can be written as a vector of length $n$ with elements in $\set{0,1}$ which is called a **random sample** of size $n$ from a Bernoulli distribution ($X_i$ denoting the $i^\text{th}$ trial).

When we're interested in the total number of successes but not the order of occurrence, then we can let $X$ be the number of successes in $n$ Bernoulli trials, and $X$ has a **binomial distribution** with parameters $n, p$.

In other words, $X$ is a binomial random variable if $n$ independent Bernoulli trials were performed and $X$ is the number of successes in $n$ trials. $X\sim Binomial(n,p)$ means $f(x)=\binom{n}{x}p^x(1-p)^{n-x}$ for $x\in\set{0,1,\ldots,n}$, $E[X]=np$, and $Var[X]=np(1-p)$.

Say we observe a sequence of independent Bernoulli trials until the first success occurs, then if $X$ is the number of trials needed to observe that success, then $X$ follows a **geometric distribution** with parameter $p$.

$X\sim Geom(p)$ means that $f(x)=p(1-p)^{x-1}$. $E[x]=\frac{1}{p}$, and $Var[X]=\frac{1-p}{p^2}$. We also know that $P(X\leq k)=1-P(X>k)$, where $$P(X>k)=\sum_{x=k+1}^\infty(1-p)^{x-1}p=\frac{(1-p)^kp}{1-(1-p)}=(1-p)^k$$Recall that the cdf $F(k)$ equals $P(X\leq K)=1-(1-p)^k$.

More generally, suppose we observe a sequence of independent Bernoulli trials until the $r^\text{th}$ success occurs, then if $X$ is the number of trials needed to observe that, then $X% follows a **negative binomial** distribution with parameters $r,p$.

Then, $f(x)=\binom{x-1}{r-1}p^r(1-p)^{x-r}$ for $x\geq r$, $E[X]=\frac{r}{p}$, and $Var[X]=\frac{r(1-p)}{p^2}$.