# Derivatives

## Finite differences

We can derive expressions for derivatives using Taylor expansions. For example

$$f(x+\Delta x) \approx f(x) + \Delta x {df\over dx} + {(\Delta x)^2\over 2} {d^2f\over dx^2} + \mathcal{O}(\Delta x)^3$$

(where the derivatives are evaluated at $x$) gives

$${df\over dx} \approx {f(x+\Delta x) - f(x)\over \Delta x} + \mathcal{O}(\Delta x).$$

This is a *first order* derivative since the error in this approximation scales $\propto \Delta x$.  Because we use the value of the function at $x+\Delta x$ it is known as a *forward difference*. We could also write a similar expression but using the value of the function at $x-\Delta x$; this would be a *backward difference*.

```{admonition} Exercise: second order finite differences
By also considering the Taylor expansion of $f(x-\Delta x)$ show that (hint: add and subtract the two expressions)

$${df\over dx} \approx {f(x+\Delta x) - f(x-\Delta x)\over 2\Delta x} + \mathcal{O}(\Delta x)^2$$

and

$${d^2f\over dx^2} \approx {f(x+\Delta x) -2 f(x) + f(x-\Delta x)\over (\Delta x)^2} + \mathcal{O}(\Delta x)^2.$$

Note that these are both second order accurate. In this case, the first derivative is using a *centered difference*.
```

## Optimal step size

What is the optimal step size $\Delta x$ to choose when evaluating the numerical derivative? Clearly, a smaller $\Delta x$ will improve the accuracy of our approximate expressions. However, we must also consider round off error. Each function evaluation has an associated roundoff error of typical size $\epsilon$ (positive or negative). In general, we can expect the difference $f(x+\Delta x) - f(x)$ to have a roundoff error $\sim \epsilon$. The total error from the first order derivative is therefore 

$$\mathrm{total\ error}\approx {\epsilon f(x)\over \Delta x} + {\Delta x f^{\prime\prime}\over 2},$$

where the first term is the roundoff error and the second term is from the term we neglected in the Taylor expansion when we wrote down the first order forward-difference.

Minimizing this expression with respect to $\Delta x$ gives

$$\Delta x\approx (2\epsilon)^{1/2} (f/f^{\prime\prime})^{1/2}.$$

We see that the minimum error that can be achieved is of order $\sqrt{\epsilon}$. This is $\sim 10^{-8}$ for double precision floats.

```{admonition} Exercise: error and optimal step size in finite differences

Choose a function $f(x)$ that has a derivative that you can calculate analytically (to check your answer). Then compute the first order (forward difference) numerical derivative for different values of $\Delta x$. Plot a log-log plot showing the absolute value of the error against the step size. Do you see the expected scaling of the error with step size $\Delta x$? 

Next try using a second order derivative (centered difference). Add this to your plot and compare with your previous results. How does the error scale with $\Delta x$ now? Can you explain what you see?

Do your results depend on where you calculate the derivative (which value of $x$)? For the first order derivative, do you see the dependence on $f^{\prime\prime}$ predicted by our estimate above?
```

SciPy has a routine to calculate derivatives based on centered differences to a specified order -- see [`scipy.misc.derivative`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.misc.derivative.html) (Note that this routine is deprecated and will be removed at some point, the documentation for this function has some suggested replacements). You could try using this routine with different orders in your code from the exercise above and see how it compares. 



## Automatic differentiation

Another approach to differentiation is [*differentiable programming*](https://en.wikipedia.org/wiki/Differentiable_programming) and [*automatic differentiation*](https://en.wikipedia.org/wiki/Automatic_differentiation). 
The idea is that when a function is evaluated, we keep track of every basic operation that is carried out on each variable (addition, multiplication, exponential/logarithms, sin/cos etc), along with the partial derivatives for each step. Whenever we need a gradient, we can then use the chain rule to calculate the gradient of the function. 

For example, if we have a function $f(x)$ that is evaluated in two steps, first an operation $y(x)$ and then $f(y)$, i.e. $f(y(x))$, then

$${df\over dx} = {df\over dy}{dy\over dx},$$

where the first derivative on the right hand side is evaluated at $y=y(x)$ and the second is evaluated at $x$. If we add more steps, e.g. $f(z(y(x)))$, we introduce more factors in the chain.

The advantage of automatic differentiation is that you can get machine precision derivatives, avoiding the discretization error associated with finite differencing. 

Here is a simple implementation of automatic differentiation in Python, which is from the Wikipedia page on automatic differentiation (I've added some comments to help see what is happening in the code).

```
import math

class Var:
    def __init__(self, value, children=None):
        self.value = value
        self.children = children or []
        self.grad = 0

    # Every time we do an operation, we return the result and also 
    # the values of the partial derivative with respect to each variable
    
    # Addition and multiplication are implemented by overloading the + and * operators
    # (see https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)
    def __add__(self, other):
        return Var(self.value + other.value, [(1, self), (1, other)])

    def __mul__(self, other):
        return Var(self.value * other.value, [(other.value, self), (self.value, other)])

    def sin(self):
        return Var(math.sin(self.value), [(math.cos(self.value), self)])

    # To calculate the gradient, recursively implement the chain rule
    def calc_grad(self, grad=1):
        self.grad += grad
        for coef, child in self.children:
            child.calc_grad(grad * coef)

# Example: f(x, y) = x * y + sin(x)
x = Var(2)
y = Var(3)
f = x * y + x.sin()

# Calculation of partial derivatives
f.calc_grad()

print("f =", f.value)
print("∂f/∂x =", x.grad)
print("∂f/∂y =", y.grad)

# Compute the analytic derivatives to check the answers
print("Expected values are:")
print("f =", x.value * y.value + math.sin(x.value))
print("∂f/∂x =", y.value + math.cos(x.value))
print("∂f/∂y =", x.value)
```

```{admonition} Exercise: automatic derivatives
Try running this code. You should find that the results agree with the analytic derivatives. Now implement an exponential function and use it to calculate the derivative of 

$$f(x) = \exp\left(\sin(x)\right)$$

for different values of $x$. Compare your answer with the analytic expression and with the finite difference gradient.
```

**Further reading**

- The example above is based on the Wikipedia page for [*automatic differentiation*](https://en.wikipedia.org/wiki/Automatic_differentiation). 

- Automatic differentiation is also discussed in Section 3.4 of Gezerlis.

- Automatic differentiation is particularly useful in applications where there are many independent variables, such as in machine learning. For implementations in Python that can automatically differentiate NumPy code, take a look at [Autograd](https://github.com/HIPS/autograd) and [JAX](https://github.com/google/jax).

