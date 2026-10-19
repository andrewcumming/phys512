# Midterm solutions

## 1. Floating-point precision

(a) What does it mean to say that the floating point precision of 64-bit floating point numbers is $\epsilon=2.2\times 10^{-16}$?

:::{seealso} Solution
:class: dropdown
The precision represents the smallest fractional difference between two floating point numbers that can be achieved. It is a consequence of the finite number of bits used to store the mantissa (52 bits so $\epsilon = 2^{-52}$).
:::

(b) The derivative $df/dx$ of a function $f(x)$ can be calculated using the first order difference
$${df\over dx} = {f(x+\Delta x) - f(x)\over \Delta x}.$$

Explain why there is an optimal choice for $\Delta x$ to achieve the smallest error possible in $df/dx$, and derive an expression for the optimal $\Delta x$. What is the order of magnitude of the best fractional error in $df/dx$ you could achieve if the floating point precision is $\epsilon =2.2\times 10^{-16}$?

:::{seealso} Solution
:class: dropdown
  A smaller $\Delta x$ makes the finite difference approximation more accurate, with error in $f(x+\Delta x)-f(x)$ coming from the next term in the Taylor expansion $\approx {f^{\prime\prime}\over 2} (\Delta x)^2$. When divided by $\Delta x$ to form the derivative, this error is $\propto \Delta x$. However, the difference $f(x+\Delta x)-f(x)$ also has a roundoff error which is $\approx \epsilon f$. When divided by $\Delta x$ to form the derivative, this error grows $\propto 1/\Delta x$.

 The optimal $\Delta x$ comes from minimizing the error $${d\over d\Delta x}\left({\epsilon f\over \Delta x} + {f^{\prime\prime}\over 2}(\Delta x)\right) =0$$ $$\Rightarrow - {\epsilon f\over (\Delta x)^2} + {f^{\prime\prime}\over 2} = 0$$ $$\Rightarrow \Delta x\approx \left({2\epsilon f\over f^{\prime\prime}}\right)^{1/2}.$$

  With this value of $\Delta x$, the error in the derivative is
  $${\epsilon f\over \Delta x} + {f^{\prime\prime}\over 2}\Delta x = 2\left({f^{\prime\prime}f\epsilon\over 2}\right)^{1/2}.$$

Dividing by $df/dx$, we see that the fractional error in the derivative is
 $$\approx \sqrt{2\epsilon} \left({f^{\prime\prime}f\over (f^{\prime})^2}\right)^{1/2}$$

which is of order $10^{-8}$.
:::
  
(c) Estimate to an order of magnitude how much better you could do by going to a second order expression for the first derivative instead.

:::{seealso} Solution
:class: dropdown
With a second order expression for the derivative, the truncation error is now $\propto (\Delta x)^2$. The optimal $\Delta x$ will then scale like $\epsilon^{1/3}$ instead of $\epsilon^{1/2}$. The error in the derivative would then be $\sim \epsilon^{2/3}\sim 10^{-32/3}\sim 10^{-11}$. Going to higher order has a smaller pay off each time.
:::


## 2. Advection equation

The advection equation
$${\partial f\over\partial t} = - v {\partial f\over \partial x}$$
arises in many problems and physically represents the translation of the quantity $f$ in the $x$-direction at velocity $v$ (e.g. something being carried by a fluid). Here, assume $v$ is a positive constant. One way to solve this with finite differences is called upwind differencing:
$$f^{n+1}_j = f^n_j -\alpha (f^n_j - f^n_{j-1})$$
where $\alpha = v\Delta t/\Delta x$ depends on the timestep $\Delta t$ and grid spacing $\Delta x$.

Use von Neumann stability analysis to find the maximum value of $\alpha$ for this method to be stable. In the unstable case, what is the most unstable wavelength?

:::{seealso} Solution
:class: dropdown

von Neumann stability analysis: write $f^n_j = \xi^n e^{i k j\Delta x}$.

This gives
$$\xi = 1 - \alpha(1 - e^{-ik\Delta x}) = 1 - \alpha + \alpha \cos k\Delta x - i\alpha \sin k\Delta x$$

  We need $|\xi|\leq 1$ for stability.
  
 $$|\xi|^2 = (1-\alpha(1-\cos k\Delta x))^2 + \alpha^2\sin^2 k\Delta x $$
  $$=1 - 2\alpha (1-\cos k\Delta x) + \alpha^2(1-\cos k\Delta x)^2 + \alpha^2\sin^2 k\Delta x$$
  $$ = 1- 2\alpha(1-\cos k\Delta x) + \alpha^2(2 - 2\cos k\Delta x)$$
$$ = 1 - (2\alpha - 2\alpha^2) (1-\cos k\Delta x).$$
$$ = 1 - 4\alpha(1 -\alpha) \sin^2 \left({k\Delta x\over 2}\right).$$

Since $\alpha$ and the $\sin^2$ term are both positive, this shows that we need the $1-\alpha$ term to also be positive to keep $|\xi|<1$. Therefore the method is stable if $\alpha<1$.

  If $\alpha>1$, then we end up with $|\xi|>1$ because the second term becomes overall positive. The worst case is when $\sin(k\Delta x/2) =1$ (or $\cos k\Delta x = -1$). The most unstable mode will therefore be when $k\Delta x = \pi$ or a wavelength $\lambda = 2\Delta x$. 
:::


## 3. Boundary conditions for diffusion

The following matrices

$$
A = \begin{pmatrix}
1 & 0 & 0 & 0 & 0 \\
\alpha & 1-2\alpha & \alpha & 0 & 0\\
0 & \alpha & 1-2\alpha & \alpha & 0\\
0 & 0 & \alpha & 1-2\alpha & \alpha \\
0 & 0 & 0 & 0 & 1\\
\end{pmatrix}
$$

$$
A = \begin{pmatrix}
1-\alpha & \alpha & 0 & 0 & 0 \\
\alpha & 1-2\alpha & \alpha & 0 & 0\\
0 & \alpha & 1-2\alpha & \alpha & 0\\
0 & 0 & \alpha & 1-2\alpha & \alpha \\
0 & 0 & 0 & \alpha & 1-\alpha\\
\end{pmatrix}
$$
and
$$
A = \begin{pmatrix}
1-2\alpha & \alpha & 0 & 0 & 0 \\
\alpha & 1-2\alpha & \alpha & 0 & 0\\
0 & \alpha & 1-2\alpha & \alpha & 0\\
0 & 0 & \alpha & 1-2\alpha & \alpha \\
0 & 0 & 0 & \alpha & 1-2\alpha\\
\end{pmatrix}
$$
are possible matrices that could be used in an explicit update for the diffusion equation, $\mathbf{T}^{n+1} = \mathbf{A}\mathbf{T}^n$.  Here $\alpha=D\Delta t/(\Delta x)^2$, and for simplicity I've shown the matrices for $N=5$ grid points.

(a) Determine the boundary conditions that have been assumed in each case.

:::{seealso} Solution
:class: dropdown
We need to look at the first and last rows of the matrix and see what is happening at the boundaries. In fact, since the matrices are symmetric, we can just look at the first row.

  In the first case, the update for grid point 1  is
  $$T^{n+1}_1 = T^n_1$$
  the temperatures at the first and last grid points do not change! So whatever they are set to in the initial condition will persist.

  In the second case, we have
  $$T^{n+1}_1 = (1-\alpha)T_1^n + \alpha T_2^n$$
  Since the general update would give
  $$T^{n+1}_1 = \alpha T^n_0 + (1-2\alpha)T_1^n + \alpha T_2^n$$
  then we can see that it has been assumed that $T_1^n=T_0^n$, which implies zero gradient at the boundaries (insulating boundaries).

  In the third case, we have
  $$T^{n+1}_1 = (1-2\alpha)T_1^n + \alpha T_2^n$$
  which is equivalent to the general update but with $T_0^n=0$. So these are boundaries with $T=0$ just off the grid.
:::

(b) Explain how these matrices would change if an implicit update scheme was used instead.

:::{seealso} Solution
:class: dropdown
For an implicit update, we would change the $n$ superscript on the right-hand side 
for an $n+1$ in the terms $\propto\alpha$, and the matrix would then be applied on the left-hand side, $\mathbf{A}\mathbf{T}^{n+1} = \mathbf{T}^n$. This would be the case for all rows, including the boundary row.
So in each case this would give the same matrix, just with a sign change $\alpha\rightarrow -\alpha$.

(If you write this as a matrix applied on the right-hand side then you would need to take the inverse of these matrices).
:::


## 4. Non-linear ODEs

Consider the set of ODEs
$${d\mathbf{y}\over dt} = \mathbf{f}(\mathbf{y})$$
where $\mathbf{f}(\mathbf{y})$ are nonlinear functions of $\mathbf{y}$.

(a) Explain how you could use the Newton iteration $\mathbf{J}\Delta \mathbf{y} = -\mathbf{F}$ as part of an algorithm to integrate these equations forward in time.

:::{seealso} Solution
:class: dropdown

To integrate these in time, we can use an implicit time step:
  $$\mathbf{y}^{n+1} = \mathbf{y}^n + h \mathbf{f}(\mathbf{y}^{n+1})$$
  (writing $h$ for the size of the time step $\Delta t$).
  This gives a non-linear equation to solve for $\mathbf{y}^{n+1}$. Newton's method is a way to solve the non-linear equation at each time step. We first make a guess (e.g. take the $\mathbf{y}$ values from the previous time step, and then iteratively update using the Newton iteration given in the question. Here the vector $\mathbf{F}$ is the vector of quantities we need to zero
  $$\mathbf{F} = \mathbf{y}^{n+1} - \mathbf{y}^n - h \mathbf{f}(\mathbf{y}^{n+1}),$$
  and the elements of the Jacobian matrix are 
  $$J_{ij} = {\partial F_i\over \partial y_j}.$$
:::

(b) A famous set of nonlinear ODEs are the Lorenz equations
$${dx\over dt} = \sigma (y-x)$$
$${dy\over dt} = x(\rho -z) - y$$
$${dz\over dt} = xy - \beta z,$$
where $\sigma$, $\rho$ and $\beta$ are constant parameters (these equations lead to the butterfly attractor in chaos theory that you may have seen before). Derive expressions for the vector $\mathbf{F}$ and matrix $\mathbf{J}$ that you would use to solve the Newton iterations for this set of equations.

:::{seealso} Solution
:class: dropdown
  In this case, we have:
  $$\mathbf{F} = \begin{pmatrix}
    &x^{n+1}-x^n -h \sigma (y^{n+1}-x^{n+1})\\
    &y^{n+1}-y^n - h\rho x^{n+1} + h x^{n+1}z^{n+1} + h y^{n+1} \\
    &z^{n+1}-z^n - hx^{n+1}y^{n+1} + h\beta z^{n+1}
    \end{pmatrix}
$$

  and therefore

  $$\mathbf{J} = \begin{pmatrix}
    1+h\sigma & -h\sigma  & 0 \\
    -h\rho +hz^{n+1} & 1+h  & h x^{n+1}  \\
    -h y^{n+1} & -h x^{n+1} & 1+h\beta 
  \end{pmatrix}
  $$
:::
