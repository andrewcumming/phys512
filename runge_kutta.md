# Initial value problems

In an initial value problem, we want to solve a set of ordinary differential equations 

$${d\vec{y}\over dx} = f(x,\vec{y})$$

for the functions $\vec{y}$ given a set of boundary conditions at the starting point $x=x_0$. We then use the information we have about the derivatives $f(x,\vec{y})$ to step away from $x_0$ and evaluate $\vec{y}$ at a location $x=x_0+h$, where $h$ is the step size (we could also write this as $\Delta x$, but we'll use $h$ here in common with many treatments of ODEs). Note that the derivative $f$ can depend on the position $x$ but also any of the $y$'s, ie. we could have a set of *coupled ODEs*. 

## Explicit methods

Explicit methods involve using the value of the function $y(x)$ and the derivative $f(x,y)$ at the current $x$ location to evaluate $y(x+h)$. There are various approximations that can be used with increasing accuracy:

### Euler method

The simplest approach is to write

$$y(x+h) = y(x) + hf(x,y),$$

i.e. a first order Taylor expansion about the point $x$. 

Note that this scheme has a **local error** which is *second order* in $h$, since the next term in the Taylor expansion that has been dropped is $\propto h^2$. However, in integrating between two points $x=a$ and $x=b$, the number of steps that we have to take grows as $h$ decreases, so that the **global error** is one power of $h$ larger. For this reason, the Euler method is a **first order** method: doubling the number of points decreases the error by a factor of 2.
 
### Midpoint method

In the midpoint method, instead of using the derivative at $x$ to step across the interval, we use the derivative from half-way across, at $x+h/2$. This gives a **second order** method. The procedure is

1. Evaluate

$$y_1 = y(x) + {h\over 2}f(x,y).$$

2. Calculate the derivative at the half-way point

$$f_1 = f(x+h/2, y_1).$$

3. Use that to step across the entire interval

$$y(x+h) = y(x) + h f_1.$$


### Runge-Kutta

The midpoint method is an example of a broader class of methods known as **Runge Kutta** methods. The idea is to use combinations of derivatives evaluated across the interval $h$ to cancel out the higher order terms. The most used of these is the **4th order Runge Kutta** method (RK4). Together with a routine to take adaptive steps (i.e. vary the stepsize $h$ to achieve a desired accuracy), this might be the only method you need for integrating ODEs (unless you are dealing with a stiff system, see implicit methods below). It is common enough that you should certainly know about RK4 and how it works.

The procedure is similar to the midpoint method, but with an extra step:

1. Step to the halfway point and evaluate the derivative there:

$$y_1 = y(x) + {h\over 2}f(x,y)$$
$$f_1 = f(x+h/2, y_1)$$

2. Now repeat, but this time use $f_1$ to step to the halfway point and re-evaluate the derivative:

$$y_2 = y(x) + {h\over 2}f_1$$
$$f_2 = f(x+h/2, y_2)$$

3. Now use $f_2$ to step across the entire interval and evaluate the derivative there:

$$y_3 = y(x) + h f_2$$
$$f_3 = f(x+h/2, y_3)$$

4. Finally, the value of $y$ at $x+h$ is given by

$$y(x+h) = y(x) + {h\over 6}\left(f_1 + 2f_2 + 2f_3 +f_4\right).$$

This formula might remind you of Simpson's rule (which similarly also has a combination of terms that cancel the third order error terms).

````{admonition} Exercise: Runge-Kutta orbit integrations

The set of coupled ODEs

$${d^2 x\over dt^2} = -{x\over (x^2+y^2)^{3/2}}$$
$${d^2 y\over dt^2} = -{y\over (x^2+y^2)^{3/2}}$$

are the equations that describe a circular orbit with period $2\pi$, i.e. $x(t)$ and $y(t)$ are a circle with unit radius and the motion around the circle takes a time $2\pi$. (These are the same equations you solved in Homework 1 question 1 but non-dimensionalized.)

We can solve these equations by first writing them as 1st order ODEs:

$${dx\over dt} = u_x$$
$${dy\over dt} = u_y$$
$${du_x\over dt} = -{x\over (x^2+y^2)^{3/2}}$$
$${du_y\over dt} = -{y\over (x^2+y^2)^{3/2}}$$

- Write a code to integrate these equations around one orbit. For example, you can take initial conditions $(x,y, u_x,u_y)=(1,0,0,1)$. After integrating for a time $2\pi$, $x$ and $y$ should have returned to their starting values. The value of $y(t=2\pi)$ is then a measure of the error in the method (since it should have returned to zero at the end, so any non-zero value is due to the error in the integration).

- Do the integration using the three different methods above. Make a plot of error vs number of steps $N$ and check the scalings with $N$ are as you expect.

Hint: try to write your integrator in as general a way as possible and take advantage of the vector expressions in numpy. For example, here is a function that does an Euler integration, given the parameters `nsteps` (number of integration steps to take), `dt` (step-size), `x0` (vector of initial values of the 4 variables), and `derivs` (the name of a function that calculates a vector of derivatives).
```
    def integrate_euler(nsteps, dt, x0, derivs):
    x = np.zeros((nsteps, len(x0)))
    x[0] = x0
    for i in range(1,nsteps):    
        f = derivs(i*dt, x[i-1])
        x[i] = x[i-1] + f*dt
    return x
```
Note that this is written assuming that the integration starts at $t=0$, but you could easily generalize that. We have also allowed for a possible $t$ dependence of the derivative, although in this orbit problem, the derivatives do not depend explictly on time.
````


## Implicit methods and stiff equations

Explicit methods often have a maximum step size $h$ beyond which the method becomes unstable. A simple example is the equation 

$${dy\over dx} = -cy$$

for some constant $c>0$ which has a solution $e^{-cx}$. The Euler method gives 

$$y(x+h) = y(x) + h\left.{dy\over dx}\right|_{x} = y(x) - ch y(x) = (1- ch) y(x).$$

If we try to take large steps $h>1/c$, the solution will oscillate with $|y|$ becoming larger and larger: this method is *numerically unstable*.  For this particular equation, that's okay, since we would want to take smaller steps anyway to get an accurate solution. But this really matters when there are multiple scales in the solution. We might have a set of equations in which one equation has a large value of c that forces us to take a small $h$ for numerically stability even if the value of the corresponding function $y$ has decayed away and is no longer important. A set of equations like this with multiple scales is a **stiff** set of equations. 

A classic example with a stiff set of equations is radioactive decay. Consider the decay chain 

$$^{224}\mathrm{Ra} \xrightarrow{3.6\ \mathrm{days}} \ ^{220}\mathrm{Rn} \xrightarrow{55\ \mathrm{s}} \ ^{216}\mathrm{Po}  \xrightarrow{0.14\ \mathrm{s}}\  ^{212}\mathrm{Pb}\xrightarrow{10.6\ \mathrm{h}}\  ^{208}\mathrm{Pb}$$

which is used in radiotherapy. The half-lives span the range $\approx 0.1$--$3\times 10^5$ seconds, or a factor of $3\times 10^6$. With an explicit method, we would be forced to take a timestep $<0.1\ \mathrm{s}$ to resolve the fastest decay time, and so would need millions of timesteps to follow the entire chain. 

**Implicit methods** allow us to get around this constraint: we can take large steps in a stable way and focus on the large scales that we are interested in.
In an implicit method, we write the update in terms of the gradient evaluated with the future values of the variables rather than the current values, ie. we use $f(x+h, y(x+h))$ rather than $f(x, y(x))$:

$$y(x+h) = y(x) + h \left.{dy\over dx}\right|_{x+h} $$ (backeuler)

which gives 

$$y(x+h) = y(x) - chy(x+h) \Rightarrow y(x+h) = {y(x) \over 1+ ch}$$

This behaves well in the limit of large $h$ since then $y(x+h)\rightarrow 0$, which is the correct solution for large $x$. Implicit methods usually have this behavior: for large steps we lose accuracy, but evolve to the correct equilibrium solution. 


### Backward Euler

Equation {eq}`backeuler` is known as the **backward Euler** method (as opposed to the explicit *forward Euler* method that we saw earlier). This can be generalized to sets of linear and non-linear equations:

**Linear equations**: For a general set of linear equations with constant coefficients,

$${d\mathbf{y}\over dx} = \mathbf{y}^\prime =  - \mathbf{C} \mathbf{y},$$

where $\mathbf{C}$ is a positive-definite matrix, the update is 

$$\mathbf{y}_{n+1} = \mathbf{y}_{n} + h  \mathbf{y}^\prime_{n+1}$$

$$\Rightarrow \mathbf{y}_{n+1} = (1 + \mathbf{C} h)^{-1} \mathbf{y}_{n},$$

which is stable for all step sizes $h$. The price for being able to take larger steps is a more complex computation: we have to invert a matrix.

**Non-linear equations**: A more complicated situation is when the derivatives are non-linear, 

$$\mathbf{y}^\prime =  \mathbf{f}(\mathbf{y}),$$ 

where we must now solve the implicit equation

$$\mathbf{y}_{n+1} = \mathbf{y}_n + h \mathbf{f} (\mathbf{y}_{n+1}).$$(nonlineareuler)

One way to approach this is to linearize the equations

$$\mathbf{y}_{n+1} = \mathbf{y}_n + h\left[ \mathbf{f} (\mathbf{y}_n) + \left.{\partial \mathbf{f}\over\partial\mathbf{y}}\right|_{\mathbf{y_n}}(\mathbf{y}_{n+1}-\mathbf{y}_n)\right]$$

$$\Rightarrow \mathbf{y}_{n+1} = \mathbf{y}_n + h \left[1 - h  \left.{\partial \mathbf{f}\over\partial\mathbf{y}}\right|_{\mathbf{y_n}}\right]^{-1}\mathbf{f}(\mathbf{y}_n).$$(newtoneuler)

This is another example of **Newton's method** that we saw earlier. The matrix $\partial \mathbf{f}/\partial\mathbf{y}$ is the **Jacobian matrix** 

$$(\mathbf{J})_{ij} = \left( {\partial \mathbf{f}\over\partial\mathbf{y}}\right)_{ij} = {\partial\over \partial y_j}{\partial f_i\over \partial x}.$$

Sometimes equation {eq}`newtoneuler` will converge in one step, but more than one iteration may be required to get an accurate answer, i.e. you can apply equation {eq}`newtoneuler` multiple times. You can check after the Newton step to see whether equation {eq}`nonlineareuler` is satisfied.

```{admonition} Exercise: implicit methods

Write a code to integrate the set of equations

$$u^\prime = 998u + 1998 v$$
$$v^\prime = -999u - 1999v$$

from $x=0$ to larger values of $x$, with boundary conditions $u(0)=1$ and $v(0)=0$. This example is from the Numerical Recipes book where they show that the analytic solution is $(u,v) = (2e^{-x} - e^{-1000} x, -e^{-x} + e^{-1000 x})$. 

Use both an explicit and implicit method with fixed step size $h$ and compare the results from the two methods for different choices of $h$. 


```

## Integrating ODEs using Scipy

You can use [scipy.integrate.solve_ivp](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html) to solve initial value problems. It has a choice of different  explicit and implicit methods that you can use, and has an adaptive step size that you can control by specifying the absolute or relative tolerance (error) for the integration. 

```{admonition} Exercise

Repeat the exercises above but now using [scipy.integrate.solve_ivp](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html).

```

