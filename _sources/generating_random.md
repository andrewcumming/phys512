# Probability distributions

Suppose we have access to a source of random numbers that are uniformly distributed. How do we generate numbers that are distributed according to some other probability distribution?

## Rejection method

If $f(x)$ is the probability distribution that we want, the rejection method is to generate points that are uniformly distributed in the $(x,y)$ plane and only accept points that lie within the curve $y=f(x)$. The $x$ values will then be distributed according to $f(x)$. 

For example, consider the distribution $f(x)=\sin x$ for $x=0$ to $x=\pi$ (note that this is normalized to unity as expected for a probability distribution). We choose a set of $(x,y)$ pairs in which $x$ is uniformly-distributed between $0$ and $\pi$, and $y$ is uniformly-distributed between $0$ and $1$ ($1$ is the maximum value of $\sin x$). We keep only the values of $x$ which have a corresponding $y$ that is $y<f(x)$. These $x$-values will be distributed according to $f(x)=\sin x$.

This method is very straightforward to implement, but has the disadvantage that we have to reject some fraction of the sampled points. The rejection fraction can be large if the probability distribution is far from uniform (e.g. very peaked). A way around this is to use another probability distribution for generating the test points which is shaped such that a small number of points end up being rejected.

## Transformation method

In the transformation method, we make a change of variables from $x$ to $y$ in such a way that $y$ is uniformly-distributed. We can then choose a sequence of $y$ values from our generator and then transform back to get the $x$ values.

A example here is the exponential distribution 

$$f(x)dx = e^{-\lambda x} dx.$$

If we choose $y = -\lambda^{-1} e^{-\lambda x}$ then we have $dy/dx = e^{-\lambda x}$, so we can write 

$$ f(x) dx = e^{-\lambda x} dx = dy = f(y) dy$$

ie. we have $f(y) = 1$, a uniform distribution. The range of $x$ is $0$ to $\infty$, corresponding to the range of $y$ from $-1/\lambda$ to $0$. We choose a set of $y$ values from the uniform distribution between these limits and then transform each one back to $x$ using the inverse transform:

$$x = -{1\over \lambda} \ln \left(-\lambda y\right).$$

The values of $x$ will be exponentially-distributed.

This method has the advantage that all samples are used (there is no rejection). Note also that we have used a sampling of points in a finite range to generate a distribution that covers a semi-infinite range in $x$ ($0$ to $\infty$).
The disadvantage of this technique is that the forward and inverse transforms may not be available analytically or hard to evaluate.

## Ratio of uniforms

The ratio of uniforms is a very interesting method that also relies on selecting points in the 2D plane, but in a different way. For a probability distribution $f(x)$, the procedure is:

1. generate points $u$ and $v$ in the 2D plane

2. keep only those points which have 

$$0\leq u \leq \sqrt{2f\left({v\over u}\right)}$$

3. form $x$ values by taking the ratio of $v$ and $u$, ie. $x=v/u$.

4. the $x$ values will be distributed according to $f(x)$.

In this method, $f(x)$ doesn't need to be normalized, only the shape of the function is needed.  This method was introduced by [Kinderman and Monahan 1977](https://dl.acm.org/doi/pdf/10.1145/355744.355750). 
Again note how this method allows us to access the range $x=0$ to $\infty$, since the ratio $v/u\rightarrow \infty$ for $u\rightarrow 0$. 

```{admonition} Exercise:
Implement these three methods for the exponential distribution and check that they work by comparing a histogram of your $x$ values with the analytic function.
```