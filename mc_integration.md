# Monte Carlo integration

One of the useful things about MCMC is that if we have $N$ samples $\{x_i\}$ from a probability distribution $f(x)$, and want to calculate the average value of some quantity $A(x)$, we can just take the mean value of $A(x)$ evaluated using the samples:
$$\langle A\rangle = \int f(x) A(x) dx \approx {1\over N}\sum_i A(x_i).$$
So for example, the mean $x$-value would be just
$$\langle x\rangle \approx {1\over N}\sum_i x_i.$$

We can use this idea to do integrals. For example, imagine that we have a function $g(x)$ that we need to integrate and a distribution $f(x)$ that we can draw samples from. Then write
$$\int g(x) dx = \int f(x) {g(x)\over f(x)} dx \approx {1\over N}\sum_i {g(x_i)\over f(x_i)}.$$

A simple case would be to take a uniform distribution for $f(x)$. For integration limits from $x=a$ to $x=b$, the normalized distribution is $f(x) = 1/(b-a)$. Therefore we can write
$$\int_a^b g(x) dx \approx {b-a\over N} \sum_i g(x_i),$$
where the $\{x_i\}$ are uniformly-distributed between $x=a$ and $x=b$. The most efficient solution is to take $f(x)$ to be as close to $g(x)$ as possible, so that the samples are concentrated in the regions where they contribute most to the integral.

We can also get an estimate for the error bar on our answer. Since the value for the integral is the mean value of the $N$ samples of $g(x)/f(x)$, ie. $I=\langle g/f\rangle$, we can estimate the error as the error in the mean $\sigma_I$ given by 
$$\sigma_I^2 = {1\over N}\mathrm{Var}(g/f) = {1\over N}(\langle \left({g/f}\right)^2\rangle - \langle {g/f}\rangle^2).$$


:::{tip} Exercise

Calculate the Monte Carlo estimate for the integral of $\exp(-\left|x\right|^3)$ between $x=0$ and $\infty$. 

Use both uniform sampling of $x$ (in which case you will have to put in an upper limit on the $x$-value), and Gaussian sampling of $x$. 

In each case, take the number of samples to be $N=10^4$ and repeat the integration 1000 times. Plot a histogram of the values of the integral that you obtain. Calculate the mean and standard deviation of the values. Compare with the error in the mean $\sigma_I$ from one of your integrations.

Repeat for a few different values of $N$. How does the error in your value for the integral scale with $N$?

Hints:

- You can use `np.mean` and `np.std` to calculate the mean and standard deviation, e.g. for $\sigma_I$ you can do `np.std(f(x)/p(x))/np.sqrt(N)`.
- This could be a good time to use a `lambda`, e.g.

``` python
p = lambda x: np.exp(-x**2/2) * np.sqrt(2/np.pi)
x = rng.normal(size = N) 
I = np.mean(f(x) / p(x))
```

:::

The previous exercise shows that the integration error is $\propto N^{-1/2}$. Standard integration methods give a much better scaling in 1D — for example Simpson's rule is a common integration method that gives error $\propto h^4 \sim N^{-4}$ ($h$ is the spacing between points). However, Monte Carlo integration wins for high-dimensional integrals. If we sample a $d$-dimensional function $N$ times, the number of points along each dimension is approximately $N^{1/d}$, so the Simpson's rule accuracy becomes $h^4\sim N^{-4/d}$. This is known as the **curse of dimensionality**. For multi-dimensional integrals, you can get a better error with Monte Carlo integration, and it is also simpler to code up.

**Further reading**

- The [VEGAS algorithm](https://en.wikipedia.org/wiki/VEGAS_algorithm) is a more sophisticated algorithm for Monte Carlo integration that uses the $f(x)$ samples that are being collected to refine the importance sampling probability distribution.
