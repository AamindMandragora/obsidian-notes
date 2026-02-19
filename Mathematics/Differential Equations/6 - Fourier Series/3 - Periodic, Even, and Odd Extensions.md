We can use variations of the Fourier series to extend functions defined on a finite domain to the whole line.
# The Periodic Extension

The simplest way to extend a function $f(x)$ defined on the domain $(0, L)$ is periodically, defining $f(x+L)=f(x)$ on the whole line. Graphically, we take the graph of the function on the original interval and repeat it for the rest of them. This procedure may introduce jump discontinuities at the ends of each interval ($x=nL$).
# The Even Extension

Recall that even functions satisfy $f(-x)=f(x)$ and is invariant under reflection across the $y\text{-axis}$. We can reflect the graph of $f(x)$ across the boundary points $x=nL$ to extend it across the whole line. Algebraically, for any interval $((n-1)L, nL)$, we define $f(nL-x)=f(x)$. If $f(x)$ was continuous originally, the even extension will also be continuous (but likely won't have a continuous derivative). Also, the even extension has period $2L$.
# The Odd Reflection

Recall that odd functions satisfy $f(-x)=-f(x)$ and is invariant under reflection across the $y\text{-axis}$ followed by reflection across the $x\text{-axis}$. We can reflect the graph of $f(x)$ across the boundary points $nL$ then across the $x\text{-axis}$ to extend it across the whole line. The odd reflection also has a period of $2L$ and will typically have jump discontinuities.