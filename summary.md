# Summary

## Things you should know about or how to do

- [Floating point representation](https://andrewcumming.github.io/phys512/floats.html#how-floating-point-numbers-are-represented) and [roundoff error](https://andrewcumming.github.io/phys512/floats.html#roundoff-error): what sets the scale of roundoff errors and the maximum and minimum floats that can be stored.
- [Finite differencing](https://andrewcumming.github.io/phys512/derivatives.html) of derivatives, including the idea of the order of the approximation and the optimal step size
- [Interpolation](https://andrewcumming.github.io/phys512/interpolation.html): how to do simple linear interpolation; the difference between cubic interpolation and a cubic spline; how to do interpolation in >1D
- [Integration](https://andrewcumming.github.io/phys512/integration.html): Newton-Cotes methods; Gaussian quadrature and how to implement it; [how to estimate the error](https://andrewcumming.github.io/phys512/hw2.html#integration-errors-solution) in a numerical integration; [Monte-Carlo integration](https://andrewcumming.github.io/phys512/monte_carlo_integration.html) and its error
- Random numbers: [how to generate samples from a distribution](https://andrewcumming.github.io/phys512/generating_random.html), including built-in NumPy/SciPy functions, rejection/transformation/ratio of uniforms methods, or using [Metropolis-Hastings](https://andrewcumming.github.io/phys512/metropolis.html).
- [Matrix operations and decompositions](https://andrewcumming.github.io/phys512/matrices.html) in Numpy, especially SVD. The idea of condition number and how to calculate it.
- Newton's method for [minimizing a function](https://andrewcumming.github.io/phys512/nonlinear.html#newton-s-method) or [finding roots](https://andrewcumming.github.io/phys512/boundary_value_problems.html#relaxation-method)
- Integrating differential equations: the difference between [explicit and implicit methods](https://andrewcumming.github.io/phys512/runge_kutta.html). Advantages and disadvantages of both methods. What is meant by the term stiff equation. 
- Different types of errors: numerical instability, amplitude errors, phase errors, von Neumann stability analysis
- How to implement an adaptive step-size. The difference between absolute and relative errors.
- What makes an integrator [symplectic](https://andrewcumming.github.io/phys512/symplectic.html#symplectic-integrators). 
- Differences between initial value and boundary value problems. Basic idea of relaxation methods.
- Methods for solving the linear system $\mathbf{Ax} = \mathbf{b}$: [inversion including with SVD](https://andrewcumming.github.io/phys512/polynomial_fit.html#polynomial-fitting), [conjugate gradient](https://andrewcumming.github.io/phys512/conjugate_gradient.html#conjugate-gradient-method) for sparse matrices.
- How to implement boundary conditions in finite difference schemes
- The discrete Fourier transform: frequency spacing, maximum spacing, aliasing, leakage
- The difference between convolution and cross-correlation and how to implement them in Fourier space. Zero-padding and why/when it is needed. 



## Problems that we looked at

**Laplace's equation**

- [Relaxation method](https://andrewcumming.github.io/phys512/laplace.html#relaxation-jacobi-and-gauss-seidel
)
- [Multigrid method](https://andrewcumming.github.io/phys512/laplace.html#multigrid-method)
- [Conjugate gradient method](https://andrewcumming.github.io/phys512/conjugate_gradient.html#application-to-laplace-s-equation)

**Diffusion**

- As a random walk: [Diffusion-limited aggregation](https://andrewcumming.github.io/phys512/hw3.html#diffusion-limited-aggregation-solution)
- Solution by the [method of lines](https://andrewcumming.github.io/phys512/hw5.html#method-of-lines-solution)
and with [different boundary conditions](https://andrewcumming.github.io/phys512/numerical_instability.html)
- 2D diffusion using the [alternating-direction implicit method](https://andrewcumming.github.io/phys512/diffusion.html) 
- [2D diffusion using DFT](https://andrewcumming.github.io/phys512/fft_solutions.html#discrete-fourier-transform)
- Solution in 1D using [Chebyshev polynomials](https://andrewcumming.github.io/phys512/hw7.html)

**Advection-diffusion**

- Constant velocity with both [Fourier and finite difference](https://andrewcumming.github.io/phys512/advect_diffuse.html)
- [Burger's equation](https://andrewcumming.github.io/phys512/burgers.html#burgers-equation)

**Wave equation**

- Standing waves on a string: [shooting method](https://andrewcumming.github.io/phys512/boundary_value_problems.html#shooting-method), [relaxation method](https://andrewcumming.github.io/phys512/boundary_value_problems.html#relaxation-method) and as an [eigenvalue problem](https://andrewcumming.github.io/phys512/hw6.html#eigenvalue-problem-for-the-wave-on-a-string-solution)
- Time-dependent waves in 1D: [electromagnetic waves](https://andrewcumming.github.io/phys512/hw6.html#leapfrogging-an-electromagnetic-wave-solution)

**Thermodynamics**

- [Constructing an equation of state table](https://andrewcumming.github.io/phys512/hw1.html#interpolation-and-thermodynamics-solution)
- [Evaluating Fermi integrals](https://andrewcumming.github.io/phys512/hw2.html#chemical-potential-of-a-fermi-gas-solution)
- [Sampling a Maxwell-Boltzmann distribution](https://andrewcumming.github.io/phys512/hw2.html#sampling-the-maxwell-boltzmann-distribution-solution)

**Orbits**

- [Symplectic integration of orbits](https://andrewcumming.github.io/phys512/symplectic.html#euler-cromer-and-leapfrog-applied-to-orbits)
- [Adaptive RK4 integrator](https://andrewcumming.github.io/phys512/hw5.html#an-adaptive-runge-kutta-integrator-solution)

**Least-squares fitting**

- Linear least squares: [polynomial fitting as an example](https://andrewcumming.github.io/phys512/polynomial_fit.html#polynomial-fitting) including [with SVD](https://andrewcumming.github.io/phys512/polynomial_fit.html#using-singular-value-decomposition-svd)
- Non-linear least squares: [newton's method](https://andrewcumming.github.io/phys512/nonlinear.html#application-to-least-squares-fitting) and [Levenberg-Marquardt](https://andrewcumming.github.io/phys512/nonlinear.html#levenberg-marquardt); [MCMC](https://andrewcumming.github.io/phys512/metropolis.html#using-data-to-constrain-model-parameters-with-mcmc)
- Extracting a signal from noise: [LIGO example](https://andrewcumming.github.io/phys512/signal_processing.html#signal-processing)

