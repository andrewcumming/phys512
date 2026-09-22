# Diffusion equation

The *method of lines* is a way to do time-evolution of partial differential equations in which we turn the PDE into a set of ODEs. To see how this works, consider the thermal diffusion equation 
$${\partial T\over \partial t} = D {\partial^2 T\over\partial x^2},$$
where $T(x,t)$ is the temperature and $D$ is the thermal diffusivity. For simplicity, we'll assume $D$ is a constant, but it could depend on position or even time in more complicated examples.

The idea is to follow the temperature $T$ on a grid of specific $x$ values. We'll label the grid cell by subscript $i$, so $T_i$ refers to the temperature at $x=x_i$. For simplicity, consider a uniform grid with constant spacing $\Delta x$. Then, we can use [finite differences](derivatives#finite-differences) to rewrite the derivative on the right hand side, giving
$${\partial T_i\over \partial t} = D {T_{i+1} - 2T_i + T_{i-1}\over (\Delta x)^2}.$$
We now have a set of coupled-ODEs that we can integrate forwards in time to determine $\vec{T}(t)$, where $\vec{T}$ is the vector of $T_i$ values.

:::{tip} Exercise

Write a code to solve the diffusion equation and calculate the evolution of the temperature over time. Write your code so that you can use either an explicit (Euler) or implicit (backwards-Euler) method. For boundary conditions, set $T=0$ at each boundary.

Try initial conditions (1) constant temperature everywhere at $t=0$, and (2) a Gaussian profile for $T$ at $t=0$.

Hints:
- the equations we have to solve are [linear](implicit-ode#backward-euler) so the first step is to find the appropriate matrix of coefficients.
- it is useful to define the dimensionless variable $\alpha = D\Delta t/(\Delta x)^2$ which measures the timestep. You will be able to write your matrix in terms of $\alpha$.
- the corners of your matrix will need to be adjusted to take into account the boundary conditions. In particular, you need $T^{n+1} = T^n$ for the left and right boundaries, where $n$ labels the timestep.
- to see the evolution, plot the profile $T(x)$ for different times on the same plot.

:::

