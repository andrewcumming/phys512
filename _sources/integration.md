# Integration

## Newton-Cotes methods

A related problem to interpolation is when we need to calculate the integral of a function 

$$I = \int_{x_1}^{x_N} f(x) dx$$

given values of the function at a discrete set of points $x_1\dots x_N$, which we'll write as $f_i\equiv f(x_i)$. For simplicity here we'll assume that the spacing between points on the grid is constant $x_{i+1}-x_i = \Delta x$, but it's straighforward to generalize to non-uniform sampling if you need to.

To estimate the value of the integral, we need to model how the function behaves in each interval $x_i<x<x_{i+1}$. As with interpolation, we can take $f(x)$ in each interval to be a polynomial, including additional terms in the polynomial to increase the accuracy of our approximation. 

**Rectangle rule**

The first approximation is to assume that $f(x)$ is a constant $f(x)=f_i$ in the interval $x_i\leq x\leq x_{i+1}$. The total integral is then

$$I\approx \sum_{i=1}^{N-1} f_i \Delta x.$$


**Trapezoidal rule**

If we make a linear approximation, we can write 

$$f(x) \approx f_i + {f_{i+1}-f_i\over \Delta x} \left(x-x_i\right)\hspace{1cm} x_i\leq x\leq x_{i+1}.$$

Then 

$$\int_{x_i}^{x_{i+1}} f(x) dx =  \int_0^{\Delta x} f(x_i+y) dy \approx f_i \Delta x + (f_{i+1}-f_i){\Delta x\over 2}  = {f_i+f_{i+1}\over 2}\Delta x.$$

This is known as the *trapezoidal rule* because it corresponds to the area of the trapezoid formed by connecting the points $(x_i,f_i)$ and $(x_{i+1}, f_{i+1})$ by a straight line.

When we sum over all intervals, each point gets counted twice except for the left and right boundaries, so

$$I \approx \Delta x \left[{f_1\over 2} + f_2 + f_3 \dots + f_{N-1} + {f_N\over 2}\right].$$


**Simpson's rule**

For the next order, we consider the double interval $x_{i-1}<x<x_{i+1}$, and write
 
$$f(x) \approx f_i + (x-x_i) \left.{df\over dx}\right|_{x_i}+ {(x-x_i)^2\over 2}\left.{d^2f\over dx^2}\right|_{x_i}.$$

The reason for considering the double interval from $x_{i-1}$ to $x_{i+1}$ is that the linear term is odd in this interval (antisymmetric about $x_i$) and so does not contribute to the integral, i.e.

$$\int_{x_{i-1}}^{x_{i+1}} f(x) dx \approx 2 f_i \Delta x + {1\over 2} \left.{d^2f\over dx^2}\right|_{x_i} {2(\Delta x)^3\over 3}.$$

Taking 

$$\left.{d^2f\over dx^2}\right|_{x_i} \approx {f_{i+1}-2f_i + f_{i-1}\over (\Delta x)^2}$$

and simplifying then gives

$$\int_{x_{i-1}}^{x_{i+1}} f(x) dx \approx {\Delta x\over 3} \left[f_{i-1} + 4f_i + f_{i+1} \right].$$

Adding up the contributions from each double interval, the total integral is

$$I \approx {\Delta x\over 3}\left[ f_1 + 4f_2 + 2f_3 + 4f_4 + 2f_5 \dots 4f_{N-1} + f_N \right],$$

where we need the total number of points $N$ to be an odd number (so we can divide the domain into a set of double intervals).


```{admonition} Exercise: Newton-Cotes
Implement these three methods to integrate a function $f(x)$ of your choice. How does the error in each method scale with the number of points $N$? What kind of functions are integrated exactly (to machine precision) for each method?
```
