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

for $f(x)$ between $x=0$ and $1$ with boundary conditions $f=0$ at $x=\pm 1$. You should use a spectral method based on Chebyshev polynomials, ie. write $f(x)$ as a series of Chebyshev polynomials

$$f(x)  = \sum_i a_i(t) T_i(x)$$

where $T_i(x)$ is the $i$th Chebyshev polynomial and $a_i(t)$ are coefficients that you evolve in time. As a test problem, take the initial $f(x)$ to be the Green's function for the diffusion equation evaluated at an initial time $t=t_0$ and evolve it forwards in time, comparing with the analytic solution. Check how the error compared to the analytic solution depends on the number of modes that you include in your Chebyshev series. Comment on how the scaling compares with what you would expect from finite differences.

Hints:

- if you need a reminder of what the Chebyshev polynomials look like, you can look at our earlier discussion of [orthogonal polynomials](https://andrewcumming.github.io/phys512/polynomial_fit.html#orthogonal-polynomials).

- you can fit a Chebyshev series to your initial $f(x)$ using [`numpy.polynomial.chebyshev.chebfit`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.chebyshev.chebfit.html) or use the code from the [orthogonal polynomial solutions](https://andrewcumming.github.io/phys512/polynomial_fit_solutions.html).

- you should use an even number of polynomials in your Chebyshev series.

- once you have the initial coefficients $a_i$, you can evolve them in time with a first order explicit Euler update.

- to calculate $da_i/dt$ you can use [`np.polynomial.chebyshev.chebder`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.chebyshev.chebder.html) to obtain the coefficients of a Chebyshev series representing the second derivative $\partial^2 f/\partial x^2$. 

- to enforce the boundary conditions initially and after each timestep, you can use the last two $a_i$ values to set the sum of the even $a_i$ coefficients and the odd $a_i$ coefficients to zero.

- you might also find it helpful to set the coefficients to zero if they become too small, e.g. set $a_i=0$ if $a_i$ drops below $10^{-10}$. This helps to avoid accumulation of roundoff errors. 

- the Green's function in this case where we have zero boundary conditions can be constructed by adding mirror images outside the domain on the left and right: (same ideas as the method of images in electrostatics!)

```
def greens_unbounded(x, x0, t, D):
    # Green's function for the diffusion equation
    return np.exp(-(x-x0)**2/(4*D*t)) / np.sqrt(4*np.pi*D*t)

def greens_bounded(x, x0, t, D):
    # Green's function for the diffusion equation with zero boundary conditions at x=-1,+1
    f = gaussian(x, x0, t, D)
    f -= gaussian(x, -1-(1+x0), t, D)
    f -= gaussian(x, 1+(1-x0), t, D)
    return f
```




