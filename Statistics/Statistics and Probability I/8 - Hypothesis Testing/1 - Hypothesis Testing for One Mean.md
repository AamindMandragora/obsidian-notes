Previously, we've estimated points and constructed confidence intervals for parameters and proportions. Now, we are going to test claims on parameters. If we have some claim $H_1$, then let $H_0$ be the negation of that claim (which will always be a statement of equality), which we will call the **null hypothesis**. $H_1$ then becomes the alternative hypothesis. We will assume the null hypothesis, calculate statistics based on the data, and decide whether to reject or fail to reject (not accept) the null hypothesis.

We often partition the sample space based on the value of some **test statistic**, and will reject the null hypothesis if the test statistic is within some range with a defined upper or lower bound. This region is referred to as the **critical region** or **rejection region**.

There are two types of errors. Type I is when we mistakenly reject the null hypothesis, and Type II is when we mistakenly fail to reject the null hypothesis. Note that these errors are a consequence of the process and not an actual mistake within it. Let $\alpha$ be the probability of a Type I error, which turns out to be the level of significance. $\beta$ will be the probability of a Type II error.

The likeliness of a null hypothesis can be measured with a **p-value**, the probability of observing data at least as extreme as the observed sample given that $H_0$ is true. If $p<\alpha$, then we will reject $H_0$.

In summary, we can either compare our p-value to $\alpha$, our confidence interval to $H_0$, or our test statistic to the critical region to see if we should reject the null hypothesis.

For one mean tests, $H_0:\mu=\mu_0$, $H_1:\mu > / < / \neq \mu _0$, and the critical regions are $z\geq / \leq \pm z_\alpha$ or $|z|\geq z_{\alpha/2}$, where $Z=\frac{\overline Z-\mu_0}{\sigma/\sqrt n}$. We can replace $z$ with $t$ when we don't know the variance, where $T=\frac{\overline Z-\mu_0}{S/\sqrt n}$.