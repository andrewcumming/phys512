# Metropolis-Hastings algorithm

Another way to sample a probability distribution is to use a **Markov Chain Monte Carlo** method. In these methods, we generate a sequence (or chain) of $x$ values that sample a given probability distribution $f(x)$. Each sample $x_i$ is generated from the previous one $x_{i-1}$ in some probabilistic way, so $x_i$ depends only on the state before it $x_{i-1}$ (this is the Markov chain part).

The **Metropolis-Hastings algorithm** is a simple method to do this. To generate the next value in  the chain $x_{i+1}$ given the current value $x_i$, the procedure is as follows:

- Propose a jump from the current value $x_i=x$ to a new candidate value $x^\prime$ according to a proposal distribution $p(x^\prime|x)$. The proposal distribution should be symmetric, ie. the probability of choosing a new  location $x^\prime$ given that we are now at $x$ should be the same as the probability of choosing $x$ if we were located at $x^\prime$. For example, in 1D we could choose $x^\prime = x_i + \Delta x$ with $\Delta x$ drawn from a Gaussian distribution.

- Evaluate the ratio $\alpha = f(x^\prime)/f(x)$. If $\alpha>1$, ie. the new point is located in a region with larger $f(x)$, then we accept the move and set $x_{i+1} = x^\prime$. If $\alpha<1$, we accept the move with probability $\alpha$. If the move is rejected, then we set $x_{i+1} = x_i$.  (Note that this entire step can be achieved by choosing a uniform deviate $u$ between 0 and 1, and accepting the move if $u<\alpha$.)

After an initial *burn-in* phase, the chain will reach equilibrium and the samples $\{x_i\}$ will be distributed according to the probability distribution $f(x)$. The reason this works is that in equilibrium, the rate at which we move from point $x$ to $x^\prime$ should be equal to the rate at which we move in the reverse direction, from $x^\prime$ to $x$. This is the principle of detailed balance. Therefore

$$n(x) \times (\mathrm{rate\ of}\ x\rightarrow x^\prime)
=
n(x^\prime) \times (\mathrm{rate\ of}\ x^\prime\rightarrow x)
$$

If $x^\prime$ has a larger value of the probability $f(x^\prime)>f(x)$, then the transition from $x$ to $x^\prime$ will always happen (we always accept), i.e. the rate is 1. The rate of the reverse transition from $x^\prime$ to $x$ is then $f(x)/f(x^\prime)<1$. Therefore

$$n(x) = n(x^\prime) f(x)/f(x^\prime)$$

$${n(x)\over n(x^\prime)}= {f(x)\over f(x^\prime)}$$

which shows that $n(x)$ will be distributed as $f(x)$.

```{admonition} Exercise

Code up this algorithm and generate $10^4$ samples from the distribution $f(x) = \exp(-\left|x^3\right|)$. Confirm that you are getting the correct distribution by comparing the histogram of $x$-values with the analytic expression.

Plot the values $\{x_i\}$ against iteration number $i$, and plot $x_i$ against $x_{i+1}$ for the values in the chain. What do you notice?

How do your results change when you take different values for the width of the proposal distribution?

Also, investigate what happens when you change your starting value of $x$. How long does it take for the chain to reach equilibrium?

Compare your results with the same plots for $10^4$ values from $f(x)$ chosen by the rejection method.
```



