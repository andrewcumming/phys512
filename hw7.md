# Homework 7

Due on Tuesday Dec 1 by 11.59pm.

## 1. The gravitational potential of a uniform density sphere

Use the DFT to solve Poisson's equation $\nabla^2 \phi = 4\pi G\rho$ for the gravitational potential of a uniform density sphere.
Compare your answer with the analytic solution:
$$\phi =
\begin{cases}
-{GM\over 2R}\left(3-{r^2\over R^2}\right), & r < R, \\
-{GM\over r},  & r \geq R,
\end{cases}
$$
where $M=(4\pi/3)\rho R^3$ is the mass of the sphere.

For simplicity in your calculation, you can take $\rho=1$, $R=1$, $G=1$. Use a 3D cartesian box and check to see how varying the size of the box changes your solution.

(a) Plot 2D maps of the density field and the gravitational potential in the equatorial plane of the sphere ($z=0$).

(b) Plot the gravitational potential and $x$-component of gravitational acceleration along the $x$-axis ($y=0$, $z=0$). Plot the analytic solution on top and compare. When computing the gravitational acceleration, you should take the gradient in spectral space.

(c) You should find that you get good agreement with the analytic solution except near the boundaries. By looking at the properties of your solution near the boundaries, explain the reason for the difference.

## 2. Scattering tails

Fast radio bursts (FRBs) are short bursts of radiation detected by radio telescopes. The shape of the pulse is affected by scattering of the radio waves as they propagate towards us. A model for the shape of an FRB is
$$h(t) = \int_{-\infty}^\infty f(t^\prime) g(t-t^\prime) dt^\prime,$$
where
$$f(t) = \exp\left(-{(t-t_0)^2\over 2\sigma^2}  \right)$$
is the intrinsic shape of the pulse produced by the astrophysical source (with a peak at time $t=t_0$), and
$$g(t) = \begin{cases}
{1\over \tau}e^{-t/\tau} & t\geq 0 \\
0  & t<0,
\end{cases}
$$
represents the delay introduced by scattering (scattered waves travel a longer path to get to us and so are delayed).

(a) Write a function that calculates the convolution of two discretely-sampled functions directly, using a sum to approximate the convolution integral. If both functions have $n$ grid points, show that the convolution has $2n-1$ values. Use your function to calculate the FRB shape $h(t)$ for different values of $\tau/\sigma$.

(b) Implement the convolution using Fourier transforms. Use zero-padding (call `numpy.fft.fft` with the parameter) so that your convolution has the correct number $2n-1$ of values. Compare your answer with part (a).

(c) Investigate what happens if you don't zero-pad the Fourier transforms in part (b).




