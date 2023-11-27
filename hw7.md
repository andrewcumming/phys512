# Homework 7

Due on Tuesday Dec 5 by midnight.

1. **Zero-padding the cross-correlation function**


The cross-correlation of two functions $f(x)$ and $g(x)$ is defined as

$$(f\star g)(y) = \int f(x) g(x+y) dx.$$

Similarly to the convolution, it is straightforward to show that this can be evaluated using Fourier transforms as

$$\mathrm{FT}(f\star g) = \mathrm{FT}(f) * \overline{\mathrm{FT}(g)},$$

where FT indicates Fourier transform and the overbar indicates the complex conjugate.

(a) Imagine that we have the values of two functions $f$ and $g$ defined on a grid in $x$ with $n$ grid points. The integral defining the cross-correlation becomes a sum in this discrete case. Show that there are $2n-1$ possible values of $y$ at which $f\star g$ can be evaluated. Write a function that evaluates the sum and returns the vector containing the $2n-1$ values of $f\star g$. 

(b) Next implement the cross-correlation using the Fourier transform (using `numpy.fft`). In order to return $2n-1$ values, you will need to zero-pad (add zeros onto the end) the $f$ and $g$ vectors before you Fourier transform them. You can do this by giving `numpy.fft.fft` the parameter `n` that specifies the length. Compare the answer with part (a) -- as a specific example, take $f$ and $g$ to be identical Gaussians displaced from each other by differing amounts along the horizontal axis. 

(c) Try part (b) without zero-padding the DFTs. What difference does it make? What part of the cross-correlation function is computed in this case?

2. **Diffusion again(!) but this time with Chebyshev polynomials**

Write a code that solves the diffusion equation

$${\partial f\over \partial t} = {\partial^2 f\over \partial x^2}$$ 

with a spectral method by approximating $f(x)$ as an $n$th order Chebyshev polynomial. The boundary conditions are $f=0$ at $x=\pm 1$. As a test problem, take the initial $f(x)$ to be the Green's function for the diffusion equation evaluated at an initial time $t=t_0$ and evolve it forwards in time, comparing with the analytic solution.

 