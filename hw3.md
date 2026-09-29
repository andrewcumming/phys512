# Homework 3

Due on Thursday October 8 by 11:59pm.

## 1. Crank-Nicolson diffusion

The Crank-Nicolson method applied to diffusion looks like this:

$$T_j^{n+1} = T_j^n + {\alpha\over 2}(T_{j+1}^n-2T_j^n+T^n_{j-1}) + {\alpha\over 2}(T_{j+1}^{n+1}-2T_j^{n+1}+T^{n+1}_{j-1}).$$

(a) Use the von Neumann stability analysis to show that the method is always stable for all values of $\alpha$.

(b) What does your result from part (a) imply for the behavior when $\alpha$ is large? How does that differ from the fully-implicit method?

(c) Implement the Crank-Nicolson update in your code [from class](solutions-ivp#diffusion-equation) (so the same problem that we looked at there: $T=1$ initially, boundaries held at fixed temperature $T=0$). Calculate the evolution for $n_\mathrm{steps}=40$ steps with $\alpha=100$. Now repeat this but replace the first few steps with fully-implicit steps and then switch to Crank-Nicolson for the rest of the evolution. What difference do you see between these two runs? Explain how this is related to your answer to part (b).

(d) Carry out integrations with a fixed total time $n_\mathrm{steps}\alpha$ but different number of timesteps $n_\mathrm{steps}$ and in each case measure the flux $dT/dx$ at $x=0$ at the end of the integration. Use this to show that Crank-Nicolson is second order in time whereas fully-implicit is first order in time.






## 2. Reflection and transmission of an electromagnetic wave

You have probably seen the reflection and transmission coefficients for electromagnetic waves at a boundary between two materials:

$$r = {n_2-n_1\over n_2+n_1}, \hspace{1cm} t = {2n_1\over n_2+n_1},$$

where $n_1$ and $n_2$ are the refractive index of each material respectively. In this question we will check these by directly integrating the wave equation!

Maxwell's equations for an electromagnetic wave propagating in the $x$-direction in a material with refractive index $n$ can be written

$${\partial B\over \partial t} = -{\partial E\over \partial x}, \hspace{1.5cm}{\partial E\over \partial t} = -{1\over n(x)^2}{\partial B\over \partial x}$$

where $E$ and $B$ are the electric and magnetic fields and we have set the speed of light $c=1$ for simplicity. Writing down a second-order finite difference approximation for both the space and time derivatives gives the update scheme 

$$B^{n+1}_i = B^{n-1}_i - {\Delta t\over \Delta x} \left(E^n_{i+1}-E^n_{i-1}\right)$$ 

$$E^{n+1}_i = E^{n-1}_i - {\Delta t\over \Delta x} \left(B^n_{i+1}-B^n_{i-1}\right){1\over n_i^2}.$$

Note that to find the values at time $n+1$ requires storing both values from the previous two timesteps, $n$ and $n-1$. This is another example of a leapfrog method.

(a) First code up this algorithm and set the refractive index $n=1$ across the grid. Start with a Gaussian profile centered on your grid. Choose $E$ and $B$ to have the same profile, but try giving them either the same sign or opposite signs. What difference does that make? Does the Gaussian propagate without changing shape?

(b) Now set $n=n_1$ on the left half of your grid and $n=n_2$ on the right half. Start a pulse in the left half moving towards the right (Hint: for $n$ not equal to 1 you will need to set $nE$ and $B$ to have the same profile to get the wavepacket moving in one direction). Do you see the expected behaviour of the wave amplitude when the pulse encounters the change in $n$ at the middle of the grid? Explain your answer as fully as you can.

