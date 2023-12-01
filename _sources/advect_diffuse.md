# Advection-diffusion

The 1D advection-diffusion equation is 

$${\partial f\over \partial t} = - v {\partial f\over \partial x} + D {\partial^2 f\over \partial x^2},$$ (advdiff)

where $v$ is a constant velocity and $D$ the diffusivity.

Imagine you want to solve this on a domain from $x=0$ to $x=1$ with periodic boundary conditions. One approach is to take the Fourier transform $f(x)\rightarrow g(k)$ which gives

$${dg\over dt} = -ikvg - Dk^2g.$$ (advdiff_fourier)

Previously, we solved this analytically to obtain $g(t)$, but now in preparation for more complicated examples, let's evolve $g$ in time numerically. 



```{admonition} Exercise: advection-diffusion with Fourier decomposition

Solve equation {eq}`advdiff_fourier` with a constant velocity $v$ and diffusivity $D$. Your goal should be to advect the starting profile $f_0(x)$ for exactly the time needed to wrap around the grid and get back to its starting point. You can then see how well your method works by comparing with the original profile.

Use a grid from $x=0$ to $x=1$ with periodic boundary conditions. Start with $v=1$ and $D=0$. You can then try different values of $D$ and $v$ to see how it changes the result.

For the time-update, try explicit, implicit or semi-implicit time updates 

$$g^{n+1} = g^n \left( 1 + {\dot{g}\over g} \Delta t\right)$$
$$g^{n+1} = g^n \left( 1 - {\dot{g}\over g} \Delta t\right)^{-1}$$
$$g^{n+1} = g^n \left( 1 - {\dot{g}\over g} {\Delta t\over 2}\right)^{-1}\left( 1 + {\dot{g}\over g} {\Delta t\over 2}\right)$$

It is useful to choose a timestep 

$$\Delta t = \alpha\ \mathrm{min}\left({\Delta x\over v}, {\Delta x^2\over D}\right)$$ 

for some constant $\alpha$ (for the explicit method you will need to choose $\alpha$ small enough to keep the method stable).
 
For $f_0(x)$, try using a Gaussian, sine wave, or top hat and see how it changes the result.

[[Solution]](https://andrewcumming.github.io/phys512/adv_diff_fourier.html#advection-diffusion-with-fourier)

```

```{admonition} Exercise: advection-diffusion with finite-difference

Repeat the previous exercise, but now using finite differences. For the advection part, here are some schemes you can use:

**Lax-Friedrich**

$$f^{n+1}_i = {f^n_{i-1} + f^n_{i+1}\over 2} - {v\Delta t\over 2\Delta x}\left(f^n_{i+1} - f^n_{i-1}\right)$$

**Lax-Wendroff**

$$f^{n+1}_i = f^n_i - {v\Delta t\over 2\Delta x}\left(f^n_{i+1} - f^n_{i-1}\right)  + {1\over 2}\left({v\Delta t\over\Delta x}\right)^2\left(f^n_{i+1} -2 f^n_i + f^n_{i-1}\right)$$

**Leapfrog**

$$f^{n+1}_i = f^{n-1}_i - {v\Delta t\over 2\Delta x}\left(f^n_{i+1} - f^n_{i-1}\right)$$

You can use **operator splitting** to advance in time: apply the advection update and diffusion update alternately to your solution.

[[Solution]](https://andrewcumming.github.io/phys512/adv_diff_finite_difference.html#advection-diffusion-with-finite-differences)
```



