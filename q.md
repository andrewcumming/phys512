# Practise questions

## Section A

1. You are given a set of samples from an MCMC code in the five dimensional space $(a_1$,.. $a_5)$. Describe how you would marginalize over $a_2$ to $a_5$ to obtain the marginalized probability distribution for $a_1$.

2. Discuss the differences in how you would approach solving a linear vs. a nonlinear equation.

3. You are solving the advection equation on a grid in $x$ with constant spacing $\Delta x$. A von Neumann stability analysis of your numerical scheme gives you the result
$$\xi = 1- \alpha(1-e^{-ik\Delta x}).$$
What values of $\alpha$ give a stable method?

4. The Euler-Cromer update for the simple harmonic oscillator is
$$v_{n+1} = v_n - h x_n$$
$$x_{n+1} = x_n + h v_{n+1}.$$
Show that this update is (i) symplectic and (ii) conserves the quantity $$x^2+v^2-hxv$$. Explain why this is useful.


## Section B

For each of the following situations, explain in as much detail as you can
how you would solve the problem. You do not need to write out any code
in your answer, but you should describe in detail the algorithm you would
use, and there should be enough detail in your answer for someone to code
up the solution. For example, where appropriate you should write out the
differencing scheme you would use, the form of any matrices involved, or
describe how you would choose the values of timestep or spatial resolution
or other parameters.

1. You need to fit a non-linear model with 5 parameters to a set of 1000 data points, and obtain an estimate for the mean of each parameter and its error bar.

2. You want to solve for the eigenmodes of a string with mass density per unit length given by an arbitrary function $\rho(x)$. The string is clamped at one end ($x=0$) and left free at the other ($x=1$).

