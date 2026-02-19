We can always solve differential equations in terms of trigonometric functions, exponentials, and polynomials when the coefficients are all constant $$y^{(n)}+p_{n-1}y^{(n-1)}+\cdots+p_1y'+p_0y=0$$To begin with, we look for solutions of the form $y=e^{rt}$. Substituting that into the ODE gives us $$r^n+p_{n-1}r^{n-1}+\cdots+p_1r+p_0=0$$which is called the **characteristic polynomial**. Finding its $n$ roots gives us $n$ solutions to the ODE.
# Real Distinct Roots

If the $n$ roots are distinct and real, then the corresponding solutions are all linearly independent, the Wronskian is guaranteed to be nonzero, and their linear combination forms the general solution.
# Complex Distinct Roots

If there are complex roots $r_1=\mu+i\sigma, r_2=\mu-i\sigma$, then we know by Euler that $e^{r_1t}=e^{\mu t}(\cos\sigma t+i\sin\sigma t)$ and $e^{r_2t}=e^{\mu t}(\cos\sigma t-i\sin\sigma t)$, from which we can obtain the two solutions $$y_1(t)=e^{\mu t}\cos\sigma t, y_2(t)=e^{\mu t}\sin\sigma t$$We can choose whether to use the complex exponential or the real trigonometric functions (often preferred).

The differential equation $$my''+ky=0$$is an idealized equation for the simple harmonic oscillator with roots $\pm i\sqrt{k/m}$. IN engineering (especially electrical engineering) it's standard to work with complex valued solutions, but we can always convert them into their real trigonometric counterparts.
# Multiple Roots

When the characteristic equation has roots $r$ of multiplicity $k$, the differential equation has $k$ linearly independent solutions $e^{rt}, te^{rt}, \ldots, t^{k-1}e^{rt}$.