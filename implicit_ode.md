# Implicit methods and stiff equations

Explicit methods often have a maximum step size $h$ beyond which the method becomes unstable. A simple example is exponential decay

$${dy\over dt} = -cy$$

for some constant $c>0$ which has a solution $e^{-ct}$. The Euler method
$$y(t+h) = y(t) + h\left.{dy\over dt}\right|_{t}$$
then gives
$$y(t+h) = y(t) - ch y(t) = (1- ch) y(t).$$

For small steps such that $ch\ll 1$, each step involves decreasing $y$ by a fixed fractional amount, exactly what we need for exponential decay.  However, if we try to take large steps $h>1/c$, the multiplier becomes negative, and the solution will oscillate. If $h>2/c$, $|y|$ will grow larger and larger: this method is *numerically unstable* for large timesteps.  For this particular equation, that's okay, since we would want to take smaller steps anyway to get an accurate solution. But this really matters when there are multiple scales in the solution. We might have a set of equations in which one equation has a large value of c that forces us to take a small $h$ for numerically stability even if the value of the corresponding function $y$ has decayed away and is no longer important. A set of equations like this with multiple scales is a **stiff** set of equations. 

A classic example with a stiff set of equations is radioactive decay. Consider the decay chain 

$$^{224}\mathrm{Ra} \xrightarrow{3.6\ \mathrm{days}} \ ^{220}\mathrm{Rn} \xrightarrow{55\ \mathrm{s}} \ ^{216}\mathrm{Po}  \xrightarrow{0.14\ \mathrm{s}}\  ^{212}\mathrm{Pb}\xrightarrow{10.6\ \mathrm{h}}\dots $$

which is used in radiotherapy. The half-lives span the range $\approx 0.1$-$3\times 10^5$ seconds, or a factor of $3\times 10^6$. With an explicit method, we would be forced to take a timestep $<0.1\ \mathrm{s}$ to resolve the fastest decay time, and so would need millions of timesteps to follow the entire chain. 

**Implicit methods** allow us to get around this constraint: we can take large steps in a stable way and focus on the large timescales that we are interested in.
In an implicit method, we write the update in terms of the gradient evaluated with the future values of the variables rather than the current values, ie. we use $f(t+h, y(t+h))$ rather than $f(t, y(t))$:

$$y(t+h) = y(t) + h \left.{dy\over dt}\right|_{t+h} $$ (backeuler)

which gives 

$$y(t+h) = y(t) - chy(t+h) \Rightarrow y(t+h) = {y(t) \over 1+ ch}.$$

For small $ch$, this is equivalent to the explicit update. But for large $h$ there is an important difference: the implicit update behaves well in the limit of large $h$ since then $y(t+h)\rightarrow 0$, which is the correct solution for large times. Implicit methods usually have this behavior: for large steps we lose accuracy, but evolve to the correct equilibrium solution. 


### Backward Euler

Equation {eq}`backeuler` is known as the **backward Euler** method (as opposed to the explicit *forward Euler* method that we saw earlier). This can be generalized to sets of linear and non-linear equations:

**Linear equations**: For a general set of linear equations with constant coefficients,

$${d\mathbf{y}\over dt} = \dot{\mathbf{y}} =  - \mathbf{C} \mathbf{y},$$

where $\mathbf{C}$ is a positive-definite matrix, the update is 

$$\mathbf{y}_{n+1} = \mathbf{y}_{n} + h  \dot{\mathbf{y}}_{n+1} = \mathbf{y}_{n} - h \mathbf{C} {\mathbf{y}}_{n+1}$$

$$\Rightarrow (\mathbf{1} + \mathbf{C} h)\mathbf{y}_{n+1} = \mathbf{y}_n$$

$$\Rightarrow \mathbf{y}_{n+1} = (\mathbf{1} + \mathbf{C} h)^{-1} \mathbf{y}_{n},$$

which is stable for all step sizes $h$. Note that $\mathbf{1}$ here is the identity matrix. The price for being able to take larger steps is a more complex computation: we have to invert a matrix.



:::{admonition} Exercise: implicit method, linear case
:class: tip

Write a code to integrate the set of equations

$$\dot{u} = 998u + 1998 v$$
$$\dot{v} = -999u - 1999v$$

from $t=0$ to larger values of $t$, with boundary conditions $u(0)=1$ and $v(0)=0$. This example is from the Numerical Recipes book where they show that the analytic solution is $(u,v) = (2e^{-t} - e^{-1000 t}, -e^{-t} + e^{-1000 t})$. 

Use both an explicit and implicit method with fixed step size $h$ and compare the results from the two methods for different choices of $h$. 

Hints:

- you can create the matrix of coefficients using
```python
C = np.array( [[-998,-1998],[999,1999]] )
```
- The matrix multiplication operator is `@`, eg. `A@x` multiplies the vector `x` by matrix `A`
- The identity matrix of size $n\times n$ can be created using `np.identity(n)`
- You can use `np.linalg.inv()` to invert the matrix

:::



**Non-linear equations**: A more complicated situation is when the derivatives are non-linear, 

$$\dot{\mathbf{y}} =  \mathbf{f}(\mathbf{y}),$$ 

so that each timestep we must solve the implicit equation

$$\mathbf{y}_{n+1} = \mathbf{y}_n + h \mathbf{f} (\mathbf{y}_{n+1}).$$(nonlineareuler)

to find $\mathbf{y}_{n+1}$. 

To do this we can use **Newton's method**. Each timestep we need to find the solution $\mathbf{y}$ to the equation

$$\mathbf{F}(\mathbf{y}) = \mathbf{y} - \mathbf{y}_n - h \mathbf{f}(\mathbf{y}) = 0.$$(eq:Fy)

The idea is to start with a guess for the solution, $\mathbf{y}^{(0)}$, and then compute a correction $\Delta\mathbf{y}$ such that
$$\mathbf{F}(\mathbf{y}^{(0)}+ \Delta\mathbf{y}) =0.$$ 
In Newton's method, we find an approximate $\Delta\mathbf{y}$ by writing a linear expansion
$$\mathbf{F}(\mathbf{y}^{(0)}+ \Delta\mathbf{y}) \approx \mathbf{F}(\mathbf{y}^{(0)})+\Delta\mathbf{y}\left.{\partial\mathbf{F}\over \partial\mathbf{y}}\right|_{\mathbf{y}^{(0)}} =0.$$
With the Jacobian matrix $$\mathbf{J} = {\partial\mathbf{F}\over \partial\mathbf{y}},$$
which for our particular form for $\mathbf{F}(\mathbf{y})$ (given by {eq}`eq:Fy`) is
$$\mathbf{J} = \mathbf{1} - h{\partial \mathbf{f}\over \partial \mathbf{y}},$$
we can write this as
$$\mathbf{J}(\mathbf{y}^{(0)}) \Delta \mathbf{y} = -\mathbf{F}(\mathbf{y}^{(0)}).$$
The updated guess is then 
$$\mathbf{y}^{(1)} = \mathbf{y}^{(0)} + \Delta\mathbf{y}.$$
We can check how well this satisfies the equation by evaluating $\left|\mathbf{F}(\mathbf{y}^{(1)})\right|$ which would be zero if we had the correct solution. If the value of $\left|\mathbf{F}(\mathbf{y}^{(1)})\right|$ is close enough to zero, we can stop here. If not, we iterate again, solving
$$\mathbf{J}(\mathbf{y}^{(1)}) \Delta \mathbf{y} = -\mathbf{F}(\mathbf{y}^{(1)})$$
and updating 
$$\mathbf{y}^{(2)} = \mathbf{y}^{(1)} + \Delta\mathbf{y}.$$
We now check $\left|\mathbf{F}(\mathbf{y}^{(2)})\right|$ to see whether it is close enough to zero to stop. If not, we keep iterating. And so on.


:::{admonition} Exercise: implicit method, non-linear case
:class: tip

Solve the equations for a non-linear pendulum 

$$\dot{\theta} = \omega$$
$$\dot{\omega} = -\sin\theta$$

using the backward Euler method with Newton iterations at each timestep.

Plot $\theta(t)$ and plot $\theta$ against $\omega$. Explore the behavior for different timesteps and initial conditions. How many Newton iterations do you need to take at each time step?

Compare the results with forward Euler. What are the differences between the two methods?


Hints:

- It's useful to first show that

$$\mathbf{F}(\mathbf{y}) = \begin{pmatrix} \theta - \theta_n -h\omega \\ \omega - \omega_n + h \sin\theta \end{pmatrix}$$
where $\mathbf{y} = (\theta,\omega)$, and
$$\mathbf{J}(\mathbf{y}) = \begin{pmatrix} 1 & -h \\ h\cos\theta & 1  \end{pmatrix}.$$

- For your first guess at each timestep, you could just take $\mathbf{y}^{(0)} = \mathbf{y}_n$.

- To do the Newton iteration, you need to solve $\mathbf{J}\Delta\mathbf{y}=\mathbf{F}$. You could do this by inverting $\mathbf{J}$ directly or you could also call [`np.linalg.solve`](https://numpy.org/doc/stable/reference/generated/numpy.linalg.solve.html) to solve the equation for you.

:::



