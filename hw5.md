# Homework 5

Due on Tuesday November 10 by 11.59pm.



## 1. Linear least-squares with SVD

(a) Show that in linear least-squares the vector of best-fitting parameters can be written as the linear expansion
$$\mathbf{a} = \sum_\alpha c_\alpha \mathbf{v}_\alpha \label{eq:hw5lsq}$$
with coefficients
$$c_\alpha = {\mathbf{u}^T_\alpha\mathbf{d}\over s_\alpha},$$
where $\{\mathbf{u}_\alpha\}$ and $\{\mathbf{v}_\alpha\}$ are the column vectors of the $\mathbf{U}$ and $\mathbf{V}$ matrices from SVD, and $\{s_\alpha\}$ are the singular values. (You can start with equation {eq}`eq:pseudoinv` from the Polynomial Fits: Part 2).

(b) Test this out by making observations of the function $f(x) = e^{-x} + e^{-1.1x}$ at 10 values of $x$ between 0 and 1. For each observation, add Gaussian noise with $\sigma=0.01$. Then use the sum in part (a) to fit the 10 observations with the model $m(x) = a_1 e^{-x} + a_2 e^{-1.1x}$. Repeat the cycle of generating observations and fitting the model 1000 times to build up a statistical sample of measured values of $a_1$ and $a_2$. Plot these points in the $a_1$-$a_2$ plane. Explain why the results look the way they do.

(c) Equation {eq}`eq:hw5lsq` gives the best-fitting parameters as a linear combination of the vectors $\mathbf{v}_\alpha$. A large singular value $s_\alpha$ means that the corresponding combination of basis functions $\mathbf{v}_\alpha$ can be well-constrained by the data. On the other hand, a small singular value means that the component $\mathbf{v}_\alpha$ is not well-constrained. For your fits in part (b), what do the $\mathbf{v}_\alpha$ vectors look like and what are the corresponding singular values? Use them to explain the $a_1$-$a_2$ plot you obtained in part (b).


## 2. Fitting planetary orbits

The file [rvs.txt](rvs.txt) contains a set of measurements of the radial velocity of a star that has an orbiting exoplanet. You can read in this data using 

``` python
tobs, vobs, eobs = np.loadtxt('rvs.txt', unpack=True)
```

which will give you the observation times `tobs` (in days), the velocities `vobs` (in m/s), and the error for each measurement `eobs` (in m/s).

(a) The orbital period of this planet is $P=1724$ days. Use Levenberg-Marquardt to find the remaining 5 parameters of the orbit $\mathbf{a} = (M_P, e, \omega_P, t_0, v_0)$. These are:

- the planet mass $M_P$ (in Jupiter masses)
- the eccentricity $e$ (from 0 for circular orbits to a maximum of 1)
- argument of periastron $\omega_P$ (in radians) (this determines the orientation of the ellipse relative to the plane of the sky)
- the time of pericenter $t_0$ (days)
- an overall velocity offset $v_0$ (m/s).

(b) Once you have the best-fitting parameters, plot the model curve on the same plot as the data to check the fit.


Hints:

- The following function returns the radial velocity for a set of times `t`, orbital period `P` and the remaining parameters in the vector `a`:

``` python
def rv(t, P, a):
    # Calculates the radial velocity of a star orbited by a planet
    # at the times in the vector t
    
    # extract the orbit parameters
    # P, t and tp in days, mp in Jupiter masses, v0 in m/s  
    mp, e, omega, tp, v0 = a
        
    # mean anomaly
    M = 2*np.pi * (t-tp) / P
    
    # velocity amplitude
    K = 204 * P**(-1/3) * mp  / np.sqrt(1.0-e**2) # m/s
    
    # solve Kepler's equation for the eccentric anomaly E - e * np.sin(E) = M
    # Iterative method from Heintz DW, "Double stars", Reidel, 1978
    # first guess
    E = M + e*np.sin(M)  + ((e**2)*np.sin(2*M)/2)
    while True:
        E0 = E 
        M0 = E0 - e*np.sin(E0)
        E = E0 + (M-M0)/(1.0 - e*np.cos(E0))
        if np.max(np.abs((E-E0))) < 1e-6:
            break
        
    # evaluate the velocities
    theta = 2.0 * np.arctan( np.sqrt((1+e)/(1-e)) * np.tan(E/2))
    vel = v0 + K * ( np.cos(theta + omega) + e * np.cos(omega))
    
    return vel
```

- You can use finite differences to calculate the design matrix $A_{ij}$. An example is given in the Lorentz fit [solutions](solutions-fitting-data#lorentzian-fit).

- You may find that taking a full Newton step causes problems, in particular trying to jump to $|e|>1$. In that case, you can artificially reduce the step, i.e. update using $\mathbf{a}_{n+1} = \mathbf{a}_n + \alpha \delta \mathbf{a}$ for some $\alpha<1$ (I used $\alpha =0.2$). (Don't worry if the eccentricity goes negative; this might happen depending on your starting guess for the parameters. The magnitude of $e$ should converge to the correct value; the minus sign happens because there is a degeneracy between the sign of $e$ and the two angles $\omega_P$ and $t_P$. The important thing is that the magnitude of $e$ should remain between 0 and 1).

## 3. Photon random walk

Photons in a scattering medium travel a distance $x$ between scatterings that has a probability density 
$$p(x)  = {1\over \ell} e^{-x/\ell},$$
where $\ell$ is the mean free path.

(a) *Without using any for loops*, write a code to follow the photon trajectory as it scatters $10^6$ times. Plot $r^2$ against $t$, where $r$ is the distance from the origin and $t$ is the time.

[Assume the scattering is isotropic, and for simplicity assume that the time between any two scatterings is always $\ell/c$ (ie. don't worry about accounting for the variations in scattering times depending on $x$). This means that you can just write the time as proportional to the number of scatterings so far.]

(b) Repeat part (a) 100 times (you can use a for loop for this if you want) and plot $\langle r^2\rangle$ against $t$, where $\langle r^2\rangle$ is the average value of $r^2$ across all trajectories at any given time. Compare with the prediction from the diffusion equation $\langle r^2\rangle = 6Dt$, where $D=c\ell/3$ is the diffusion coefficient.

(c) Repeat part (b) now using $10^4$ trajectories each with 1000 steps. Plot a histogram of the final $x$-coordinate from each trajectory and compare with the Green's function for the diffusion equation
$$p(x) = {1\over \sqrt{4\pi Dt}} e^{-x^2/4Dt}.$$

[In both parts (b) and (c) you should find good agreement with the diffusion equation predictions.]