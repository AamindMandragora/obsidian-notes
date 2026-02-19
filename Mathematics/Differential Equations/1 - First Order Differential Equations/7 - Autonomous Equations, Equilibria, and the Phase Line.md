**First order autonomous equations** are those that don't contain the independent variable. They are always separable and solvable, but we can obtain qualitative information on the solution through other means.

An **equilibrium** is any constant solution to $\frac{dy}{dt}=f(y)$ and therefore a solution to $f(y_0)=0$. If we draw the slope field for an autonomous equation, there won't be any change on the $t$-axis, so we can compress it into a **phase line** with only one $y$-axis. Along the axis, we put upwards arrows where the slope is positive and downwards arrows where the slope is negative.

An equilibrium is **stable** if nearby initial points converge to it, and **unstable** otherwise. Let $y^*$ be an equilibrium, then the **Hartman-Grobman theorem** states that if $f'(y^*)>0$ then it is unstable and if $f'(y^*)<0$ then it is stable. When it's equal to zero, then further analysis is required.

For logistic growth, the equilibria are $P=0$ (unstable) and $P=P_0$ (stable), the carrying capacity.

A differential equation with **bistability** has three equilibria, the outer two being stable and the middle one being unstable. Circuits with bistable behavior are called **flip-flops**, as they remain in one of the two stable states until an external force changes it.

A population of fish can be easily represented using the logistic model, but to model the effect of fishing on that population, we can subtract a constant term to create the following model $$\frac{dP}{dt}=kP(P_0-P)-h$$If the fishing constant $h$ can be set by something like a policy decision, it'd be best to understand how it affects the fish population and how large we can set it before it fails to be sustainable. The right side is a quadratic, and solving gives the equilibria $$P=\frac{1}{2}(P_0\pm\sqrt{P_0^2-4h/k})$$This means that if the discriminant is positive ($\sqrt{h/k}<P_0/2$) then there are two positive real roots, the lower being unstable and the upper being stable. If the discriminant is negative, then there aren't any real roots or equilibria, which forces the population to crash rapidly.

We can create a diagram plotting the equilibria against $h$ to see its effects on the number and existence of roots, called a **bifurcation diagram**. In the case of fishing, there's a **saddle-node** bifurcation where the two equilibria combine then disappear.