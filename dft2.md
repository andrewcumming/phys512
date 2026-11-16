# Discrete Fourier transform part 2

Here are some things you can do to become more familiar with DFTs:

:::{tip} Exercise: 1D DFTs

Make a grid in $x$ with $n$ points and a spacing $\Delta x=1$ that goes from $x=0$ to $x=n-1$. Plot the DFT of $f(x)=\sin(kx)$ and $f(x)=\cos(kx)$ where $k=2\pi (m/n)$ for some integer $m$. 
- What happens as you change $m$?
- What differences do you see between $\cos$ and $\sin$?
- Compare the results for $m$ and $m+n$.
- Try non-integer $m$.
- Check that $F(k)=F^\star(-k)$ for real $f(x)$.
- Confirm that [Parseval's identity](https://en.wikipedia.org/wiki/Parseval's_identity) holds for the DFT.
:::

:::{tip} Exercise: Manipulating $f(x)$ in Fourier space

For a function $f(x)$

- Take the complex conjugate of $F(k)$ and transform back to real space. What does that do to $f(x)$? (You need an $f(x)$ that is not symmetric about the center of your grid, ie. has an odd component).

- Multiply $F(k)$ by $e^{-ik y}$ for some constant $y$ and transform back to real space. What does that do to $f(x)$? (You can use `j = complex(0,1)` for the square root of minus 1.)
:::

:::{tip} Exercise: Convolution with DFT

Write a function to calculate the convolution of two functions $f(x)$ and $g(x)$ using the fact that the Fourier transform of the convolution is equal to the product of the Fourier transforms of the two functions ([convolution theorem](https://en.wikipedia.org/wiki/Convolution_theorem)). Try it with a Gaussian and a top hat for example. You can compare your result with [`scipy.signal.convolve`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.convolve.html).
:::

:::{tip} Exercise: Red noise generator

Set $f(x)$ equal to $n$ samples from a Gaussian distribution.

- Plot the power spectrum $|F(k)|^2$ as a function of (positive) $k$. Does it look like white noise? 
- Next, divide $F(k)$ by $ik$ and again plot the power spectrum on a log-log plot. Take the inverse transform to get the modified function $g(x)$ and plot $g(x)$ against $x$.
- Take the first half of the $g(x)$ values as $x$ and the second half as $y$ and plot the $(x,y)$ points. What do you see?
:::

## Further reading

- A couple of interesting papers on red noise: [Flicker noises in astronomy and elsewhere](https://ui.adsabs.harvard.edu/abs/1978ComAp...7..103P/abstract), [1/f laws found in non-human music](https://ui.adsabs.harvard.edu/abs/2023NatSR..13.1324J/abstract)
