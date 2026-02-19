# Basic Terminology

A **differential equation** is any relationship between a function $y$ and its derivatives up to some order. An **ordinary** differential equation is when $y$ is a function of a single variable (denoted as $t$ or $x$). A **partial** differential equation is when $y$ is a function of multiple variables.

The **order** of a differential equation is the order of the highest derivative included in the equation. To find a unique solution, it's also necessary to specify initial conditions equal to the order $n$ of the differential equation, usually the values of the function and of the first $n-1$ derivatives at some initial point $t_0$. Differential equations of that kind are called **initial value problems**.

If instead the values of the function at $n$ different points are given, its called a **boundary value problem**.

An equation is **linear** if there are no powers of or products between $y$ and its derivatives, and can be written in the form $$a_n(t)y^{(n)}+a_{n-1}(t)y^{(n-1)}+\cdots+a_0(t)y=f(t)$$In the case where $f(t)=0$, the equation is said to be **homogeneous**.
# Examples of Differential Equations

Differential equations show up in several real-world contexts where the rate of change of some quantity depends on the quantity itself, usually taking the form of some physical law.

In the case of **compound interest**, if a savings account holding $P_i$ during year $i$ accrues interest compounded yearly at a rate $r$, the account can be modeled by $$P_{i}=(1+r)P_{i-1}$$with solution $$P_i=(1+r)^iP_0$$If instead we compound twice a year, then the account can be modified by $$P_i=\left(1+\frac{r}{2}\right)^{2i}P_0$$which is in fact a larger amount than the account compounding yearly. Taking it one step further, if we compound $n$ times a year, our account will satisfy the following: $$P_i=\left(1+\frac{r}{n}\right)^{ni}P_0$$If we take the limit as $n$ tends to infinity, $P_i$ becomes a continuous function $P(t)$. Using the discrete approximation for the derivative, and letting $\Delta t=\frac{1}{n}$, we get $$\frac{\Delta P}{\Delta t}=\frac{P_0(1+r/n)-P_0}{1/n}$$and after some algebra, we get the differential equation $$\frac{dP}{dt}=rP$$with solution $P(t)=P_0e^{rt}$.

**Newton's Law of Cooling** states that the rate of change of the temperature of a body is proportional with constant $-k$ to the difference in temperature between the body $T$ and the surrounding environment $T_0$, yielding the differential equation $$\frac{dT}{dt}=-k(T-T_0)$$
**Newton's Law of Motion** states that, if $x(t)$ represents the position of a particle, using his second law of motion ($F=ma=m\frac{d^2x}{dt^2}$), assuming a conservative force (work independent of path), then the force is equal to the negative derivative of the potential energy $V(x(t))$, creating the second order (usually) nonlinear differential equation $$m\frac{d^2x}{dt^2}=-\nabla_xV(x(t))$$
A simple way to model the **growth of a raindrop** is to set proportional the rate of change of the volume to the surface area of the drop, giving the differential equation $$\frac{dV}{dt}=\frac{d}{dt}\left(\frac{4\pi}{3}r(t)^3\right)=4\pi r^2\frac{dr}{dt}=k(4\pi r^2)$$which means that the rate of change of the radius is constant.