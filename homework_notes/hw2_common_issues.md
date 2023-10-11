# Homework 2: Common Issues

```{note}
This section was written by Valentin Boettcher, the TA who graded homework 2.
```

First of all: You all did a great job. Almost all codes I received were ran without modification (except for the missing `hw2_data.txt`, see below). My feedback on myCourses may sound overly negative. This is because I mostly documented the reasons for deducting points due to time constraints. Furthermore, some suggestions below may seem a little pedantic. This is certainly not a software-engineering course. See my suggestions more as general tips and not as strict requirements.

# Probem 1 a)
The formulation "Explain why" may have been slightly ambiguous. My interpretation of that was: "show that". There are two sides to the issue. I will use the Simpson rule as an Illustration. First, we subdivide the integration interval into pieces of length $Δx$ and Taylor-expand the integrand in each sub-interval (assuming that the radius of convergence of the expansion is smaller than $Δx$):

$$f(x) = f(x_i) + f'(x_i) (x-x_i) + f''(x_i) \frac{(x-x_i)^2}{2} + D\cdot (x-x_i)^3+ \mathcal{O}(|x-x_i|^4)\hspace{1cm} x_i\leq x\leq x_{i+1}.$$ (taylor)

Why did I include the third order term in the above? In principle, we could have absorbed that into the error term (adjusting its order accordingly). However, our analysis needs the precise form of the error term to this order as, we will see later.

Now, {eq}`taylor` is not quite what we want yet. As we integrate over a double interval (see {doc}`../integration`) the linear term vanishes (analytically!), but we still need the second derivative at $x_i$. Therefore we *approximate* the second derivative as

$${d^2f\over dx^2}(x_i) = {f(x_i+\Delta x) -2 f(x_i) + f(x_i-\Delta x)\over (\Delta x)^2} + \mathcal{O}\left(|\Delta x|^2\right).$$ (num-deriv)

```{admonition} Landau's Trashcan
Here, I employ the [Big O Notation](https://en.wikipedia.org/wiki/Big_O_notation).
We say $f(x) = g(x) + \mathcal{O}(h(x))$ for $x\to a$ (omitted if $a=0$) iff there exists a $M>0$ for a certain range around $x_i$ such that

$$ \lim_{x\to a} \frac{|f(x)-g(x)|}{h(x)} = M<∞.$$

For a truncated Taylor series ($h(x)=|x-x_0|^n$) this fact is quite obvious, as the Taylor expansion continues with terms $\sim (x-x_i)^n$.

When we quantify the asymptotic error for $x\to a$ we can replace the error term with $M h(x)$. This discussion of the Big O may seem a little overkill, but I thought it might be useful, as it comes up quite often. In my analysis course, it was actually used to define the derivative.

The nice thing about "Landau's Trashcan" is that we can use an $=$ sign rather than some ill defined $\approx$. We can conventiently manipulate the Big O (see the Wikipedia link above), without worrying about details.
```


Putting {eq}`taylor` and {eq}`num-deriv` together, we get (note the equals sign)

$$f(x) = f(x_i) + f'(x_i) (x-x_i) + \left[{f(x_i+\Delta x) -2 f(x_i) + f(x_i-\Delta x)\over (\Delta x)^2} + \mathcal{O}\left(|\Delta x|^2\right) \right] \frac{(x-x_i)^2}{2} + D \cdot(x_i - x)^3 + \mathcal{O}(|x-x_i|^4)\hspace{1cm} x_i\leq x\leq x_{i+1}.$$

We are only interested in the error terms, so let's throw the rest away

$$f(x)-f_{\mathrm{approx}}(x) = \mathcal{O}\left(|\Delta x|^2\right)  \frac{(x-x_i)^2}{2} + D\cdot (x_i - x)^3 + \mathcal{O}(|x-x_i|^4).$$

As we will integrate our approximated function over the interval $[x_{i-1},x_{i+1}]$, the difference between the exact integral and our approximation will work out to

$$Δ I_n = \int_{x_{i-1}}^{x_{i+1}}  (f(x)-f_{\mathrm{approx}}(x)) \mathrm{d}x$$

$$=\int_{x_{i-1}}^{x_{i+1}}  \left[\mathcal{O}\left(|\Delta x|^2\right)  \frac{(x-x_i)^2}{2} + D\cdot (x_i - x)^3 + \mathcal{O}(|x-x_i|^4)\right] \mathrm{d}x$$

$$=\mathcal{O}\left(|\Delta x|^2\right)  \frac{(2Δx)^3}{6} + D\cdot 0 + \mathcal{O}(|2Δx|^5)$$

$$= \mathcal{O}\left(|\Delta x|^5\right)$$

Due to the symmetry of the integrand, the $D$ term vanished. Note that here $\mathcal{O}(Δx)$ *does not* depend on $x$, whereas $\mathcal{O}(|x-x_i|^4)$ does. One of the terms came from the approximation of the derivative and the other came from the Taylor series. We see now that a better numerical approximation for the second derivative would not have been useful, as the largest error term would still be $\mathcal{O}(|x-x_i|^5)$.

In the final step, we summ over all sub-intervals, which corresponds to multiplying our error term by $N \sim (Δx)^{-1}$ leaving us with an error of $N \mathcal{O}\left(|\Delta x|^5\right) = \mathcal{O}\left(|\Delta x|^4\right)$.

# Proportionality is not Equality
As can be ascertained from our discussion of the Big-O above, the notation $\mathcal{O}((\Delta x)^4)$ can be interpreted as "for $Δx$ sufficiently small, the error is **proportional to** $(Δx)^n$". The error term is in general not equal to $(Δx)^n$. The proportionality constant is arbitrary and may not be on the order of $1$, except by accident. It may even be zero, in which case our error term was estimated too strictly. I believe that the more precise definition given above is more suitable when thinking about the Big-O.

I deducted points where errors were equated with $(Δx)^n$, as this assumption can lead to dangerous overconfidence in results.

So what do we do, when we *know* how the error scales with $\Delta x$? We calculate the value in question for two different $\Delta x$ and cancel out the proportionality constant, just as in Problem 1b.

This method could have been applied to quantify the interpolation error in Problem 2b.

In problem 2b, we have two different error estimates that are of order $\mathcal{O}((\Delta x)^2)$ and $\mathcal{O}((\Delta x)^4)$ respectively. Now, if I make the (maybe not even reasonable) assumption that the proportionality constants are equal in both cases, we can observe that the error for the trapezoidal rule is indeed two orders of magnitude above the error of the simpson rule.

```{admonition} And now: Repeat after me!
Proportionality is *not* equality.
```

Further, keep in mind that if $\Delta x$ is not "small" anymore, the error may scale in arbitrary ways! Thus, we often need to use many datapoints to find out what "small" means :).

# Data and Naming of Files
The code you submit has to run *without modification* on our machines. Most of you did not include ``hw2_data.txt`` in the repository, or even loaded the file directly from their download directory. Please include data in the future, especially if it is a *small* plain-text file!

Also, why do so many of you give your files such cryptic names. A simple structure like ``homework_01/problem_01.nb`` would do the trick. You don't need redundancy like ``homework_1/hw1_p1.nb`` et cetera.

I believe it is most sensible to have **one notebook per problem** as this avoids overriding variables by accident. Also, large notebooks are pretty rough on my poor laptop.

# Log-Log Plots
For error-terms that scale with some small quantity, a log-log plot is ideal.
Polynomial scaling will manifest as a straight line with the slope being equal to the leading power of the polynomial. Note that a curve like $x^n+1$ will *not* be a straight line, so that you can detect when an error does not converge to zero.

One thing that you should consider when plotting on a logarithmic x-axis (such as in a log-log plot) is that you x-values should be exponentially spaced so that they'll *appear* linearly spaced on the plot. Otherwise, the uneven spacing makes it *very* hard to fit or eyeball the scaling. See [numpy.logspace](https://numpy.org/doc/stable/reference/generated/numpy.logspace.html).

```{admonition} Histograms
For histograms, you can pass an array as the `bins` argument to `matplotlib.pyplot.hist`. Using the output of `logspace` as the definition for the bins will give you lovely, uniformly-looking bins in a log-log histogram.
```


# Discussions
Providing a discussion for *every* result you obtain is very important. Sometimes the output of you code doesn't really speak for itself, or the code is hard to understand. Further, the TAs can't read you mind! Having something there in natural language is always a plus and helps us give points when in doubt.

Even when the result is wrong, and you discuss that in some way, we can still give 2 (or sometimes even 3) points :).

In problem 3c some of you only printed a number or showed a plot. I have not deducted points where the result was *unambiguously correct* and there was no discussion. However, meant was that you (in addition to your code) should compare the statistical and the analytical result.

Another example is problem 2b, where some plots clearly showed that there was something wrong with the sampled distribution. Without a discussion it is hard not to deduct a point, as it is not clear whether the student realized, that something is wrong.

# Plots: Axis Labels, Legends
Plots should always have axis labels (on **all** axes), except when
those labels would have absolutely no meaning (such as plotting the
value of a dimensionless quantity without name).

When plotting multiple lines, please make them visually distinct and add a legend where *all* lines show up. On the other hand, if there is only one line in the plot a legend is redundant.

# Notebook Clutter
Please don't leave clutter in you notebooks. By clutter, I mean:

 - remnants of numerical experiments that are *not* required for the solution of a problem
 - large chunks of commented-out code
 - copy pasted code output

This stuff makes it harder to read through the solution.

# Monster Cells
Don't try to do too many things in one cell. If you have to distinguish print outputs by adding extra strings to the output, you might want to split that code into multiple cells. (Of course, the opposite extreme is also undesirable...)

# Re-Evaluate Notebooks from a Cold Start
One problem that comes with interactive coding styles is that often some global variable is defined and then removed from the code at a later point.

For example you might at first write something like
```
k_B = 1.380649e-23


def some_function(x):
    return np.exp(-(x**2) / k_B)
```
and then later remove the definition of `k_B`.
```
def some_function(x):
    return np.exp(-(x**2) / k_B)
```

As long as the session in which the variable was defined is still active you can use the variable all you like. However, when we TAs run your code it won't work, as now the variable has never been defined.

There are multiple issues with the code above that we'll get into below.

**So please restart jupyter and check whether your code still runs.**


# Quantitative Statements: Measure, don't just Claim
I often deducted a point for answers to question 3c) that showed a log-log plot of the error scaling and then claimed that the error scaled in some weird way (like $N^{-7\over 2}$ for example) in the discussion. Usually, there was no reference line in the log-log plot to support such a claim.

The correct answer was $N^{-1/2}=1/\sqrt{N}$. Some people fitted a polynomial to the error curve (nice!), but even drawing $1/\sqrt{N}$ into the same plot would have sufficed.

# Arrays, While and List-Comprehensions
Use arrays whenever possible. Loops are slow in python. Everything is slow in python. You want to use as much of numpy's C backend as possible.

As a side note: ``while``-loops are just as bad as ``for``-loops. List comprehensions are also loops!

# Chemical Potential
## Overflows
Many, many people encountered overflows in problem 2. These came from the exponential in the denominator of the integrand. Although the effects were rather benign in this case, one should never dismiss such warnings *without a discussion*.

Note also, that these problems could be mitigated by rewriting

$$ \frac{1}{1 + e^{x}} = \frac{e^{-x}}{e^{-x} + 1}.$$

Playing with the integration range can be a solution too, although a dangerous one.

## Extrapolation
The spline extrapolation does not have any physical motivation apart
from smoothness. Do not expect correct, physical results from it.

## Integration
The integration routine ``scipy.integrate.quad`` is a fine tool. It is adaptive, gives you an error estimate and **can handle infinite integration ranges**.

Nobody seemed to have realized, that one can actually pass target errors (absolute and relative) as arguments.

## μ Range
Many of you chose an **insanely large** range for $μ$ and the resulting in a low resolution at $μ=0$. This may be due to lack of physical intuition and I did not deduct points solely for a bad range.

What is the chemical potential? It measures how much energy relative to $k_B T$ it takes to change the particle number of the system by one, while keeping the temperature constant. As $k_B T$ is roughly the mean energy per degree of freedom, we wouldn't expect this energy to be much more than around $10 k_B T$.

Beyond those limits, the integral also simplifies enormously and yields the (non)degenerate limits. The region where a numerical approach is interesting, is precisely where these limits don't hold. Here this range was roughly $-2.7 \leq μ \leq 9$.

## Sources of Error
The two (major) sources of numerical error in the calculation are the integration and the interpolation. The integration error can be obtained from the output of `quad`.

The interpolation error (see {doc}`../interpolation`) also scales polynomial in the grid density. Therefore, a similar formula to the one proposed in problem 1b can be employed. The crucial point here is, that the error is **not equal**, but **proportional** to $(\Delta x)^n$. To determine the constant of proportionality, at least two different $\Delta x$ have to be used. Comparing the interpolation the integral at selected points incorporates the integration error into the procedure and very much depends on how these additional points are chosen.

Errors, if treated as standard deviations of random variables, are added as

$$\epsilon_\mathrm{tot} = \sqrt{\sum_n \epsilon_n^2}$$

and should ideally be compared to the absolute value of the integral (see below).

## Temperature and Units
The final, simplified form of the integrand is (with $x=\epsilon/ k_BT$, $\alpha=\mu/k_BT$)

$${N\over Vn_Q} = {4\over\sqrt{\pi}} \int_0^\infty {x^{1/2} dx\over e^{x-\alpha}+1}.$$

The left-hand side is conveniently non-dimensional, as is everything on the right-hand side.
In fact $k_B T = 1$ and $n_Q = 1$ define our unit system in this case (we measure $n=N/V$ in units of $n_Q$).
Any change in temperature can be treated by re-scaling the units without re-evaluating the integral! Therefore, temperature is not an independent parameter here.

It is useful to get rid of units in numerical code. This leads to fewer pre-factor errors, fewer redundancies and to better numerical stability (things are close to 1 where the floating point numbers are densest). The computer does not know about units and that's why it feels so awkward to work with them. You may find the library [pint](https://pint.readthedocs.io/en/stable/) useful.

# Absolute vs. Relative Errors
Be careful about how you discuss numerical errors. The absolute error (not to be confused with the absolute value of an error) is most meaningful when it can actually be related to something measurable.

When comparing values of wildly varying magnitudes or with some arbitrary units unknown to the TA, then stating a relative error is best.

# Style Issues
Although a programming language like python gives us a lot of freedom in how we accomplish a task, there are some style conventions that can make our life much easier.

It's like cleaning the dishes right after using them: It's more effort in the short term; but at least you won't have to share your apartment with tons of bugs and work harder later to chisel the dried up food remnants off your plates.

In the following we will review some common stylistic issues, that came up in the solutions to `HW2`.

There are tools that can help you with many of those issues ([python language server](https://github.com/python-lsp/python-lsp-server), any decent editor (emacs, vscode)). Formatting is a solved problem: [black](https://github.com/psf/black), [autopep8](https://github.com/hhatto/autopep8).

## Action at a Distance
There is a reason, while almost every programming guide and language documentation **strongly discourages** the use of global variables. What is a global variable? Any variable that is not defined within the function or as an object member (in python). Please see [the python documentation on scope](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces).

Let's revisit the example from above:
```
k_B = 1.380649e-23


def some_function(x):
    return -(x**2) / k_B
```

If you do something like that, please keep the definition of `k_B` in the same cell! Otherwise it might end up somewhere far removed from the code it affects and that makes the code extremely hard to reason about.

In this case `k_B` might reasonably be expected to be a constant, but you might later decide to set it to unity. Sometimes I see variables like `x_vals` or similar with extremely generic names. Those are prone to being redefined often.

A simple fix in this case would be
```
def some_function_local(x):
    k_B = 1.380649e-23

    return -(x**2) / k_B
```

Another possibility would be to define some global constants in a cell at the beginning of the notebook or in some other well specified place. Python has no constant variables, but the convention is to make constants `UPPER_CASE`. So you could call `k_B`, or something similar `BLOTZMANN`. The best solution is to use `scipy.constants.Boltzmann` in the example above.

There are even cases where global variables give worse performance:
```python
global_var = 10


def work():
    for i in range(100000):
        10 * global_var


def work_local():
    local_var = 10
    for i in range(100000):
        10 * local_var


%timeit work()
%timeit work_local()
```
Run that code on your machine. I get the output
```text
2.15 ms ± 23.8 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

1.75 ms ± 22 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```

Also, make it explicit what your function operates upon.
In this example, it is nonsensical to define a function (don't even worry that this function is pretty pointless and should be replaced by `numpy.arange`):
```python
values = np.empty(10)

def fill_values():
    for i in range(len(values)):
        values[i] = i
```
It might allow you to fill you `values` more than once, but other than that it is just confusing.

As a first fix, this would be acceptable:
```python
def fill_values(values):
    for i in range(len(values)):
        values[i] = i

    return values
```

Now, we can call `fill_values` everywhere we want, even far away from its definition, and we can still understand what the function does. Everything that is required to understand this function is local to it! You might even decide to augment its behavior later, or to implement it in a completely different way. The semantics are clear: data in, data out. We don't care where it comes from and where it may go.

Be aware that `fill_values` will actually modify its argument. If you want to prevent that, you can do something like
```python
def fill_values(values):
    values = values.copy()

    for i in range(len(values)):
        values[i] = i

    return values
```


You may object that our use of `range` or `len`, or indeed calling any other function inside our function constitutes some sort of nonlocality/action-at-a-distance. Let me counter this by reminding you that these functions ideally do something that we can understand just by looking at their names. They operate in a way that is *easy to understand*: i give them some data, they'll give me some data back (well, well, I won't get into the details here, but look up "pure functions").

### When are globals ok?
This is optional reading :).

Jupyter notebooks are great as a *front-end* to your code. Code cells are great for defining the parameters and input data for your algorithms. Additionally, their output format is ideal for presenting results and plotting graphics.

A rule of thumb is that your code should accept all input data as arguments and define the rest locally and just pass it around. Then in a notebook cell you can call your algorithms like
```python
temperatures = np.linspace(1, 100, 1000)
mass = 10
tolerance = 1e-8

energies = thermal_energy(temperatures, mass, tolerance)
```

Here `thermal_energy` might define auxiliary arrays or other variables internally. However, we will never need to worry about them :), as they're locally. I have not given a definition of that function, as we really don't need to worry about it :P.

And then a cell close by (or even the same cell) might plot the result:
```python
plt.plot(temperatures, energies, xlabel=...)
```

You can use variables in more than one cell. Just make sure that they are not too far away from each other and that the variables have *sensible* names so that you can easily reason about them.

For "generic" variables like ``xs = np.linpspace(...)`` I'd strongly recommend limiting their use to the cell where they are defined.


Sometimes, a collection of parameters might turn up again and again. If you don't want to define all your functions with the same arguments over and over (which makes it hard to maintain), you can collect your parameters in a dictionary, or slightly nicer, like a [dataclass](https://docs.python.org/3/library/dataclasses.html):
```python
@dataclass
class Particle:
    mass: float
    position: np.ndarray
    velocity: np.ndarray


def kinetic_energy(particle):
    return np.linalg.norm2(particle.velocity) ** 2 * mass / 2
```

You could even choose to work in an object-oriented style, but personally I prefer to separate data and algorithms. You might also be tempted to put all you code into a class, but beware: member variables are globals in disguise. If you every only create one instance of a class, you probably don't need one in the first place.

```{admonition} Module-Notebook Style
You can also write [python modules](https://docs.python.org/3/tutorial/modules.html) and import them into you notebook. This provides a nice separation between the implementation and the application to the problems in the homework. You still do the discussion and the evaluation of the code in the notebook, but you can use a proper editor and tools like formatters or linters to edit the modules.
```

## Unused or Trivial Variables
Sometimes I've seen something like:
```python
k_B = 1
m = 1


def some_function(some_argument):
    return do_some_stuff * m / k_B
```

If you're choosing units where `k_B` is equal to unity, why do you put
that into the code? If this seems nontrivial to you, remark about it
in a docstring or comment. Also, you might be interested on [how to find even better units](https://en.wikipedia.org/wiki/Nondimensionalization).

Also, don't compute things you won't use, like:
```python
def some_function():
    a = expensive()
    b = expensive()

    return a
```

Also, keep in mind that you don't have to perform the same task twice:
```python
def some_function():
    array = expensive()

    a = array[array < expensive_too(array)]
    b = array[array >= expensive_too(array)]

    return a, b
```

Rather, you could do something like:
```python
def some_function():
    array = expensive()

    mask = array < expensive_too(array)
    a = array[mask]

    # `~` is the `logical not` operator for arrays
    b = array[~mask]

    return a, b
```


## DRY: Don't repeat yourself
If you realize that you just copy-pasted the same line for the third time you probably want
to define a function for that, or at least use a loop. Copy pasting produces longer and, more importantly, buggier code. If you want to change functionality or fix a bug, you have to do it *multiple times*.

A (rather mild) example would be:
```python
x = some_array
y = some_other_array


x[::2] *= 2
x[1::2] *= 5

y[::2] *= 2
y[1::2] *= 5

# and so on
```
which could be improved as follows:
```python
def scale_elements(array):
    array[::2] *= 2
    array[1::2] *= 5

    # python uses pass-by-reference so returning is optional in this case
    return array


# explicit style
x = scale_elements(x)
y = scale_elements(y)

# implicit hackery: not very clear, discouraged
map(scale_elements, (x, y))
```

You can use `numpy.copy` when you don't want to modify the argument to
a function and return a modified version.

## Names
Naming things is hard. But please try to be descriptive in your variable names. Today, we don't have to conserve space on a punch-card; [the great vowel shortage is over](https://www.youtube.com/watch?v=FyCYva9DhsI)!

So rather than `n_NQV`, something like `particle_density` can be understood at a glance.

Also, in python it is convention to use `snake_case` for variables and functions and `CamelCase` (capitalized!) only for classes and modules. An exception could be, if the thing your function does has a proper name, like `MaxwellBoltzmann`.

## Astropy
Using the constants from astropy is a bit of an overkill, especial in light of [scipy.constants](https://docs.scipy.org/doc/scipy/reference/constants.html).

## Comments
Comments are meant to explain code that doesn't speak for itself or
performs a nontrivial task. Don't write your code twice: once in English and once in python :).

A bad example would be:
```python
# iterate over a range
for i in range(100):
    # set the jth element of y to 2
    y[i] = 2
```
The first comment states the obvious and the second one is subtly wrong.

If you comment your code, please do it for a nontrivial reason. If the comment is extremely short, you can comment on the same line, although having the comment on a separate line is recommended.

Here is an example of a nontrivial remark.
```python

# start at index `2` as we skip the second (index `1`) element
values[2::2] *= 2

# LINE TOO LONG
values[2::2] *= 2 # start at index `2` as we skip the second (index `1`) element
```


For functions, python provides you with the wonderful tool of docstrings. Those allow you to say what the function does and which arguments it takes. This way you don't even have to read the code later on if you just want to be reminded of what the code did.
```python
def some_function(foo, bar):
    """Calculate the baz value from the qux `foo` and the quux `bar`.

    Note that `foo` and `bar` can either be floats or numpy arrays.
    """

    # lot's of code
    # we don't need to know the details at this time...
    pass
```

If the docstring grows out of hand this could be a sign that your function does too many things. It is not uncommon, however, that the docstring is longer than the code.

## Whitespace and Empty Lines
If you wan't to group bits of code together, you can fence them with empty lines. They come for free. However, be consistent in your use of white-space.

For example
```python
x = 1
y = 2
z = complicated()

result = function(x, y, z)
print(f"The result is: {result:.2f}")
```
is much easier to read than
```python
x = 1
y = 2
z = complicated()
result = function(x, y, z)
print(f"The result is: {result:.2f}")
```
or even (*please don't do that*)
```python
#-----
x = 1
y = 2
z = complicated()
#----------------------------
result = function(x, y, z)
print(f"The result is: {result:.2f}")
```


Please be consistent with you indentation levels too.

## Formatting
All the above may already touch on it, but code structures are *much easier to understand* if they are *consistently formatted*. That's why there is the [PEP8](https://pep8.org/)! That's why every major python project you could contribute to on github has a style-guide (see [numpy](https://numpydoc.readthedocs.io/en/latest/format.html) for an example).

Please don't do things like:
```python
first_array = some_stuff[:2, : 4,: :1]
second_array=  whitespace_all_over_the_place(x, y,z )

for i in range(100):
    do_stuff()
for i in enumerate(second_array):
        do_more_stuff()
```

Formatting is a solved problem, and you don't have to do it yourself! For example, running [black](https://github.com/psf/black) on it will give us
```python
first_array = some_stuff[:2, :4, ::1]
second_array = whitespace_all_over_the_place(x, y, z)

for i in range(100):
    do_stuff()

for i in enumerate(second_array):
    do_more_stuff()
```


Jupyter-notebook code-cells **do not replace a proper editor** and formatting code consistently can be a painful, manual process. You could either use an editor like vscode or emacs that provide convenient wrapper around the code cells or try your luck with a [jupyter notebook extension](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator/tree/master/src/jupyter_nbextensions_configurator/static/nbextensions_configurator) that does the formatting for you.
