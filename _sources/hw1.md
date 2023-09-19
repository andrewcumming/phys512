# Homework 1

Due on Tuesday September 19 by midnight.


1. **Roundoff error in long term integrations of planetary orbits**

Roundoff errors can accumulate in situations where you have to carry out a sequence of many operations. Modelling the growth of the error over $N$ operations as a random walk gives *Brouwer's law* which states that the total error after $N$ steps will be $\approx \sqrt{N} \epsilon$ (where $\epsilon \sim 10^{-16}$ for double precision floats).

A classic example of this is when integrating planetary orbits. One method for doing this is the *semi-implicit Euler method*, which involves updating the velocity and position of the planet from timestep $n$ to timestep $n+1$ according to

$$\vec{v}_{n+1} = \vec{v}_n + \vec{a}_{n}\Delta t$$
$$\vec{x}_{n+1} = \vec{x}_n + \vec{v}_{n+1}\Delta t$$

where $\Delta t$ is the timestep, and $\vec{x}$, $\vec{v}$ and $\vec{a}$ are the position, velocity, and acceleration respectively. Note that in this scheme the velocity is updated first, using the acceleration, and then the new value of velocity is used to update the particle position. 

(a) Write a code which uses the semi-implicit Euler method to follow the Earth's orbit around the Sun. Integrate the orbit for one year and plot the orbit in the $x$-$y$ plane to check that the Earth moves as expected. 

[You can assume for simplicity that the Sun is fixed at the origin and the Earth moves in a circular orbit under the (Newtonian) gravity of the Sun. Useful values are: $(GM)_\mathrm{Sun} = 1.3271\times 10^{20}$ in SI units and the Earth-Sun distance (astronomical unit) $1.496\times 10^{11}\ {\rm m}$.]

(b) Try using different time-steps $\Delta t$ for your one-year integration, and check how well the code conserves energy by evaluating the total energy (kinetic plus gravitational) associated with the Earth's motion and comparing with the starting value. 
Start with a timestep of $0.1$ years and go down to as small a value as you can integrate in a reasonable time (I was able to get to $10^{-9}\ {\rm years}$ which took about 10 minutes to run). 

Plot the fractional change in energy over the orbit $\Delta E/E=(E_\mathrm{final}-E_\mathrm{initial})/E_\mathrm{initial}$ against timestep $\Delta t$ and against the number of steps $N$.  (It's helpful to make these log-log plots). Discuss the scaling of $\Delta E/E$ with $\Delta t$ (or $N$) that you find. How does it behave for large and small $\Delta t$ (or $N$)? Do you see any evidence for Brouwer's law?

(c) *Optional*: Using $v_n$ instead of $v_{n+1}$ in the formula for the position update gives the less accurate *explicit Euler method*. Try it in your code -- if you plot the orbit you will see that this method does much worse. How does $\Delta E/E$ and its scaling with $\Delta t$ compare with the semi-implicit Euler method?
[If you want to see an example of a *more* accurate (higher order) method, look at [Rein and Spiegel 2015](https://ui.adsabs.harvard.edu/abs/2015MNRAS.446.1424R/abstract), which implements a 15th order integrator! Compare your results here with their Figure 1 for example.]




2. **Interpolation and thermodynamics**

Interpolation can be tricky in thermodynamics, where there are particular requirements on derivatives that need to be satisfied. We'll investigate that in this question by looking at an ideal gas in a range of temperatures and densities appropriate for Earth's atmosphere. The pressure and entropy per particle of an ideal gas are given by 

$$P = nk_BT, \hspace{1cm} S = k_B \left({5\over 2}-\ln \left({n\over n_Q} \right) \right)$$

where $T$ is the temperature, $n$ is the number density of molecules and $n_Q=(m k_BT/2\pi\hbar^2)^{3/2}$ with $m$ the particle mass. For Earth atmosphere modelling, we can take density and temperature ranges of $10^{-6}$--$1\ {\rm kg\ m^{-3}}$ and $100$--$1000\ {\rm K}$. For this question, you can assume that the atmosphere has a mean molecular weight of $28$, ie. $m=28m_u$ where $m_u$ is the atomic mass unit, and $\rho = 28 m_u n$. 

Whereas in the example in class we used [`scipy.interpolate.RegularGridInterpolator`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.RegularGridInterpolator.html) for 2D interpolation, in this question you may wish to use [`scipy.interpolate.RectBivariateSpline`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.RectBivariateSpline.html#scipy.interpolate.RectBivariateSpline) instead because you can use it to generate derivatives for you (see the `partial_derivative()` method).

(a) Use the formulae given above to calculate the pressure ($\log_{10} P$) and entropy ($S/k_B$) for a uniformly-spaced grid of values of $\log_{10}T$ and $\log_{10}\rho$ that span the range of interest for Earth's atmosphere. Since we are interested in interpolation here, use a small number of points in each direction (e.g. $10$ points).
Interpolate within your table and plot a color map of the fractional error across the $\log_{10}T$--$\log_{10}\rho$ plane. [Since pressure spans orders of magnitude, it's useful to deal with $\log_{10} P$ and similarly $S/k_B$ is a useful way to look at the entropy.]

(b) Now check to what extent your interpolation is *thermodynamically consistent*: In terms of the Helmholtz free energy per particle $F=E-TS$, where $E=(3/2)k_BT$ is the internal energy per particle, the pressure and entropy are given by 

$$S = -\left.{\partial F\over \partial T}\right|_n; \hspace{1cm} P = n^2 \left.{\partial F\over \partial n}\right|_T.$$ 

This implies that the entropy and pressure must satisfy the Maxwell relation

$$-\left.{\partial S\over \partial n}\right|_T = {1\over n^2}\left.{\partial P\over \partial T}\right|_n.$$

Numerically evaluate these derivatives to see how well the Maxwell relation is satisfied in your interpolation scheme. (Plot the fractional difference between the left and right hand sides).
Explain what you are finding -- is it a surprise given the equation of state and interpolation scheme you are using? Would you expect this result to hold for more complicated equations of state?

(c) *Optional*: One way to interpolate that is automatically thermodynamically consistent is to make a grid of values of 

$$F = k_BT \left(\ln \left({n\over n_Q}\right) -1 \right),$$ 

interpolate $F$ as a function of density and temperature, and then calculate $P$ and $S$ as derivatives of $F$ using derivatives of the interpolating functions. Try implementing this and evaluate the errors in $P$ and $S$ and check that your values are thermodynamically consistent. Compare your results with what you found in (a) and (b) where you interpolated $P$ and $S$ directly.























