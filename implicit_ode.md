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

**Non-linear equations**: A more complicated situation is when the derivatives are non-linear, 

$$\dot{\mathbf{y}} =  \mathbf{f}(\mathbf{y}),$$ 

where we must now solve the implicit equation

$$\mathbf{y}_{n+1} = \mathbf{y}_n + h \mathbf{f} (\mathbf{y}_{n+1}).$$(nonlineareuler)

One way to approach this is to linearize the equations

$$\mathbf{y}_{n+1} = \mathbf{y}_n + h\left[ \mathbf{f} (\mathbf{y}_n) + \left.{\partial \mathbf{f}\over\partial\mathbf{y}}\right|_{\mathbf{y_n}}(\mathbf{y}_{n+1}-\mathbf{y}_n)\right]$$

$$\Rightarrow \mathbf{y}_{n+1} = \mathbf{y}_n + h \left[\mathbf{1} - h  \left.{\partial \mathbf{f}\over\partial\mathbf{y}}\right|_{\mathbf{y_n}}\right]^{-1}\mathbf{f}(\mathbf{y}_n).$$(newtoneuler)

This approach is known as **Newton's method**. The matrix $\partial \mathbf{f}/\partial\mathbf{y}$ is the **Jacobian matrix** 

$$(\mathbf{J})_{ij} = \left( {\partial \mathbf{f}\over\partial\mathbf{y}}\right)_{ij} = {\partial f_i\over \partial y_j}.$$

Sometimes the Newton iteration {eq}`newtoneuler` will converge in one step, but more than one iteration may be required to get an accurate answer, i.e. you can apply equation {eq}`newtoneuler` multiple times, each time taking the values $y_{n+1}$ that come out on the left hand side as the new values $y_n$ to put in on the right hand side. You can check after the Newton step to see whether equation {eq}`nonlineareuler` is satisfied.

:::{admonition} Exercise: implicit methods
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
