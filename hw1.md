# Homework 1

Due on Thursday September 17 by 11:59pm.

## 1. Roundoff error in long-term integrations of a harmonic oscillator

Roundoff errors can accumulate in situations where you have to carry out a sequence of many operations. If the roundoff error in each operation is approximately random, the accumulated error behaves like a random walk. This leads to *Brouwer's law*, according to which the accumulated error after $N$ steps typically grows approximately as $\sqrt{N}\epsilon,$ where $\epsilon\sim10^{-16}$ for double-precision floating-point numbers or $\epsilon\sim 10^{-7}$ for single-precision.

A simple example in which to investigate this is the harmonic oscillator,
$$\frac{d^2x}{dt^2}=-\omega^2x.$$
One method for integrating this equation is the *semi-implicit Euler method*. Writing $v=dx/dt$, the velocity and position are updated from timestep $n$ to timestep $n+1$ according to
$$v_{n+1}=v_n-\omega^2x_n\Delta t,$$
$$x_{n+1}=x_n+v_{n+1}\Delta t.$$
Note that the velocity is updated first, using the current acceleration, and the new value of the velocity is then used to update the position.

For simplicity, use units in which $\omega=1$, and take the initial conditions $x(0)=1$, $v(0)=0.$ The exact solution is then $x(t)=\cos t$, oscillations with period $2\pi$.

(a) Write a code that uses the semi-implicit Euler method to follow the motion of the harmonic oscillator. Integrate the oscillator for a fixed total time of $t_{\rm final}=100$ (about 16 oscillation periods). Plot $x$ as a function of time and compare your numerical result with the exact solution $x(t)=\cos t$. You can also make a phase-space plot of $v$ against $x$ (what shape do you expect the exact solution to trace out?)

(b) The total energy of the oscillator is
$$E=\frac12v^2+\frac12\omega^2x^2.$$
Try using different timesteps $\Delta t$ for your integration, and investigate how well the numerical method conserves energy. Start with a relatively large timestep, for example $\Delta t=0.1$, and progressively decrease it. Continue to as small a timestep as is practical (you can probably get to about a billion steps which will take several minutes to integrate).

For each integration, keep track of the maximum fractional energy error over the course of the integration,
$$\left|\frac{\Delta E}{E}\right|_{\max}=\max_t\left|\frac{E(t)-E(0)}{E(0)}\right|.$$
Then plot this quantity against the timestep $\Delta t$ and against the number of integration steps $N$ (use log-log plots so you can see the scalings).

Discuss the behaviour that you find. How does the error depend on timestep $\Delta t$?

(c) Repeat part (b) using single-precision floating-point numbers instead of double-precision numbers (Be careful to make sure every float and array you declare has the `np.float32` data-type, otherwise you may "contaminate" the answer). How does your answer change? Explain the differences that you see. In particular, what happens when the timestep becomes very small? At sufficiently small $\Delta t$, does decreasing the timestep continue to improve the answer? Is the behaviour at very large $N$ consistent with an accumulated roundoff error proportional to $\sqrt{N}\epsilon$, and if not, why not?


## 2. An adaptive Runge-Kutta integrator

In the exercise "Orbit integrations" that we did in class, we looked at a circular orbit. The same equations apply for eccentric orbits as well, if you choose different initial conditions. For example, you can set an initial distance $r=1+e$ where $e$ is the eccentricity, and a perpendicular velocity $v = \sqrt{(2/r)-1}$ (from energy conservation), and you should get an eccentric orbit with eccentricity $e$. Because an object on an eccentric orbit spends different amounts of time in different parts of the orbit, integrating with a constant time-step is not very efficient, particularly for large eccentricities.

To address this issue, implement an adaptive-step-size RK4 integrator and use it to integrate an eccentric orbit over a time $2\pi$ (so the orbit closes), keeping the relative error just below 1 part in $10^6$.

To choose the step size you can do the following:

1. Take an RK4 step with the current step size $h$ and compare that with the result you get if you instead take two steps with half the step-size $h/2$. The difference between the results gives you a measure of the error.

2. If the error is smaller than the desired tolerance, then you keep the result and increase $h$ by a factor of 2. If the error is larger than the desired tolerance, reject this step and try again with $h$ smaller by a factor of 2.

3. Repeat. You will need to adjust the final step-size so that your final value of time is exactly at $t=2\pi$.

Plot the orbit to make sure you are indeed getting an ellipse and that the orbit goes back to its starting point after a time $2\pi$. Compare the number of steps you need to take to get an accuracy of $10^{-6}$ for a full $e=0.9$ orbit with adaptive step size and with constant step size.
