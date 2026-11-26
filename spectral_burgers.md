# A nonlinear example

As a final example of spectral methods, we'll look at the non-linear PDE 

$${\partial f\over \partial t} + f{\partial f\over \partial x} = D {\partial^2 f\over \partial x^2}\label{eq:burgers}$$

which is known as the **viscous Burgers' equation**. It comes up in fluid dynamics, where $f=u$ the fluid velocity, where it describes nonlinear sound waves and the formation of shocks.

The difference from the previous examples is that there is a nonlinear term, which introduces a coupling between modes. Using the inverse Fourier transform
$$f(x) = {1\over 2\pi} \int dk\ g(k) e^{ikx}$$
(where the normalization matches the `numpy.fft` convention), the non-linear term is
$$f{\partial f\over \partial x} = {1\over (2\pi)^2} \int dk^\prime dk^{\prime\prime} g(k^\prime) g(k^{\prime\prime}) ik^{\prime\prime} e^{i(k^\prime+k^{\prime\prime})x}$$
Now multiply this by 
$$\int dk\ e^{-ikx}$$
and use
$$\delta(k_0-k) = {1\over 2\pi} \int dk\ e^{i(k_0-k)},$$
which will extract the $k$-component:
$$\int f{\partial f\over \partial x} e^{-ikx} dx = 
{1\over 2\pi} \int dk^\prime dk^{\prime\prime} g(k^\prime) g(k^{\prime\prime}) ik^{\prime\prime} \delta(k^\prime + k^{\prime\prime} - k)$$
$$ = {1\over 2\pi} \int dk^\prime g(k^\prime) g(k-k^\prime) i (k-k^\prime).$$

We can then take the Fourier transform of equation {eq}`eq:burgers` to get 
$${dg(k)\over dt} = - Dk^2 g(k) - {1\over 2\pi} \int dk^\prime g(k^\prime) g(k-k^\prime) i (k-k^\prime).\label{eq:burgerdgdt}$$
Note that the last term is a convolution between the functions $g(k)$ and $ikg(k)$.


:::{tip} Exercise

Implement equation {eq}`eq:burgerdgdt` and evolve $f(x,0) = \sin(4\pi x)$ on a grid from $x=0$ to $x=1$ (don't forget to do `endpoint=False`). For the timestepping you can just use forward Euler. For the convolution, you can use

```(python)
def convolution(f,g):
    ft = np.fft.fft(f)
    gt = np.fft.fft(g)
    return np.fft.ifft(ft*gt)
```
The last term in equation {eq}`eq:burgerdgdt` can be evaluated as `-convolution(g, 1j*k*g) / n` where the normalization $1/2\pi$ for the continuous Fourier transform has been replaced by $1/n$ for the discrete Fourier transform.

Evolve for a time $t=0.1$ and see what happens. You can try $D=10^{-6}$ to effectively turn off diffusion or $D=10^{-3}$ to include diffusion. What difference does it make?

What happens if you integrate longer than $t=0.1$ with and without diffusion?

You can also try adding a constant to the sinusoid profile, eg. $f(x,0) = 1 + A\sin(4\pi x)$ where $A$ is the wave amplitude.

:::
