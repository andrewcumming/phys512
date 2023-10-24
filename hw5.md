# Homework 5

Due on Friday Nov 3 by midnight.

1. **An adaptive Runge-Kutta integrator**

In the exercise on [integrating planetary orbits](https://andrewcumming.github.io/phys512/runge_kutta.html#runge-kutta), we looked at a circular orbit. The same equations apply for eccentric orbits as well, if you choose different initial conditions. For example, you can set an initial distance $r=1+e$ where $e$ is the eccentricity, and a perpendicular velocity $v = \sqrt{(2/r)-1}$ (from energy conservation). Because an object on an eccentric orbit spends different amounts of time in different parts of the orbit, integrating with a constant time-step is not very efficient, particularly for large eccentricities.

To address this issue, implement an adaptive-step-size RK4 integrator and use it to integrate an eccentric orbit over a time $2\pi$ (so the orbit closes), keeping the relative error just below 1 part in $10^6$.

To choose the step size you can do the following:

1. Take an RK4 step with the current step size $h$ and compare that with the result you get if you instead take two steps with half the step-size $h/2$. The difference between the results gives you a measure of the error.

2. If the error is smaller than the desired tolerance, then you keep the result and increase $h$ by a factor of 2. If the error is larger than the desired tolerance, reject this step and try again with $h$ smaller by a factor of 2.

3. Repeat. You will need to adjust the final step-size so that your final value of time is exactly at $t=2\pi$.

Plot the orbit to make sure you are indeed getting an ellipse and that the orbit goes back to its starting point after a time $2\pi$. Compare the number of steps you need to take to get an accuracy of $10^{-6}$ for a full $e=0.9$ orbit with adaptive step size and with constant step size.

2. **Method of lines**

The *method of lines* is a way to do time-evolution of partial differential equations. For example, consider the thermal diffusion equation

$${\partial T\over \partial t} = {\partial^2 T\over\partial x^2},$$

where we set the thermal diffusivity to 1 for simplicity. We want to solve the following 1D diffusion problem: A piece of metal is initially at a temperature $T=1$ everywhere. At time $t=0$, the end of the piece of metal at $x=1$ is set to a temperature $T=0$ and held at that value. The temperature at the other end $x=0$ is held constant at $T=1$. Calculate the temperature profile as a function of time $T(x,t)$.

In the method of lines, we transform the PDE into an ODE by finite differencing the spatial derivative on a grid in $x$:

$${dT_i\over dt} = {T_{i+1} - 2T_i + T_{i-1}\over (\Delta x)^2}$$(Tevol)

where $T_i$ is the temperature at $x=x_i$. If there are $N$ grid points, we now have $N$ coupled-ODEs that we can integrate in time. 

Use this method to solve the problem described above. You should write your own *implicit* integrator using the techniques discussed in [the notes](https://andrewcumming.github.io/phys512/runge_kutta.html#implicit-methods-and-stiff-equations) (do not use `solve_ivp` for this problem).

You will need to take care at the boundaries: the values of $T_1$ and $T_N$ need to be held fixed according to the boundary conditions, so they obey $dT_1/dt=dT_N/dt=0$ rather than equation {eq}`Tevol`.

Plot the temperature profile $T(x)$ at different times and discuss whether it shows the behavior you expect.
