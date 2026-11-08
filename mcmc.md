# Markov Chain Monte Carlo

Another way to sample a probability distribution is to use a **Markov Chain Monte Carlo** method. In these methods, we generate a sequence (or chain) of $x$ values that sample a given probability distribution $f(x)$. Each sample $x_i$ is generated from the previous one $x_{i-1}$ in some probabilistic way, so $x_i$ depends only on the state before it $x_{i-1}$ (this is the Markov chain part). By choosing the dependence of $x_i$ on $x_{i-1}$ in an appropriate way, we can generate a sequence $\{x_i\}$ that samples from any given probability distribution $f(x)$.

## Metropolis-Hastings algorithm

The **Metropolis-Hastings algorithm** is a simple method to do this. To generate the next value in  the chain $x_{i+1}$ given the current value $x_i$, the procedure is as follows:

:::{admonition}
1. Propose a jump from the current value $x_i=x$ to a new candidate value $x^\prime$ according to a proposal distribution $p(x^\prime|x)$. The proposal distribution should be symmetric, ie. the probability of choosing a new  location $x^\prime$ given that we are now at $x$ should be the same as the probability of choosing $x$ if we were located at $x^\prime$. For example, in 1D we could choose $x^\prime = x_i + \Delta x$ with $\Delta x$ drawn from a Gaussian distribution.

2. Evaluate the ratio $\alpha = f(x^\prime)/f(x)$. If $\alpha>1$, ie. the new point is located in a region with larger $f(x)$, then we accept the move and set $x_{i+1} = x^\prime$. If $\alpha<1$, we accept the move with probability $\alpha$. If the move is rejected, then we set $x_{i+1} = x_i$.  (Note that this entire step can be achieved by choosing a uniform deviate $u$ between 0 and 1, and accepting the move if $u<\alpha$.)
:::

The size of the jump in step 1 is set by the width of the Gaussian used to sample the jump size $\Delta x$. The width should be chosen so that a reasonable number of trial jumps are accepted. Typically an acceptance fraction of $\approx 0.25$ is a good choice.

After an initial *burn-in* phase, the chain will reach equilibrium and the samples $\{x_i\}$ will be distributed according to the probability distribution $f(x)$. The reason this works is that in equilibrium, the rate at which we move from point $x$ to $x^\prime$ should be equal to the rate at which we move in the reverse direction, from $x^\prime$ to $x$. This is the principle of *detailed balance*. Balancing the forward and backward rates gives

$$n(x) \times (\mathrm{jump\ probability\ from}\ x\rightarrow x^\prime)$$

$$=
n(x^\prime) \times (\mathrm{jump\ probability\ from}\ x^\prime\rightarrow x),
$$
where $n(x)$ is the density of points at $x$.
If $x^\prime$ has a larger value of the probability $f(x^\prime)>f(x)$, then the transition from $x$ to $x^\prime$ will always happen (we always accept), i.e. the rate is 1. The rate of the reverse transition from $x^\prime$ to $x$ is then $f(x)/f(x^\prime)<1$. Therefore

$$n(x) = n(x^\prime) f(x)/f(x^\prime)$$

$${n(x)\over n(x^\prime)}= {f(x)\over f(x^\prime)}$$

which shows that $n(x)$ will be distributed as $f(x)$.



:::{tip} Exercise: Sampling $\exp(-|x^3|)$ with Metropolis-Hastings

Code up this algorithm and generate $10^4$ samples from the distribution $f(x) = \exp(-\left|x^3\right|)$. Confirm that you are getting the correct distribution by comparing the histogram of $x$-values with the analytic expression (the normalization factor was given last time).

Plot the values $\{x_i\}$ against iteration number $i$, and plot $x_i$ against $x_{i+1}$ for the values in the chain. What do you notice?

How do your results change when you take different values for the width of the proposal distribution?

Also, investigate what happens when you change your starting value of $x$. How long does it take for the chain to reach equilibrium?

Compare your results with the same plots for $10^4$ values from $f(x)$ chosen by the rejection method.

:::


## Using data to constrain model parameters with MCMC

A common application of MCMC methods is in fitting models to data and constraining model parameters. Imagine that you have a set of measurements $\{y_i\}$ and a model that makes a prediction for each measurement $m_i(\vec{a})$, where $\vec{a}=(a_1, a_2, ...)$ are the parameters of the model.
Assuming that the errors are Gaussian with standard deviation $\sigma_i$ for measurement $i$, the probability of getting the data given a set of model parameters $\vec{a}$, ie. the **likelihood**, is

$$\mathrm{Prob}\left(\{y_i\} | \vec{a}   \right) \propto  \prod_i \exp\left(-{(y_i-m_i(\vec{a}))^2\over 2\sigma_i^2}\right) $$

or

$$\mathrm{Prob}\left(\{y_i\} | \vec{a}   \right) \propto  \exp\left(-  \sum_i {(y_i-m_i(\vec{a}))^2\over 2\sigma_i^2}\right) = \exp\left(-{\chi^2(\vec{a})\over 2}\right).$$

Minimizing $\chi^2$ maximizes the likelihood, so one approach is to find the set of parameters that minimize $\chi^2$: these are the best fit parameters. But we can also view the likelihood as telling us the probability of different parameter choices. If we sample from this probability distribution, we can generate the distribution of parameters $\vec{a}$ consistent with the data. This not only shows us what the best fit parameters are (maximum likelihood), but we can also do things like calculate the mean value of different parameters given their distribution, or look at the width of the distribution to estimate the error in the best fit parameters, or look for correlations between parameters. 

There's one additional subtlety to mention: the likelihood is the probability of the data given the model, but we really want to know the probability of the model given the data. 
In Bayesian statistics, this is called the **posterior**, and is the product of the **prior** (probability of the parameters $\vec{a}$) and the likelihood, i.e. $\mathrm{Prob}(\vec{a}|\{y_i\}) \propto \mathrm{Prob}(\vec{a})\mathrm{Prob}\left(\{y_i\} | \vec{a}   \right)$. The prior summarizes our prior knowledge about the parameters $\vec{a}$. Here for simplicity, we will assume that all priors are uniform, so that 

$$\mathrm{Prob}\left(\vec{a}|\{y_i\}\right)\propto  \exp\left(-{\chi^2(\vec{a})\over 2}\right).$$

To find the constraints on the model parameters, we can use MCMC to draw samples of $\vec{a}$ from this probability distribution.

:::{tip} Exercise: exoplanet hunting with radial velocities
Do the radial velocity fit from [Homework 5 question 2](hw5#id-2-fitting-planetary-orbits) using MCMC. Use the data file from the homework as well as the function given there to calculate the predicted radial velocities.

Plot the chain of each of the 5 parameters ($M_P,e,\omega_P,t_0,v_0$) and identify the burn-in phase.

Plot the radial velocity curve $v(t)$ corresponding to the last set of parameters in your chain and make sure it goes through the data points. (Or you could plot multiple parameters on the same plot to see the range of solutions, or plot the maximum likelihood solution).

Plot histograms of the 5 parameters. Also look at 2D histograms for different pairs of parameters: do you see any correlations between parameters?
:::

Some hints for this exercise:

- Change only one parameter at a time when you make a step in the MCMC. This will lead to a higher acceptance fraction. Each time you make a step, you can select one of the parameters at random and change only that one. 
- You can use a Gaussian proposal distribution for each parameter. Good choices for the widths of the Gaussians for the parameters $(M_P, e, \omega_P, t_0, v_0)$ are $(0.03, 0.03, 0.03, 3.0, 1.0)$. 
- If you define the numpy array `x = np.zeros((N, 5))` then you can use `x[i]` to access the parameters for iteration `i`. (`N` here is the number of iterations)
- Rather than take the ratio of $e^{-\chi^2/2}$ before and after the jump, it is better to write the ratio of probabilities as $e^{-\Delta\chi^2/2}$ to avoid overflow/underflow errors.
- To visualize your results, you can use [`corner`](https://corner.readthedocs.io/en/latest/) to plot a "triangle" plot. If you don't have this installed, try `pip install corner`. If your samples are in the vector `x`, then you can use 

```
import corner
figure = corner.corner(x, 
        labels = [r'$M/M_J$', r'$e$', r'$\omega$', r'$t_P$', r'$v_0$'], 
        show_titles=True)
```


## Further reading


- A book I like on Monte Carlo methods is [Monte Carlo methods in statistical physics](https://mcgill.on.worldcat.org/search/detail/40927360?datasource=library_web&search_field=all_fields&search=true&database=all&scope=wz%3A12129&format=&clusterResults=on&func=find-b&q=&topLod=0&queryString=newman%20barkema&find=Go) by Newman & Barkema, but unfortunately there is no ebook version. A more modern alternative is Binder & Heerman [Monte Carlo simulation in statistical physics : an introduction](https://mcgill.on.worldcat.org/oclc/1101173204).


