# Monte Carlo integration

In our discussion of the rejection method, you might have wondered whether we could use the samples of $f(x)$ to calculate the integral under the curve. Indeed, for a uniform sampling of $x$ values $\left\{x_i\right\}$ between $x=a$ and $x=b$ and the corresponding function values $f(x_i)$, 
we can estimate the value of the integral using

$$\int_a^b dx f(x) \approx {(b-a)\over N} \sum_i f(x_i).$$

You can think of this as $\sum_i f(x_i) \Delta x$ with the mean spacing of points $\Delta x = (b-a)/N$.

Depending on the shape of the integral, this estimate can be improved by using a different distribution for the $x$-values which concentrates the samples of $x$ in the regions where the integrand is largest (the ideal case would be to sample from $f(x)$ itself). In this case, we have to rescale the weight of a given $x$-value according to the ratio $f(x)/p(x)$. The procedure for estimating the integral is: (1) select $N$ samples of $x$ from the probability distribution $p(x)$, and (2) evaluate the integral using

$$ {1\over N} \sum_i {f(x_i)\over p(x_i)}.$$

The factor $1/p(x_i)$ measures the effective spacing of the samples at $x=x_i$. If $p(x)$ is large, many points will be chosen near that value of $x$, so the effective spacing between points is small in that region. In the case of a uniform distribution, $p(x) = 1/(b-a)$, we recover our original expression for uniform sampling.

```{admonition} Exercise

Calculate the Monte Carlo estimate for the integral of $\exp(-\left|x\right|^3)$ between $x=0$ and $\infty$. 

Use both uniform sampling of $x$ (in which case you will have to put in an upper limit on the $x$-value), and Gaussian sampling of $x$. 

In each case, take the number of samples to be $N=10^4$ and repeat the integration 1000 times. Plot a histogram of the values of the integral that you obtain. Calculate the mean and standard deviation and compare the uniform and Gaussian sampling, and also with the exact value of the integral (you can use `scipy.integrate.quad`).

Repeat for different $N$ values. How does the error in your value for the integral scale with $N$?

```
