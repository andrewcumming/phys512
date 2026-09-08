# Explicit methods

We're going to start off by looking at **initial value problems**. A simple example is the motion of a particle of mass $m$ in 1-dimension due to a force $F$. The force could depend on position $x$, velocity $v=dx/dt$ or even explicitly on time. The time evolution is given by the two ordinary differential equations (ODEs)
$${dx\over dt} = v$$
$${dv\over dt} = {F(x,v,t)\over m}.$$
Given $x$ and $v$ at an initial time, we want to step forward in time and solve for the time evolution $x(t)$ and $v(t)$.

More generally, we can imagine a set of ODEs

$${d\vec{y}\over dt} = \vec{f}(t,\vec{y})$$

for the functions $\vec{y}(t)$. We know the value of $\vec{y}$ at the initial time, and want to evolve it forward in time. So for the example given above we have $\vec{y} = (x,v)$ and $\vec{f} = (v, F/m).$ Note that in a general problem each derivative $f_i$ can depend on the time $t$ but also any of the $y_i$'s, i.e. in general we have a set of *coupled ODEs*.

I've written the independent variable here as $t$ for time, since often in physics problems we are integrating some quantities $\vec{y}$ forwards in time. This is not always the case, however. The independent variable might be a spatial coordinate rather than time. We'll see later an example where we want to solve for the eigenfunction of a standing wave on a string, in which case we have a set of ODEs for the transverse displacement and transverse velocity of the string as a function of the coordinate $x$ along the string, and we want to integrate along the length of the string.

The way we can tackle this numerically is to use the information we have about the derivatives $\vec{f}(t,\vec{y})$ to update the values of $\vec{y}$ at the current time $t$ to their values at a later time $t+h$, where $h$ is the step size[^step_size]. We'll refer to this as "taking a timestep". 
[^step_size]: We could also write this as $\Delta t$, but we'll use $h$ here in common with many treatments of ODEs.

**Explicit methods** involve using the value of the function $y(t)$ and the derivative $f(t,y)$ at the current time $t$ to evaluate $y(t+h)$. In this way, the value at the next timestep $y(t+h)$ is written *explicitly* in terms of things that we know at the current timestep, hence the name *explicit method*. Because the gradient of the function changes as time evolves, we make an error by taking a discreet step $h$. How large an error depends on how exactly we do the timestep update. There are various approximations that can be used with increasing accuracy:

### Euler method

The simplest approach is to write

$$y(t+h) = y(t) + hf(t,y),$$

which is just a first order Taylor expansion.

Note that this scheme has a **local error** which is *second order* in $h$, since the next term in the Taylor expansion that has been dropped is $\propto h^2$. However, in integrating from $t=a$ to $t=b$, the number of steps that we have to take grows as $h$ decreases, so that the **global error** is one power of $h$ larger. For this reason, the Euler method is a **first order** method: for integration over a fixed time interval, doubling the number of points in the integration decreases the error by a factor of 2.
 
### Midpoint method

In the midpoint method, instead of using the derivative at $t$ to step across the interval, we use the derivative from half-way across, at $t+h/2$. This gives a **second order** method. The procedure is

1. Evaluate

$$y_1 = y(t) + {h\over 2}f(t,y).$$

2. Calculate the derivative at the half-way point

$$f_1 = f(t+h/2, y_1).$$

3. Use that to step across the entire interval

$$y(t+h) = y(t) + h f_1.$$


### Runge-Kutta

The midpoint method is an example of a broader class of methods known as **Runge Kutta** methods. The idea is to use combinations of derivatives evaluated across the interval $h$ to cancel out the higher order terms. The most used of these is the **4th order Runge Kutta** method (RK4). Together with a routine to take adaptive steps (i.e. vary the stepsize $h$ to achieve a desired accuracy), this might be the only method you will need for integrating ODEs. It is common enough that you should certainly know about RK4 and how it works.

The procedure is similar to the midpoint method, but with an extra step:

1. Step to the halfway point and evaluate the derivative there:

$$f_0 = f(t,y)$$
$$y_1 = y(t) + {h\over 2}f_0$$
$$f_1 = f(t+h/2, y_1)$$

2. Now repeat, but this time use $f_1$ to step to the halfway point and re-evaluate the derivative:

$$y_2 = y(t) + {h\over 2}f_1$$
$$f_2 = f(t+h/2, y_2)$$

3. Now use $f_2$ to step across the entire interval and evaluate the derivative there:

$$y_3 = y(t) + h f_2$$
$$f_3 = f(t+h, y_3)$$

4. Finally, the value of $y$ at $t+h$ is given by

$$y(t+h) = y(t) + {h\over 6}\left(f_0 + 2f_1 + 2f_2 +f_3\right).$$

This particular combination of the different derivatives results in an overall global error $\sim\mathcal{O}(h^4)$.


::::{tip} Exercise: Orbit integrations
:class: admonition

The set of coupled ODEs

$${d^2 x\over dt^2} = -{x\over (x^2+y^2)^{3/2}}\label{eq:orbitx}$$
$${d^2 y\over dt^2} = -{y\over (x^2+y^2)^{3/2}}\label{eq:orbity}$$

are the equations that describe a circular orbit[^with_units] with period $2\pi$, i.e. $x(t)$ and $y(t)$ are a circle with unit radius and the motion around the circle takes a time $2\pi$.

[^with_units]: To see how these equations would arise in an orbital dynamics problem, Newton's law of gravity gives the motion of a test mass around a massive body with mass $M$ at the origin as $${d^2\vec{r}\over dt^2} = - {\vec{r}\over r}{GM\over r^2},$$ ie. the acceleration is $GM/r^2$ always directed towards the origin. Equations {eq}`eq:orbitx` and {eq}`eq:orbity` are the components of this equation with units normalized so that $GM=1$, so that an orbit with orbital radius $1$ has period $2\pi$.

We can solve these equations by first writing them as 1st order ODEs:

$${dx\over dt} = u$$
$${dy\over dt} = v$$
$${du\over dt} = -{x\over (x^2+y^2)^{3/2}}$$
$${dv\over dt} = -{y\over (x^2+y^2)^{3/2}}$$

1. Write a code to integrate these equations around one orbit. For example, you can take initial conditions $(x,y,u,v)=(1,0,0,1)$. After integrating for a time $2\pi$, $x$ and $y$ should have returned to their starting values. The value of $y(t=2\pi)$ is then a measure of the error in the method (since it should have returned to zero at the end, so any non-zero value is due to the error in the integration).

2. Do the integration using the three different methods above. Make a plot of error vs number of steps $N$ and check the scalings with $N$ are as you expect.

Hint: try to write your integrator in as general a way as possible and take advantage of the vector expressions in numpy. For example, here is a function that does an Euler integration, given the parameters `nsteps` (number of integration steps to take), `dt` (step-size), `x0` (vector of initial values of the 4 variables), and `derivs` (the name of a function that calculates a vector of derivatives).
```python
    def integrate_euler(nsteps, dt, x0, derivs):
       x = np.zeros((nsteps, len(x0)))
       x[0] = x0
       for i in range(1,nsteps):    
          f = derivs((i-1)*dt, x[i-1])
          x[i] = x[i-1] + f*dt
       return x

    def derivs(t, x):
       xdot = ...
       ydot = ...
       udot = ...
       vdot = ...
       return np.array((xdot, ydot, udot, vdot))
      
```
Note that this is written allowing for a possible $t$ dependence of the derivative, although in this orbit problem, the derivatives do not depend explictly on time.

::::
