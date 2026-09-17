# Homework 2

Due on Tuesday September 29 by 11:59pm.

## 1. Midpoint method

In class, we discussed the solution of the initial value problem
$${dy\over dt} = f(t, y).$$

(a) Consider the situation that, at time $t_n$, we know the value of $y(t_n)$, and we want to step forward to time $t_n+h$. Show that

$$y(t_n+h) \approx y(t_n) + hf_n + {h^2\over 2}\left(\left.{\partial f\over\partial t}\right|_n + \left.{\partial f\over\partial y}\right|_nf_n\right) +\mathcal{O}(h^3),$$
where we write $f(t_n, y(t_n))$ as $f_n$ and the partial derivatives are evaluated at $t=t_n$ and $y=y(t_n)$.

(b) Use your result from part (a) to show that the explicit midpoint method has a local truncation error of $\mathcal{O}(h^3)$. Hint: do a 2-dimensional Taylor expansion of
$$f\left(t+{h\over 2}, y(t_n) + {h\over 2}f_n\right)$$
about the point $(t_n, y(t_n))$.

(c) Explain why the midpoint method is described as a second order integrator even though the local truncation error is $\mathcal{O}(h^3)$. 


## 2. Radioactive decay

Implement the radioactive decay sequence[^hw2footnote]

$$^{224}\mathrm{Ra} \xrightarrow{3.6\ \mathrm{days}} \ ^{220}\mathrm{Rn}\xrightarrow{55\ \mathrm{s}} \ ^{216}\mathrm{Po}  \xrightarrow{0.14\ \mathrm{s}}\  ^{212}\mathrm{Pb}\xrightarrow{10.6\ \mathrm{h}}\ ^{208}\mathrm{Pb} $$

using the backward Euler implicit scheme.

[^hw2footnote]: Note that we've simplified the last step of the sequence for this question, in reality the last step goes through a couple of different branches involving intermediate elements.

Start with pure $^{224}\mathrm{Ra}$ and follow the evolution for 10 days. [Note that the times shown are the half-lives of the different decays, the decay rate is $\ln 2/t_{1/2}$.]

How do the abundances of the other elements evolve?
Investigate the effect of changing your timestep.

Use the ratios of the different half-lives to test your numerical results.


## 3. Second order implicit integration

There are a couple of options for implicit update schemes that are second order. One is the implicit midpoint method:

$$\mathbf{y}_{n+1} = \mathbf{y}_n + h \mathbf{f}\left({\mathbf{y}_n + \mathbf{y}_{n+1}\over 2}\right)$$

and another is the Crank-Nicolson method:

$$\mathbf{y}_{n+1} = \mathbf{y}_n + {h\over 2} \left[ \mathbf{f}\left(\mathbf{y}_n\right) + \mathbf{f}\left(\mathbf{y}_{n+1}\right)\right].$$

Use both of these to integrate the non-linear pendulum, and check that they both give 2nd order integrators. How do your results compare with the backward-Euler results from class?
