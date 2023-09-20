# Integration

## Newton-Cotes methods

A related problem to interpolation is when we need to calculate the integral of a function 

$$I = \int_{x_1}^{x_N} f(x) dx$$

given values of the function at a discrete set of points $x_1\dots x_N$, which we'll write as $f_i\equiv f(x_i)$. For simplicity here we'll assume that the spacing between points on the grid is constant $x_{i+1}-x_i = \Delta x$, but it's straighforward to generalize to non-uniform sampling if you need to.

To estimate the value of the integral, we need to model how the function behaves in each interval $x_i<x<x_{i+1}$. As with interpolation, we can take $f(x)$ in each interval to be a polynomial, including additional terms in the polynomial to increase the accuracy of our approximation. 

**Rectangle rule**

The first approximation is to assume that $f(x)$ is a constant $f(x)=f_i$ in the interval $x_i\leq x\leq x_{i+1}$. The total integral is then

$$I\approx \sum_{i=1}^{N-1} f_i \Delta x.$$


**Trapezoidal rule**

If we make a linear approximation, we can write 

$$f(x) \approx f_i + {f_{i+1}-f_i\over \Delta x} \left(x-x_i\right)\hspace{1cm} x_i\leq x\leq x_{i+1}.$$

Then 

$$\int_{x_i}^{x_{i+1}} f(x) dx =  \int_0^{\Delta x} f(x_i+y) dy \approx f_i \Delta x + (f_{i+1}-f_i){\Delta x\over 2}  = {f_i+f_{i+1}\over 2}\Delta x.$$

This is known as the *trapezoidal rule* because it corresponds to the area of the trapezoid formed by connecting the points $(x_i,f_i)$ and $(x_{i+1}, f_{i+1})$ by a straight line.

When we sum over all intervals, each point gets counted twice except for the left and right boundaries, so

$$I \approx \Delta x \left[{f_1\over 2} + f_2 + f_3 \dots + f_{N-1} + {f_N\over 2}\right].$$


**Simpson's rule**

For the next order, we consider the double interval $x_{i-1}<x<x_{i+1}$, and write
 
$$f(x) \approx f_i + (x-x_i) \left.{df\over dx}\right|_{x_i}+ {(x-x_i)^2\over 2}\left.{d^2f\over dx^2}\right|_{x_i}.$$

The reason for considering the double interval from $x_{i-1}$ to $x_{i+1}$ is that the linear term is odd in this interval (antisymmetric about $x_i$) and so does not contribute to the integral, i.e.

$$\int_{x_{i-1}}^{x_{i+1}} f(x) dx \approx 2 f_i \Delta x + {1\over 2} \left.{d^2f\over dx^2}\right|_{x_i} {2(\Delta x)^3\over 3}.$$

Taking 

$$\left.{d^2f\over dx^2}\right|_{x_i} \approx {f_{i+1}-2f_i + f_{i-1}\over (\Delta x)^2}$$

and simplifying then gives

$$\int_{x_{i-1}}^{x_{i+1}} f(x) dx \approx {\Delta x\over 3} \left[f_{i-1} + 4f_i + f_{i+1} \right].$$

Adding up the contributions from each double interval, the total integral is

$$I \approx {\Delta x\over 3}\left[ f_1 + 4f_2 + 2f_3 + 4f_4 + 2f_5 \dots 4f_{N-1} + f_N \right],$$

where we need the total number of points $N$ to be an odd number (so we can divide the domain into a set of double intervals).


```{admonition} Exercise: Newton-Cotes
Implement these three methods to calculate the integral 

$$\int_0^{\pi/2} \cos x \, dx = 1.$$

How does the error in each method scale with the number of points $N$? Is it what you expected?

Next, use your code to check the result that the average value of $\sin^2(x)$ is $1/2$, i.e.

$${1\over \pi} \int_{0}^{\pi} \sin^2(x) dx = {1\over 2}.$$

Does the result surprise you? What happened?
```

```{admonition} Follow up exercise:
What kind of functions are integrated exactly (to machine precision) for each method? (Hint: polynomials of a certain order, which order?)
```

## Gaussian quadrature

In the previous examples, we were given the function $f(x)$ evaluated at a pre-determined set of points $\{ x_i\}$. But if instead we are able to choose the values of $x$ where we can evaluate the function, we can do better using the method of *Gaussian quadratures*. 

The simplest example is the integral

$$\int^{1}_{-1} f(x) dx$$ 

(which is quite general because for different integration limits you can make a change of variables to bring the limits to $-1$ and $+1$). We write the integral as

$$\int^{1}_{-1} f(x) dx = \sum_{i=1}^N w_i f(x_i).$$

For a suitable choice of the weights $w_i$ and the locations $x_i$ it can be shown that this expression is exact when $f(x)$ is a polynomial of order $2N-1$ or less. 

```{admonition} Question
This means that with 2 evaluations of $f(x)$ we can exactly evaluate the integral of a cubic polynomial! How does this compare with Simpson's method?
```

The proof that this works and the calculation of the values of $w_i$ and $x_i$ is quite involved. [It uses the theory of orthogonal polynomials, so for example for this particular form of the integral, the values of $x_i$ are the roots of the Legendre Polynomial $P_N(x)$.] However, we can look up $w_i$ and $x_i$ using [`numpy.polynomial.legendre.leggauss`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.legendre.leggauss.html)

For example, 

```
>>> np.polynomial.legendre.leggauss(2)
(array([-0.57735027,  0.57735027]), array([1., 1.]))
```

gives the $N=2$ values. In this case, the weights are both 1, and the $x$ values are $\pm 1/\sqrt{3}$. 


```{admonition} Exercise: Gaussian quadrature
Using [`numpy.polynomial.legendre.leggauss`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.legendre.leggauss.html)
to get the locations and weights for different choices of $N$, implement Gaussian quadratures. 

Check that the answer is indeed exact for polynomials of degree $2N-1$ or less. What happens for higher order polynomials? 

Try a function that is not a simple polynomial, e.g. $e^{-x^2}$. What is the error in the approximation and how does it scale with $N$?

Hints: 
- to get the correct value of the integral to compare with to see how accurate your approximation is, you could use the general purpose integrator [`scipy.integrate.quad`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.quad.html)
- to generate a polynomial with degree `N` and random coefficients between -10 and +10 (for example) you can use
`np.polynomial.Polynomial(np.random.randint(-10,high=10,size=N+1))`
```

Gaussian quadratures can be applied more generally to integrals of the form

$$\int W(x) f(x) dx$$

for some weight function $W(x)$ (in the previous example we had $W(x)=1$). We write the integral as a sum  

$$\int W(x) f(x) dx= \sum_{i=1}^N w_i f(x_i)$$

and find the choices of $w_i$ and $x_i$ that make the sum exact for a polynomial of degree $2N-1$ or less.

One example is $W(x)=e^{-x^2}$, ie. integrals

$$\int_0^\infty e^{-x^2} f(x) dx.$$

In this case, the weights and locations are given by [`numpy.polynomial.hermite.hermgauss`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.hermite.hermgauss.html). [The locations $x_i$ are the roots of the $N$th Hermite polynomial, which are the polynomials that are orthogonal under an inner product defined with weight function $W(x)$.]


```{admonition} Exercise: Gauss-Hermite

Modify your code to use the Gauss-Hermite coefficients and check that you can get an exact answer for the integral of $e^{-x^2}$.

Hint: If you want to use `scipy.integrate.quad` again to get the value of the integral as a comparison, note that you can give it limits of $-\infty$ to $+\infty$ using `-np.inf` and `np.inf`.
```

Other examples are

-  $W(x)=e^{-x}$ with integration limits $0$ to $\infty$. In this case, we need Gauss-Laguerre integration -- see [`numpy.polynomial.laguerre.laggauss`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.laguerre.laggauss.html)
- $W(x)=1/\sqrt{1-x^2}$ from $x=-1$ to $1$ is Gauss-Chebyshev quadrature -- see 
[`numpy.polynomial.chebyshev.chebgauss`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.chebyshev.chebgauss.html)


## Integration challenge

```{admonition} Exercise: Average velocity of the Maxwell-Boltzmann distribution.

Use Simpson's rule, Gaussian quadrature, and the general purpose integrator [`scipy.integrate.quad`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.quad.html) to evaluate the average velocity $\langle\left|v\right|\rangle$ for the 3D [Maxwell-Boltzmann distribution](https://en.wikipedia.org/wiki/Maxwell–Boltzmann_distribution).

For each method, check the numerical error comparing to the analytic result. How many points do you need to get to $0.1$% accuracy?

For Simpson's rule you can use your own implementation from above or you could try [`scipy.integrate.simpson`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.simpson.html).

For Gaussian quadrature, try both Gauss-Hermite and Gauss-Laguerre. Which one is best?
```

## Further reading

- Integration methods are covered in Chapter 7 of Gezerlis.

- Overview of [`scipy.integrate'](https://docs.scipy.org/doc/scipy/tutorial/integrate.html)

- [QUADPACK](https://en.wikipedia.org/wiki/QUADPACK) is the Fortran 77 library that is used by `scipy.integrate.quad` under the hood.







