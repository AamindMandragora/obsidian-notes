Sample spaces can either be discrete, where the number of outcomes are at most countably infinite, or continuous, where the number of outcomes are uncountable. For discrete variables, $P(X=x)=f(x)$, the pmf of the variable. However, the corresponding **probability density function** of a continuous variable is zero for all $x$ since the variable has an uncountably infinite number of outcomes. These such pdfs must satisfy $f(x)\geq 0$ for all $x\in S$, $\int_Sf(x)dx=1$, and for all intervals $(a,b)\subset S$, $P(a<x<b)=\int_a^bf(x)dx$.

Recall that the Fundamental Theorem of Calculus states that if $f(x)$ is continuous over $[a,b]$, and the function $F(x)=\int_a^xf(t)dt$, then $F'(x)=f(x)$ over the interval. We can use this theorem to find that the cumulative density function, $F(x)=P(X\leq x)=\int_{-\infty}^xf(t)dt$ for any $-\infty\leq x\leq\infty$, has a derivative equal to the pdf.

The pdf doesn't need to be continuous, but the cdf will be. We can interchange $<$ and $\leq$ when talking about continuous distributions as, since $f(x)=0$ for all $x$, including an extra point in the inequality doesn't change anything.

For continuous random variables, we have $\mu = E[X]=\int_{-\infty}^\infty xf(x)dx$ and $\sigma^2=Var[X]=E[(X-\mu)^2]=\int_{-\infty}^\infty(x-\mu)^2f(x)dx$.

The $p\text{th}$ percentile is a number $\pi_p$ such that $p=\int_{-\infty}^{\pi_p}f(x)dx=F(\pi_p)$.