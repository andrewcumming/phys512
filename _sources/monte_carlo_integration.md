# Monte Carlo integration

In the previous integration methods we looked at, we typically evaluated the integrand $f(x)$ on a regular grid of points in $x$. In Monte Carlo integration, we sample the $x$-points from a probability distribution instead. 

The simplest case is to take a uniformly-distributed sample of $x$ values $\left\{x_i\right\}$ between $x=a$ and $x=b$. If we calculate the corresponding function values $f(x_i)$, 
we can then estimate the value of the integral using

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

The previous exercise shows that the integration error is $\propto N^{-1/2}$. This may not look that attractive -- for one-dimensional integrals, Simpson's rule gives a much smaller error $\propto N^{-4}$. However, Monte Carlo integration wins for high-dimensional integrals. If we sample a function $N$ times in $d$ dimensions, the number of samples per dimension is $N^{1/d}$, so the Simpson's rule accuracy for any particular direction becomes $N^{-4/d}$. This is known as the **curse of dimensionality**. For multi-dimensional integrals, you can get a better error with Monte Carlo integration, and it is also simpler to code up.

**Further reading**

- The [VEGAS algorithm](https://en.wikipedia.org/wiki/VEGAS_algorithm) is a more sophisticated algorithm for Monte Carlo integration that uses the $f(x)$ samples that are being collected to refine the importance sampling probability distribution.

- Metropolis (1953) [Equation of State Calculations by Fast Computing Machines](https://pubs.aip.org/aip/jcp/article/21/6/1087/202680/Equation-of-State-Calculations-by-Fast-Computing)

- A good book is Newman and Barkema [Monte Carlo Methods in Statistical Physics](https://mcgill.on.worldcat.org/search/detail/40927360?queryString=au%3A%28newman%29%20AND%20ti%3A%28monte%20carlo%20methods%29&databaseList=283%2C638&origPageViewName=pages%2Fadvanced-search-page&clusterResults=&groupVariantRecords=&expandSearch=true&translateSearch=false&queryTranslationLanguage=&lang=en&scope=wz%3A12129)
