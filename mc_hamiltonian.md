# Hamiltonian Monte Carlo

The Metropolis algorithm has the downside that we need to reject many of the trial steps. As we have seen, this leads to correlations between samples. **Hamiltonian Monte Carlo** is a interesting method that takes advantage of Hamiltonian dynamics to take large steps in parameter space with an acceptance probability close to 1.

Imagine we want to build a sample from the 1D probability distribution $f(x)$. The idea is to instead sample from the 2D distribution
$$\mathrm{Prob}(x,p) \propto e^{-H(x,p)} \propto e^{-U(x)} e^{-{1\over 2}p^2}$$
but keep only the $x$-values, so we marginalize over $p$. The $x$-values will then be distributed as
$$\mathrm{Prob}(x) \propto e^{-U(x)}.$$
We can make this our target distribution $f(x)$ if we define
$$U(x) = -\ln f(x).$$

To do this, we can use the Metropolis algorithm. We know that the distribution of $p$ should be Gaussian, so we can just sample that directly. At any starting value of $x$, we choose a momentum $p$ from a Gaussian distribution. Then to propose a step, rather than use a Gaussian proposal distribution as we did for Metropolis last time, instead we can take advantage of the fact that we know how to evolve $x$ and $p$ in a way that conserves $H$. If we identify $H$ as the Hamiltonian of a system (obviously we had this in mind all along which is why we wrote $H(x,p)=U(x)+p^2/2$), then the evolution is given by Hamilton's equations
$${dx\over dt} = {\partial H\over \partial p} = p;\hspace{1cm} {dp\over dt} = -{\partial H\over \partial x} = -{\partial U\over \partial x}.$$
We know how to integrate these numerically in a way that conserves energy: we can use a symplectic integrator such as leapfrog,
$$p_{n+1/2} = p_n - {\Delta t\over 2} \left.{\partial U\over\partial x}\right|_{x=x_n}$$
$$x_{n+1} = x_n + \Delta t\, p_{n+1/2}$$
$$p_{n+1} = p_{n+1/2} - {\Delta t\over 2} \left.{\partial U\over\partial x}\right|_{x=x_{n+1}}.$$
The idea then is to take a step in parameter space by integrating these equations forward in time (say for a time $n_\mathrm{steps}\Delta t$ to go from a starting location $(x,p)$ to a new location $(x^\prime,p^\prime)$. This is the proposed jump. The key thing is that the integration will conserve energy up to some numerical error, so the value of $e^{-H(x^\prime,p^\prime)}$ will be very close to the starting value $e^{-H(x,p)}$ as long as we take small enough timesteps in the Leapfrog integration. This means that the acceptance probability will be close to 1. This should be a very efficient method!

Here is some code that implements this as well as the traditional Gaussian proposal step for comparison:

::: python
def run_chain(N, x0, metropolis=True):
    x = np.zeros((N,len(x0)))
    x[0] = x0  # starting point
    
    count = 0  # count accepted jumps

    # leapfrog parameters
    dt = 0.1
    nsteps = 25
    
    for i in range(N-1):
    
        if metropolis:
            # Metropolis
            x_try = x[i] + rng.normal(size=2, scale=2)
            u = rng.uniform()
            if u <= f(x_try) / f(x[i]):
                x[i+1] = x_try
                count = count + 1
            else:
                x[i+1] = x[i]
    
        else:
            # Hamiltonian
            p = rng.normal(size=2, scale=1)
            H = U(x[i]) + 0.5*p@p
    
            x1 = np.copy(x[i])
            p1 = np.copy(p)
            for j in range(nsteps):
                p1 = p1 - 0.5 * dt * gradU(x1)
                x1 = x1 + dt * p1
                p1 = p1 - 0.5 * dt * gradU(x1)
            
            H1 = U(x1) + 0.5*p1@p1
            alpha = np.exp(H-H1)
            u = rng.uniform()
            if u<=alpha:
                x[i+1] = x1
                count = count + 1
            else:
                x[i+1] = x[i]
    return x, count
:::

Note that this is written to support multi-dimensional distributions. You pass the initial location as a vector `x0`. So for a 2D distribution $f(x,y)$ for example, if you start the chain at $x=2$, $y=3$, then pass the vector `x0 = (2,3)`.


:::{tip} Exercise

Use this code to sample $10^4$ points from a 2D skewed Gaussian distribution defined as follows:

``` python
coeff = 2 - 0.05

def f(z):
    x,y = z
    return np.exp(-x**2 + coeff*x*y -y**2)

def U(z):
    return -np.log(f(z))

def gradU(z):
    x,y = z
    return np.array([2*x - coeff*y, 2*y - coeff*x])
```

You may recognize this function as the 2D wavefunction from the homework. I've set the coefficient of the $xy$ term to be close to 2, so that $x$ and $y$ are correlated.

Call `run_chain` two times, once with `metropolis=True` and once with `metropolis=False`.

- What is the acceptance fraction in each case?

- Plot the two distributions of $(x,y)$ points to make sure you are getting good samples from $f(x,y)$.
Then plot the two chains: how do they compare?

- Calculate the autocorrelation function $c(k) = \sum_n x_n x_{n+k}$ of each chain and plot them on the same plot (set the $x$-axis limits so you plot from $k=0$ to $k=500$). Do you see a difference between the Metropolis and Hamiltonian results?

Here is a function you can use to get the autocorrelation function:
``` python
def acf(x):
    x = x - x.mean()
    n = len(x)
    f = np.correlate(x, x, mode="full")[n-1:] # positive lags
    return f/f[0]
```

:::
