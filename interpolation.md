# Interpolation

## Linear interpolation

Often we know the values of a function at a fixed set of points $f(x_i)$ and need to interpolate to find the value at a point $x$ where $x_i<x<x_{i+1}$. One way to do this is a *linear interpolation*

$$f(x) \approx f(x_i) + (x-x_i) {f(x_{i+1})-f(x_i)\over x_{i+1}-x_i}\hspace{1cm}x_i<x<x_{i+1},$$

or in a more symmetric form this can be written

$$f(x) \approx {x_{i+1}-x\over x_{i+1}-x_i} f(x_i) + {x - x_i \over x_{i+1}-x_i} f(x_{i+1})\hspace{1cm}x_i<x<x_{i+1}.$$

This is equivalent to drawing a straight line between the two points. 

```{admonition} Exercise: linear interpolation
Choose a function (e.g. $\sin(x)$ between $0$ and $2\pi$ or a Gaussian) and investigate how the error made in linear-interpolation depends on the number of points. You can use [`numpy.interp`](https://numpy.org/doc/stable/reference/generated/numpy.interp.html) to carry out the linear interpolation. Plot your function, the sampled points, and the interpolated function on the same plot. How quickly does the error decrease with the number of sampled points? How do you explain the scaling that you see?
```



## Cubic interpolation and splines

Linear interpolation is an example of a polynomial fit -- given two data points, we can fit a straight line between them. We could go to higher order by including more data points when modelling each interval. For example, for the interval $x_i<x<x_{i+1}$, we could use the four points $x_{i-1}, x_i, x_{i+1}$, and $x_{i+2}$ to fit a cubic polynomial to use in this range. 

Here is some code that uses 
[numpy.polynomial.polynomial.Polynomial](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.polynomial.Polynomial.html) to fit a cubic polynomial to each interval. (Note that the interpolated curve is calculated only between $x_2$ and $x_{N-1}$ because we don't have enough information to fit a cubic for $x_1<x<x_2$ or $x_{N-1}<x<x_N$).

```
# Sample the function
xp = np.linspace(0.0, 2*np.pi, num=10)
fp = np.sin(xp)

# Cubic interpolation
x = np.linspace(x1, x2, num=1000)
f2 = np.zeros_like(x)
# do a cubic polynomial fit to each interval (except the first and last)
for i in range(len(xp)-3):
    poly = np.polynomial.Polynomial.fit(xp[i:i+4], fp[i:i+4], 3)    
    ind = np.where(np.logical_and(x<xp[i+2],x>=xp[i+1]))
    f2[ind] = poly(x[ind])
# exclude the first and last intervals
ind = np.where(np.logical_and(x>=xp[1],x<xp[-2]))
x_cubic = x[ind]
f_cubic = f2[ind]
```

```{admonition} Exercise: cubic interpolation

Add this to your code from the previous exercise and compare with the results for linear interpolation. Do you see a different scaling with the number of points?

Use the [`deriv()`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.polynomial.Polynomial.deriv.html#numpy.polynomial.polynomial.Polynomial.deriv) method associated with the polynomial to plot the first and second derivatives of the interpolating function (as a function of $x$). Does the behavior of the derivatives make sense? What happens at the boundary between each interval $x_i<x<x_{i+1}$?
```

You should find in the previous exercise that the cubic polynomial gives a much more accurate interpolation compare to linear. **Cubic splines** (usually referred to just as "**splines**", although splines can be constructed for higher orders than cubic) extend this by 

- ensuring that the first and second derivatives are continuous at the boundaries between the different fitting intervals
- solving simultaneously for the coefficients of the cubic polynomials of all intervals at once (rather than separately for each interval as we did in the code above)
- deal with the boundaries by making an extra assumption about the second derivative at the boundaries. Often this is that the second derivatives vanish at the boundaries (known as *natural splines*).

When constructing a spline, we need to derive three coefficients for each interval (the coefficients of the $x$, $x^2$ and $x^3$ terms). By specifying three constraints at each grid point --- the value of the function, continuity of the first derivative, and continuity of the second derivative --- there is enough information to solve for the spline coefficients. 

In SciPy, the way splines work is that you first create the spline using [`scipy.interpolate.CubicSpline`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.CubicSpline.html), e.g.

`spline = scipy.interpolate.CubicSpline(xp,fp)`

where `xp` and `fp` are the sample points and function values at those points. This returns an interpolating function that you can use to interpolate, i.e. use `spline(x)` for a set of $x$-values `x`.


```{admonition} Exercise: splines
Add a spline fit to your code and compare with the linear and cubic polynomial fits. In particular, how does the spline fit differ from the cubic polynomial fit in terms of accuracy and the first and second derivatives?

Look at the documentation and try changing what happens at the boundaries by setting different `bc_type` values. Natural splines are not the default behavior for `scipy.interpolate.CubicSpline` -- what is it?
```

```{admonition} Exercise: higher order fits, oscillations, and noise
Once you have the linear, cubic, and spline fits working, try the following:

- With $n$ data points, we can fit an $n-1$ polynomial to the whole data set. This is known as the [Lagrange polynomial](https://en.wikipedia.org/wiki/Lagrange_polynomial), and you can obtain it easily using [`scipy.interpolate.lagrange`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.lagrange.html). Add this to your different fits and see how it behaves.

Next you can try two different changes to test how well the different methods perform: 

- Rather than a smooth function, try a function with a discontinuity in value (e.g. a step function) or with a discontinuity in the gradient. 

- Add some Gaussian noise to the data points (you can use [`np.random.normal`](https://numpy.org/doc/stable/reference/random/generated/numpy.random.normal.html) to do this).

You should see that the higher order fits, and even the spline/cubic fits, are susceptible to oscillations. Even a small amount of noise can lead to bad behavior of the Lagrange polynomial compared to the splines/linear fits.
```



## Bilinear interpolation

Bilinear interpolation is the extension of linear interpolation to 2D. In this case, we have a function of two parameters, $f(x,y)$ sampled on a 2D grid of points $(x_i, y_i)$, and we would like to evaluate the function at an arbitrary $(x,y)$ point. 

If the point ($x,y$) lies within in the square with corners at $(x_i, y_j)$, $(x_{i+1}, y_j)$, $(x_i, y_{j+1})$ and $(x_{i+1}, y_{j+1})$, then

$$f(x,y) \approx f(x_i, y_j) + a (x-x_i) + b (y-y_j) + c (x-x_i)(y-y_j),$$

where 

$$a = {f(x_{i+1}, y_j) - f(x_i, y_j) \over x_{i+1}-x_i}, \hspace{1cm} b={f(x_i, y_{j+1}) - f(x_i, y_j) \over y_{j+1}-y_j}$$

and

$$c = {  f(x_{i+1}, y_{j+1})-f(x_{i+1}, y_j) -f(x_i, y_{j+1}) + f(x_i, y_j) \over  (x_{i+1}-x_i)(y_{j+1}-y_j)}.$$

You will use this in the homework problem about a tabulated equation of state.

Here is an example of interpolation in 2D using [`scipy.interpolate.RegularGridInterpolator`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.RegularGridInterpolator.html):

```
import numpy as np
import scipy.interpolate
import matplotlib.pyplot as plt

def func(X,Y):
    return np.exp(X*Y)

# sample the function on a coarse grid and set up the interpolation
xp = np.linspace(2,3,10)
yp = np.linspace(2,3,10)
X, Y = np.meshgrid(xp, yp, indexing='ij')
fp = func(X,Y)
interp = scipy.interpolate.RegularGridInterpolator((xp,yp),fp, bounds_error=False, fill_value=None)

# now compute the function on a finer grid
xx = np.linspace(2,3,100)
yy = np.linspace(2,3,100)
X, Y = np.meshgrid(xx, yy, indexing='ij')
f = func(X,Y)

# plot the fractional error
plt.imshow((interp((X,Y), method='linear')-f)/f, origin='lower', extent=(2,3,2,3))
plt.colorbar()
plt.show()
```

Note that you can specify the interpolation method when you call the interpolating function. For example, changing `method` from `linear` to `cubic` will use a bivariate cubic spline instead of bilinear interpolation (try it!).






## Further reading

- To learn more about the details of how splines are constructed, you can look at section 6.3 of Gezerlis, or section 3.3 in Numerical Recipes.

- Interpolation with SciPy is covered in the [User Guide](https://docs.scipy.org/doc/scipy/tutorial/interpolate.html). There are interpolation routines for regular and irregular sampling and 1D or multi-D.





