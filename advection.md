# Advection

The advection equation
$${\partial f\over \partial t} = -v {\partial f\over \partial x}$$
describes advection of the quantity $f$ in the $x$-direction at speed $v$. We'll take $v$ to be a constant.

We saw a finite difference method for solving this in the midterm. Let's instead solve this using a spectral method. We will work in the domain from $x=0$ to $x=1$ and assume periodic boundary conditions. We can then take the Fourier transform $f(x)\rightarrow g(k)$ which gives
$${dg\over dt} = -ikvg.\label{eq:adv_fourier}$$
Just as with the diffusion equation, the Fourier transform turns the PDE into an ODE that we can solve for $g(k,t)$ and then take the inverse transform to get $f(x,t)$.

For constant $v$, we can solve equation {eq}`eq:adv_fourier` analytically, giving $g(k,t) = g(k,0) e^{-ikvt}$. This matches what we found in the last exercise: multiplying by $e^{-iky}$ for some constant $y$ causes a translation of $f(x)\rightarrow f(x-y)$. In our case we have $f(x)\rightarrow f(x-vt)$ corresponding to advection to the right at speed $v$.

**However** even though we could do this analytically, let's instead do the time integration numerically. This will be useful for more complicated examples where there is no analytic solution.

:::{tip} Exercise: Advection using a Fourier decomposition

Solve equation {eq}`eq:adv_fourier` with a constant velocity $v=1$. Your goal should be to advect the starting profile $f_0(x)$ for exactly the time needed to wrap around the grid and get back to its starting point. You can then see how well your method works by comparing with the original profile.

Use a grid from $x=0$ to $x=1$ with periodic boundary conditions. Note that because the boundary conditions are periodic, the point $x=1$ is the same as $x=0$, so you don't need to include $x=1$ on your grid. You can achieve this using `x = np.linspace(0, 1, n, endpoint=False)`.

For the time-update, try explicit, implicit or semi-implicit time updates 
$$g^{n+1} = g^n \left( 1 + {\dot{g}\over g} \Delta t\right)$$
$$g^{n+1} = g^n \left( 1 - {\dot{g}\over g} \Delta t\right)^{-1}$$
$$g^{n+1} = g^n \left( 1 - {\dot{g}\over g} {\Delta t\over 2}\right)^{-1}\left( 1 + {\dot{g}\over g} {\Delta t\over 2}\right),$$
where $\dot{g}/g = -ikv$.
It is useful to write the timestep as
$$\Delta t = \alpha\ {\Delta x\over v}$$
for some constant $\alpha$.
 
Try three different initial conditions: a Gaussian, sine wave, or top hat.

Questions:
- for the explicit method, we proved in the midterm that we need $\alpha<1$ for stability. What about the spectral method?
- what do you notice about the results for the three time updates? Are they stable? What changes in shape do you notice in the advected profile compared to its initial shape, and how are they different between the different methods? What are the differences between the three different initial profiles?
- Can you understand what is happening with each method by looking at the update formula for each case?
- I almost titled this section "Advection is not easy". Do you agree?


:::


