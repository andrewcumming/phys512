# Initial value problems

In an initial value problem, we want to solve a set of ordinary differential equations 

$${dy_i\over dx} = f(x,\{y_i\})$$

for the functions $y_i$ given a set of boundary conditions at the starting point $x=x_0$. We then use the information we have about the derivatives $f(x,\{y_i\})$ to step away from $x_0$ and evaluate $y_i$ at a location $x=x_0+h$, where $h$ is the step size (we could also write this as $\Delta x$, but we'll use $h$ here in common with many treatments of ODEs). Note that the derivative $f$ can depend on the position $x$ but also any of the $y_i$'s, ie. we could have a set of *coupled ODEs*. 

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





## Implicit methods

### Backward Euler

### Newton's method for finding roots

## Integrating ODEs using Scipy



