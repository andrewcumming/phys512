# Homework 4

Due on Thursday October 29 by 11.59pm.

## 1. Time-independent Schrödinger equation

Use [`scipy.linalg.eigh`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.eigh.html) to solve the time-independent Schrödinger equation

$$-{\hbar^2\over 2m} {d^2\psi\over dx^2} + V(x)\psi = E\psi,$$ 

for a particle moving in the 1D potential 

$$V(x) = - V_0\exp\left(-(x/a)^2\right)$$

($-\infty<x<\infty$). (You might find it helpful to look back at the solution for [waves on a string](solutions-bvp#waves-on-a-string-using-scipy-linalg-eigh).)

Choose a value of $V_0$ for which there is a bound state, and plot the ground state wavefunction $\psi_0(x)$. Obtain a numerical value for the energy of the ground state in appropriate units that is accurate to at least 1 part in 1000. (You should demonstrate this by obtaining a numerical estimate of the energy error.)

## 2. Separable wavefunctions

(a) For the singular value decomposition of a matrix $\mathbf{A} = \mathbf{U}\mathbf{S}\mathbf{V^T}$, show that $\mathbf{A}$ can also be written as

$$\mathbf{A} = \sum_\alpha s_\alpha \mathbf{u_\alpha} \mathbf{v_\alpha^T},\label{eq:sum}$$

where $s_\alpha$ are the singular values (that make up the diagonal of $\mathbf{S}$), and $\mathbf{u_\alpha}$ and $\mathbf{v_\alpha}$ are the column vectors that make up the matrices $\mathbf{U}$ and $\mathbf{V}$. This shows that the matrix $\mathbf{A}$ can be decomposed into a linear combination of the matrices formed by the outer product of each pair of $\mathbf{u_\alpha}$ and $\mathbf{v_\alpha}$ vectors. (These are rank-1 matrices since they are constructed from a single column and row vector.)

(b) To test this result, construct a $10\times 6$ matrix $\mathbf{A}$ filled with random numbers (uniform between 0 and 1) and compute the component matrices $\mathbf{u_\alpha} \mathbf{v_\alpha^T}$. Plot color maps of the component matrices (you can use the function `plot_matrices` from the notes on [Laplace's equation](laplace#multigrid-method)). Compute the sum from part (a) and confirm that it gives you the original matrix.

(c) Consider the 2D wavefunction
$$\psi(x,y) = C e^{-(x^2+y^2+\alpha x y)/2},$$
where the parameter $\alpha$ is in the range $-2<\alpha<2$. Sample $\psi(x,y)$ on an $n\times n$ grid in $x$ and $y$ (and optionally you can normalize it once you have done so). Then, compute the SVD of the resulting matrix. Plot the singular values and also explore how well the sum in equation {eq}`eq:sum` reproduces the wavefunction if you truncate the sum after different numbers of terms.

(d) How do your results depend on $\alpha$? In particular, what happens when you take $\alpha=0$? Explain what you find and comment on the use of SVD as a measure of separability of a wavefunction.

## 3. Lorenz equations

The Lorenz equations
$${dx\over dt} = \sigma (y-x)$$
$${dy\over dt} = x(\rho -z) - y$$
$${dz\over dt} = xy - \beta z,$$
were originally developed as a simplified model of atmospheric convection. They are an important example of a dynamical system that shows chaotic behavior, in particular sensitivity to initial conditions.

(a) Use backwards-Euler timestepping in conjunction with Newton's method to integrate these equations in time (you can use the analytic Jacobian from the midterm). Choose parameters $\sigma=10$, $\rho=28$, and $\beta=8/3$. For the initial condition, you can take $(x,y,z)=(1,0,0)$. Integrate to $t=3$ and plot $x(t)$, $y(t)$ and $z(t)$ against time.

How small a timestep $h$ do you need to get a converged solution? For example, try $h=10^{-2}$, $10^{-3}$, $10^{-4}$ and $10^{-5}$ and compare the results. Does the solution converge with the scaling with $h$ that you would expect?

(b) Now extend your integrations to $t=30$. What do you notice now about the behavior of the solutions as you make $h$ smaller? Can you explain what is happening?

(c) Plot the $(x,y,z)$ trajectories for several different timesteps on the same 3D plot. You should see the famous butterfly-shaped Lorenz attractor. Although the trajectories for different $h$ diverge from one another, do they trace out the same attractor? What do you think might be a useful measure of convergence for a chaotic system if the detailed trajectories themselves do not converge?






