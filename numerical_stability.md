# Numerical stability

We have already seen examples of numerical instability, and discussed the idea that one advantage of implicit methods is that they are stable even with large timesteps. In the case of the implicit Euler update for the diffusion equation, for example,
$$T_j^{n+1} = T_j^n + \alpha (T_{j-1}^{n+1} - 2T_j^{n+1} + T_{j+1}^{n+1}),$$
we can see that large timesteps force the solution to steady-state ($\partial^2 T/\partial x^2=0$) (as $\alpha$ becomes large the only way to solve the equation is for the term in brackets on the right hand side to go to zero). Short wavelengths are not followed accurately with large timesteps, but adopt their steady-state solution. This makes physical sense — on timescales long compared to the local diffusion time, we expect the system to go to steady-state.

We can assess the stability of a numerical scheme using **von Neumann stability analysis**. For example, the explicit Euler method for the diffusion equation above gives

$$T^{n+1}_j = \alpha T^n_{j-1} + (1-2\alpha) T^n_j +\alpha T^n_{j+1}.$$(explicit)

To check the stability of the method, we look for a solution

$$T^n_j = \xi^n e^{ikx} = \xi^n e^{ik(j\Delta x)}.$$

The idea is that if $|\xi|>1$ for any wavevector $k$ then the scheme is unstable because $T_j$ will grow exponentially with time.

Substituting this solution into equation {eq}`explicit` gives

$$e^{ikj\Delta x}\xi^{n+1} = \alpha e^{ik(j-1)\Delta x}\xi^n +(1-2\alpha) e^{ikj\Delta x}\xi^n + \alpha e^{ik(j+1)\Delta x}\xi^n$$

$$\Rightarrow \xi = \alpha e^{-ik\Delta x} + (1-2\alpha) + \alpha e^{ik\Delta x}$$

$$\Rightarrow \xi = 1 - 2\alpha \left(1-\cos(k\Delta x)\right)$$

$$\Rightarrow \xi = 1 - 4\alpha \sin^2\left({k\Delta x\over 2}\right)$$

We need $2\alpha < 1$ in order to keep $|\xi|<1$ (or $-1\leq \xi\leq 1$), i.e.

$$\alpha = {\Delta t\over (\Delta x)^2}< 1/2$$ 

for stability. Note that this has a physical interpretation - we need the timestep to be shorter than the diffusion time across a grid cell for the method to be stable. 

For values of $\alpha > 1/2$, the largest value of $|\xi|$ corresponds to $\sin^2 (k\Delta x/2)=1$ or $k\Delta x/2 = \pi/2$. The wavelength of this most-unstable mode is 

$$\lambda = {2\pi\over k} = 2\Delta x,$$

on the scale of the grid spacing. This is why numerical instability often shows up with alternating grid points growing large and positive or large and negative (a spiky profile).

:::{admonition} Exercise
:class: tip
To practise applying this kind of stability analysis, check the stability of the *implicit* Euler method for the diffusion equation and show that all wavelengths are stable no matter what the timestep.
:::
