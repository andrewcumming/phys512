# Homework 3

Due on Friday Oct 13 by midnight.

In this homework, we will look at some applications of Monte Carlo methods.

1. **Diffusion-limited aggregation**

In this question we want to model the growth of a crystal starting with a small solid "seed" that grows as particles from the liquid randomly encounter the seed and stick to it. Once the particle sticks, it becomes part of the solid and can in turn cause liquid particles to stick to it. In this way, the liquid particles freeze onto the growing solid. Because the rate of growth is set by how quickly the liquid particles diffuse and encounter the growing crystal, this process is known as diffusion-limited aggregation.

(a) First model the liquid particles -- consider a collection of $N=10^4$ particles undergoing Brownian motion, i.e. random walks. The particles do not interact, so we don't have to worry about collisions between particles, they move independently from one another. 

A simple way to do this is to assume that the particles can only occupy discrete locations ($i$, $j$) on a square $n\times n$ grid (a good value to choose is $n=200$). Start with a random choice of position for each particle and update the positions at each timestep by assuming the particle can either stay where it is or jump to one of the neighbouring 8 squares. (Note that you can do this efficiently by storing the locations of all the particles in a vector and updating all the locations at once in a vector operation. Also, you can assume periodic boundary conditions, so that if a particle moves off the right hand side of the grid it reappears on the left and so on.) 

Implement this and plot the trajectories of your particles. Investigate how the distance moved from the starting location depends on time. Is it what you expect for a random walk?

(b) Now create a separate $n\times n$ array that will keep track of which locations are occupied by solid (once particles become solid they stop moving so you just need to keep track of where they are). For the initial condition, put a small collection of solid particles in the center of your grid (the "seed"). Then evolve the liquid particles in time as in part (a). If a liquid particle moves next to a solid particle, then it becomes part of the solid (and is removed from the collection of liquid particles). Evolve your simulation in time until 80% of your particles have solidified. 
Plot the 2D distribution of liquid and solid particles at different times during the simulation (even better, make a movie showing what happens over time). 

(c) Plot the number of solid particles against time. How does $N_\mathrm{solid}$ change with time? Can you understand this using your results from part (a) on how the liquid particles move? 

(It may also help to plot the number density of liquid particles as a function of radial distance from the centre of the grid and look at how it evolves with time).


2. **Ising model** 

The Ising model is a simple model of a ferromagnetic material in which a lattice of spins interact via nearest-neighbour interactions. In 2D, we have a square lattice with $n\times n = N$ lattice sites on which the spins sit. Each spin can be either up ($s=+1$) or down ($s=-1$), and the total energy of the system is

$$E = -\sum_\mathrm{nn} s_i s_j,$$

where "nn" in the sum indicates that the sum is over nearest-neighbour pairs (with each pair counted once). Any given spin interacts only with the spins that are immediately up, down, left and right from its location. We can see that the energy will be minimized when neighbouring spins are aligned. 

For any given configuration of spins, the total magnetization is

$$M = \sum_i s_i.$$

At a temperature $T$, the probability that the system is in a configuration with energy $E$ is given by the Boltzmann distribution

$$\mathrm{Prob}(E) \propto \exp\left(-E/k_b T\right).\hspace{1cm} (*)$$

To calculate the magnetization at a given temperature, we need to sample the possible configurations from this probability distribution and then we can average over them to find the mean magnetization $\langle M\rangle$ as a function of temperature. Since the energy $E$ depends on the values of the $N$ spins $\{s_i\}$, we are dealing with an $N$ dimensional parameter space (with $2^N$ possible sample points), and so this an example where Monte Carlo methods are needed to sample from the probability distribution.

(a) Use the Metropolis-Hastings algorithm to generate a sample of configurations $\{s_i\}$ from the probability distribution (*). Rather than storing the sequence of configurations, instead calculate $M$ at each step and store the chain of $M$ values. 

Some hints:

- for the proposal step of the Metropolis algorithm, you can choose a spin at random and consider flipping it from up to down or vice versa.
- when you assess whether or not to accept the proposed change, you can use the change in energy due to the spin flip, which is 

$$\Delta E = 2 s_i \left( s_\mathrm{up} + s_\mathrm{down} + s_\mathrm{left} +s_\mathrm{right} \right)$$

(where the spins here are the current values before the spin flip).

- you can use periodic boundary conditions for the lattice, and you might find it simpler to store the spins in a 1D array rather than a 2D array. 

(b) Make plots of your chains of $M$ (i.e. plot $M$ against iteration number) for different temperatures in the range $k_BT = 1$--$4$. Do you see a "burn-in" phase? You should see a dramatic difference in behavior between low and high temperatures - a phase transition. How do your sequences change as $T$ changes?

(c) Plot the mean magnetization $\langle |M|\rangle$ and the variance $\mathrm{Var}(M)$ as a function of temperature. A characteristic of second order phase transitions is large fluctuations during the phase transition. Do you see this behaviour?











