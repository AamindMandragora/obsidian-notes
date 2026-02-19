# Linear Independence and the Wronskian

If we have solutions $y_1(t),y_2(t),\ldots,y_m(t)$ to some linear homogeneous differential equation, then any linear combination of them is also a solution. If that differential equation has order $m$, then a linear combination of those linearly independent solutions is the **general solution**. A set of functions is linearly independent if and only if the only linear combination of them that equals zero is when each coefficient is zero.

We also know that for $n$ linear equations in $n$ unknowns, a unique solution exists if and only if the determinant is nonzero. This determinant takes the following form: $$W(y_1,y_2,\ldots,y_n)(t)=\begin{vmatrix}y_1(t)&y_2(t)&\ldots&y_n(t) \\ y'_1(t)&y'_2(t)&\ldots&y'_n(t) \\ \vdots&\vdots&\ddots&\vdots \\ y_1^{(n-1)}(t)&y_2^{(n-1)}(t)&\ldots&y_n^{(n-1)}(t) \end{vmatrix}$$and is called the **Wronskian**.

If we have $n$ solutions to an $n\text{th}$ order linear homogeneous differential equation where the coefficients are continuous in a neighborhood of $t_0$ and the Wronskian is nonzero then any solution to the corresponding initial value problem can be written as a linear combination of the $n$ solutions.

Abel gives us that if we have $n$ solutions to the linear homogeneous differential equation, then the Wronskian is a solution to the first order homogeneous linear differential equation $$W'(t)+a_{n-1}(t)W(t)=0$$A corollary of this is that $$W(t)=W(t_0)e^{\int_{t_0}^ta_{n-1}(s)ds}$$so the Wronskian is zero if and only if $W(t_0)=0$.