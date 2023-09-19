# Homework 2

Due on Friday Sep 29 by midnight.

1. **Integration errors**

In the class exercises on integration, we knew the function we were integrating, so we could calculate the error in our numerical integrations by comparing with the analytic answer for the integral. However, in real situations where you have a tabulated function to integrate without knowing the underlying analytic function, how do we estimate the error? One method is to see how much the numerical integral changes when you change the number of points. For example, if you have $N$ function evaluations with spacing $\Delta x$, you can calculate the integral using all $N$ points, and then again using approximately half the points to increase the spacing to $2\Delta x$. Comparing the values of the two integrals gives an estimate of the error. You'll work through this idea in this question.

(a) First, explain why the errors associated with the trapezoidal rule and Simpson's rule scale $\propto (\Delta x)^2$ and $(\Delta x)^4$ respectively.

(b) If the value for the integral when all the points are used (spacing $\Delta x$) is $I_1$, and the value for the integral when half the points are used (spacing $2\Delta x$) is $I_2$, show that the error in $I_1$ can be written 

$$\epsilon_1\approx {I_1-I_2\over 3}$$

for the trapezoidal rule, and 

$$\epsilon_1\approx {I_1-I_2\over 15}$$

for Simpson's rule. [*Hint*: write the true value of the integral as $I=I_1+\epsilon_1=I_2+\epsilon_2$ and use the fact that you know how the errors scale with $\Delta x$.]

(c) The file [`hw2_data.txt`](https://github.com/andrewcumming/phys512/blob/main/hw2_data.txt) has 21 samples of a function $f(x)$ between $x=1$ and $x=2$. Column 1 in the file gives the $x$ values and column 2 the corresponding $f(x)$ values.  Write a code that reads in the data from the file, integrates the function from $x=1$ to $x=2$, and then carries out the procedure above to estimate the error in the integration. [*Hint*: One option to read in the data is to use  [`numpy.loadtxt`](https://numpy.org/doc/stable/reference/generated/numpy.loadtxt.html) which can read the data straight into numpy arrays.]

Do this for both the trapezoidal and Simpson's rules. Discuss whether the answer you get for the error makes sense given what you know about the accuracy of the trapezoidal rule and Simpson's rule.

(d) Because I generated the data points in the file, I know the true underlying function and its integral -- the true value of the integral is `1.482974344768713` (as obtained by `scipy.integrate.quad`). Use this value to calculate the true error of the trapezoidal and Simpson's rule integrals and compare with your estimate from part (c). Does our estimate procedure from part (b) do a good job at estimating the error?

(e) *Optional*: An extension of this question is to use this idea to write an adaptive integrator that takes a function $f(x)$ and iteratively increases the number of integration points until a specified precision is reached.


2. **Chemical potential of a Fermi gas** 

In statistical mechanics, the chemical potential $\mu$ of a gas of $N$ non-interacting fermions in a volume $V$ is given by the integral

$$N = V \int_0^\infty {8\pi p^2 dp\over h^3} {1\over 1+e^{(\epsilon-\mu)/k_BT}},$$

where you can assume that the particles are non-relativistic so $p = \sqrt{2m\epsilon}$. The goal in this question is to numerically invert this integral to return the chemical potential $\mu$ as a function of number of particles $N$ and temperature $T$.

(a) Numerically evaluate the integral for different values of $\mu/k_BT$ (from large and negative to large and positive) and set up an interpolating function that returns $\mu/k_BT$ as a function of $N/n_QV$ where 
$n_Q=(m k_BT/2\pi\hbar^2)^{3/2}$. [You may find it helpful to simplify the integral first, for example to write the integration variable as $x=\epsilon/k_BT$.]

(b) Assess the errors in your calculation: describe the sources of error and estimate the accuracy of your final value of $\mu(N, T)$.

(c) Compare your results with the analytic limits of non-degenerate $\mu = k_BT \ln (n/2n_Q)$ and degenerate $\mu=E_F$ fermions (where $E_F = p_F^2/2m$ with $p_F=\hbar(3\pi^2 n)^{1/3}$ is the Fermi energy). Determine the region of parameter space where each of these limits is accurate to 1\%. 


3. **Sampling the Maxwell-Boltzmann distribution**

(a) Write a function `MaxwellBoltzmann(N)` that returns a numpy array containing `N` samples of velocity from a 3D Maxwell-Boltzmann distribution. Your function (1) should be based on [`numpy.random.Generator.uniform`](https://numpy.org/doc/stable/reference/random/generated/numpy.random.Generator.uniform.html) or equivalent that samples a uniform probability distribution, and (2) should not use any for loops. You should be able to generate a million samples in less than a second of runtime. In your answer, explain your algorithm.

(b) Generate a large number of samples and compare a histogram of the velocities with the analytic distribution on a log-log plot. [You can use [`matplotlib.pyplot.hist`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist.html) to draw the histogram.]

(c) Compute the average velocity of your sample, and compare with the analytic result $(8k_BT/\pi m)^{1/2}$. How does the error in the average velocity depend on the number of samples that you take from the distribution?

<!---
# 3. **Changing variables to handle an infinite range**
# -  Rational function fit used to fit a function with poles ?
# - Gauss Chebyshev quadrature
# - Select a particle 
-->