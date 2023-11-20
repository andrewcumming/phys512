# Discrete Fourier transform

The goal today is to become familiar with the discrete Fourier transform (DFT) routines in numpy ([numpy.fft](https://numpy.org/doc/stable/reference/routines.fft.html) - scroll to the bottom of the page to see the description of DFTs and how they are implemented) and scipy ([scipy.fft](https://docs.scipy.org/doc/scipy/reference/generated/scipy.fft.fft.html)).

To do this, let's continue the 2D diffusion example from last time, in which we [evolved a Gaussian temperature profile](https://andrewcumming.github.io/phys512/adi.html).
If you've solved the diffusion equation analytically before, you likely did it using Fourier methods. The Fourier transform of the temperature field can be written

$$\tilde{T}(k_x, k_y, t) = \int T(x,y,t) e^{-ik_x x}e^{-ik_y y}\ dx dy$$

with inverse

$$T(x,y,t)  = {1\over (2\pi)^2}\int \tilde{T}(k_x, k_y, t) e^{ik_x x}e^{ik_y y} \ dk_x dk_y.$$

Substituting this into the diffusion equation

$${\partial T\over \partial t} = {\partial^2 T\over \partial x^2} + {\partial^2 T\over \partial y^2}$$

gives the time-dependence of each mode

$${\partial \tilde{T}\over \partial t} = -(k_x^2 + k_y^2)\tilde{T}= -k^2\tilde{T}$$

or

$$\tilde{T}(k_x, k_y, t) \propto e^{-k^2 t}.$$(fourierevolve)

In a diffusion problem, each Fourier mode $(k_x, k_y)$ decays with time, with short-wavelength modes decaying faster than long-wavelength ones. So the solution involves

- decomposing the initial condition at $t=0$ into Fourier modes
- evolving each Fourier mode in time according to {eq}`fourierevolve`
- reconstruct the solution at time $t$ with an inverse transform back to real space.

With the DFT tools available in numpy we can apply the same approach to find a numerical solution to the diffusion problem.


```{admonition} Exercise: 2D diffusion with Fourier methods

Use DFTs to solve the 2D diffusion of a Gaussian temperature profile, i.e. find $T(x,y)$ as a function of time. Follow the algorithm above:

- take the 2D Fourier transform of your initial condition (temperature defined on a grid in $x$ and $y$), which you can take to be the Green's function

$$T(x, y, t) = {1\over 4\pi t} \exp\left(-{x^2+y^2\over 4t}\right)$$  

evaluated at the initial time.
- at any given future time $t$ apply the appropriate decay factor to each mode
- do the inverse transform to get back to the temperature field in real space

You can also set your initial condition in Fourier space at $t=0$ because $T(x,t)$ is then a delta-function which has a simple representation in Fourier space.

Check your answer by comparing with the analytic solution. 

The idea of this exercise is to become familiar with the DFT. Here are some questions you could ask - does the Fourier transform look as you expect (you can plot a color map of $\tilde{T}(k_x, k_y)$)? What are the values of $k_x$ and $k_y$ that are used to evaluate the Fourier transform? If you take the transform and then inverse transform, do you get back to where you started?

Useful functions:

- [numpy.fft.fft2](https://numpy.org/doc/stable/reference/generated/numpy.fft.fft2.html) - compute the 2D Fourier transform
- [numpy.fft.fftshift](https://numpy.org/doc/stable/reference/generated/numpy.fft.fftshift.html) - shifts the zero-frequency mode into the center, which is useful for plotting
- [numpy.fft.fftfreq](https://numpy.org/doc/stable/reference/generated/numpy.fft.fftfreq.html) - an easy way to get a list of the $k_x$ or $k_y$ values
- [numpy.fft.ifft2](https://numpy.org/doc/stable/reference/generated/numpy.fft.ifft2.html) - compute the inverse transform

```

```{admonition} Exercise: 1D DFTs

Here are some things you can do to become more familiar with DFTs:

1. Make a grid in $x$ with $n$ points and a spacing $\Delta x=1$ that goes from $x=0$ to $x=n-1$. Plot the DFT of $f(x)=\sin(kx)$ and $f(x)=\cos(kx)$ where $k=2\pi (m/n)$ for some integer $m$. 
- What happens as you change $m$?
- What differences do you see between $\cos$ and $\sin$?
- Compare the results for $m$ and $m+n$.
- Try non-integer $m$.
- Check that $F(k)=F^\star(-k)$ for real $f(x)$.
- Confirm that [Parseval's identity](https://en.wikipedia.org/wiki/Parseval's_identity) holds for the DFT.

2. Manipulating $f(x)$ in Fourier space:

- Take the complex cojugate of $F(k)$ and transform back to real space. What does that do to $f(x)$? (You need an $f(x)$ that is not symmetric about the center of your grid, ie. has an odd component).

- Multiply $F(k)$ by $e^{-ik y}$ for some constant $y$ and transform back to real space. What does that do to $f(x)$? (You can use `j = complex(0,1)` for the square root of minus 1.)

3. Write a function to calculate the convolution of two functions $f(x)$ and $g(x)$ using the fact that the Fourier transform of the convolution is equal to the product of the Fourier transforms of the two functions ([convolution theorem](https://en.wikipedia.org/wiki/Convolution_theorem)). Try it with a Gaussian and a top hat for example.

4. Set $f(x)$ equal to $n$ samples from a Gaussian distribution.

- Plot the power spectrum $|F(k)|^2$ as a function of (positive) $k$. Does it look like white noise? 
- Next, divide $F(k)$ by $ik$ and again plot the power spectrum on a log-log plot. Take the inverse transform to get the modified function $g(x)$ and plot $g(x)$ against $x$.
- Take the first half of the $g(x)$ values as $x$ and the second half as $y$ and plot the $(x,y)$ points. What do you see?

```

## Further reading

- A couple of interesting papers on red noise: [Flicker noises in astronomy and elsewhere](https://ui.adsabs.harvard.edu/abs/1978ComAp...7..103P/abstract), [1/f laws found in non-human music](https://ui.adsabs.harvard.edu/abs/2023NatSR..13.1324J/abstract)
