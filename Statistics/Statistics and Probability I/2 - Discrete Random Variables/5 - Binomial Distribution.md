A **Bernoulli experiment** is a random experiment where the outcome can be classified as one of two mutually exclusive ways, and is called a **Bernoulli trial** when performed multiple independent times, so the probability of success $p$ stays constant.

If random variable $X$ has a Bernoulli distribution, then $f(x)=p^x(1-p)^{1-x}$ for $x\in\set{0,1}$, $E[X]=\sum_{x=0}^1f(x)=0(1-p)+1(p)=p$, $Var[x]=\sum_{x=0}^1(x-p)^2f(x)=p(1-p)$, and $SD[X]=\sqrt{p(1-p)}$.

An observed sequence of $n$ Bernoulli trials can be written as a vector of length $n$ with elements in $\set{0,1}$ which is called a **random sample** of size $n$ from a Bernoulli distribution ($X_i$ denoting the $i^\text{th}$ trial).

When we're interested in the total number of successes but not the order of occurrence, then we can let $X$ be the number of successes in $n$ Bernoulli trials, and $X$ has a **binomial distribution** with parameters $n, p$.

In other words, $X$ is a binomial random variable if $n$ independent Bernoulli trials were performed and $X$ is the number of successes in $n$ trials. $X\sim \text{Binomial}(n,p)$ means $f(x)=\binom{n}{x}p^x(1-p)^{n-x}$ for $x\in\set{0,1,\ldots,n}$, $E[X]=np$, and $Var[X]=np(1-p)$.