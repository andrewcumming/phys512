# Diffusion with Chebyshev

We saw last time that for periodic boundaries, a Fourier expansion is a good choice. What if the boundary conditions are not periodic? In that case, we can choose a different set of basis functions. For example, consider once again the diffusion equation in 1D,
$${\partial f\over\partial t} = {\partial^2f\over \partial x^2},\label{eq:chebydiffuse}$$
with boundary conditions $f=0$ at $x=-1$ and $x=1$.

This time let's expand in Chebyshev polynomials, which we met earlier in the course when we were doing polynomial fits to data. We write 
$$f(x,t) = \sum_i a_i(t) T_i(x)$$
where $T_i(x)$ is the $i$th Chebyshev polynomial and $a_i(t)$ are the coefficients that evolve with time.

You can see a list of Chebyshev polynomials $T_i(x)$ written out on [Wikipedia](https://en.wikipedia.org/wiki/Chebyshev_polynomials#First_kind). The Chebyshev polynomial $T_i(x)$ is an $i$th degree polynomial in $x$. If you differentiate a Chebyshev polynomial, it produces a polynomial in $x$ of degree $i-1$, which itself can be written as a sum of Chebyshev polynomials. This means that we can write the right-hand side of equation {eq}`eq:chebydiffuse` as 
$${\partial^2 f\over \partial x^2} = \sum_i a_i {d^2T_i\over dx^2} = \sum_i b_i T_i(x),$$
so the same kind of expansion but with a different set of coefficients $\{b_i\}$. We'll see later that we'll be able to use a numpy routine to calculate the values of $b_i$ for us.

The diffusion equation is therefore
$$\sum_i {da_i\over dt} T_i(x) = \sum_i b_i T_i(x),$$
or, since the Chebyshev polynomials are orthogonal,
$${da_i\over dt} = b_i.$$
This then gives us a way to integrate in time. Each timestep, we calculate the coefficients $\{b_i\}$ that define $d^2f/dx^2$ based on the current set of coefficients $\{a_i\}$ that define $f(x)$. Then we can take a timestep, for example just doing forward Euler: $\Delta a_i = b_i\Delta t$ for each coefficient $i$.

There is just one subtlety here: because we differentiate twice, there are 2 fewer $b_i$ values compared to $a_i$. We set the last two values of $a_i$ using the boundary conditions, which are
$$f(1) = \sum_i a_i T_i(1) = 0\Rightarrow \sum_i a_i =0$$
$$f(-1) = \sum_i a_i T_i(-1) = 0 \Rightarrow \sum_i (-1)^i a_i=0.$$
Here we used the properties $T_i(1) = 1$ and $T_i(-1)=(-1)^i$. Adding and subtracting these shows that the even and odd coefficients should separately sum to zero:
$$\sum_{i\ \mathrm{even}} a_i = 0; \hspace{1cm}\sum_{i\ \mathrm{odd}} a_i = 0.$$
We can enforce these after each time step by adjusting the last odd and even coefficients.

:::{tip} Exercise: Diffusion with Chebyshev

Implement this. A good starting $f(x)$ is the Green's function for the diffusion equation because you then have an analytic prediction for the future time evolution.

Some hints:
- you can fit a Chebyshev series to your initial $f(x)$ using [`numpy.polynomial.chebyshev.chebfit`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.chebyshev.chebfit.html) (we used this previously in the section on fitting polynomials to data)
- once you have the initial coefficients $a_i$, you can evolve them in time with a first order explicit Euler update
- to calculate $da_i/dt$ you can use [`np.polynomial.chebyshev.chebder`](https://numpy.org/doc/stable/reference/generated/numpy.polynomial.chebyshev.chebder.html) to obtain the coefficients of a Chebyshev series representing the second derivative $\partial^2 f/\partial x^2$. 
- to enforce the boundary conditions initially and after each timestep, you can use the last two $a_i$ values to set the sum of the even $a_i$ coefficients and the odd $a_i$ coefficients to zero.
- the Green's function in this case where we have zero boundary conditions can be constructed by adding mirror images outside the domain on the left and right: (same ideas as the method of images in electrostatics!)

```
def gaussian(x, x0, t, D):
    # Green's function for the diffusion equation
    return np.exp(-(x-x0)**2/(4*D*t)) / np.sqrt(4*np.pi*D*t)

def greens(x, x0, t, D):
    # Green's function for the diffusion equation with zero boundary conditions at x=-1,+1
    f = gaussian(x, x0, t, D)
    f -= gaussian(x, -1-(1+x0), t, D)
    f -= gaussian(x, 1+(1-x0), t, D)
    return f
```

:::


