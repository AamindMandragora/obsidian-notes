The even extension of a function defined on $(0,L)$ results in a function with period $2L$ and will only require the cosine terms of the Fourier series as it is even. Also, we have to rewrite the integrals to be in terms of the original interval, which gives us $$A_0=\frac{1}{L}\int_0^Lf(x)dx,~ A_n=\frac{2}{L}\int_0^Lf(x)\cos\left(\frac{n\pi x}{L}\right)dx$$the Fourier cosine series.

The odd reflection of a function, for the same reason as above, will require only sine terms, with coefficient integrals $$B_n=\frac{2}{L}\int_0^Lf(x)\sin\left(\frac{n\pi x}{L}\right)dx$$Note that the Fourier sine series does not include the constant coefficient $A_0$.

The **Gibbs phenomenon** is the typical oscillation associated with jump discontinuities, which happens often with the Fourier sine series.