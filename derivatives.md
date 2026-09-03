# Derivatives

## Finite differences

We can derive expressions for derivatives using Taylor expansions. For example

$$f(x+\Delta x) \approx f(x) + \Delta x {df\over dx} + {(\Delta x)^2\over 2} {d^2f\over dx^2} + \mathcal{O}(\Delta x)^3$$

(where the derivatives are evaluated at $x$) gives

$${df\over dx} \approx {f(x+\Delta x) - f(x)\over \Delta x} + \mathcal{O}(\Delta x).$$

This is a *first order* derivative since the error in this approximation scales $\propto \Delta x$.  Because we use the value of the function at $x+\Delta x$ it is known as a *forward difference*. We could also write a similar expression but using the value of the function at $x-\Delta x$; this would be a *backward difference*.

:::{admonition} Exercise: second order finite differences
:class: tip

By also considering the Taylor expansion of $f(x-\Delta x)$ show that (hint: add and subtract the two expressions)

$${df\over dx} \approx {f(x+\Delta x) - f(x-\Delta x)\over 2\Delta x} + \mathcal{O}(\Delta x)^2$$

and

$${d^2f\over dx^2} \approx {f(x+\Delta x) -2 f(x) + f(x-\Delta x)\over (\Delta x)^2} + \mathcal{O}(\Delta x)^2.$$

Note that these are both second order accurate. In this case, the first derivative is using a *centered difference*.

:::


## Optimal step size

What is the optimal step size $\Delta x$ to choose when evaluating the numerical derivative? Clearly, a smaller $\Delta x$ will improve the accuracy of our approximate expressions. However, we must also consider round off error. Each function evaluation has an associated roundoff error of typical size $\epsilon$ (positive or negative). In general, we can expect the difference $f(x+\Delta x) - f(x)$ to have a roundoff error $\sim \epsilon$. The total error from the first order derivative is therefore 

$$\mathrm{total\ error}\approx {\epsilon f(x)\over \Delta x} + {\Delta x f^{\prime\prime}\over 2},$$

where the first term is the roundoff error and the second term is from the term we neglected in the Taylor expansion when we wrote down the first order forward-difference.

Minimizing this expression with respect to $\Delta x$ gives

$$\Delta x\approx (2\epsilon)^{1/2} (f/f^{\prime\prime})^{1/2}.$$

We see that the minimum error that can be achieved is of order $\sqrt{\epsilon}$. This is $\sim 10^{-8}$ for double precision floats.

:::{admonition} Exercise: error and optimal step size in finite differences
:class: tip

Choose a function $f(x)$ that has a derivative that you can calculate analytically (to check your answer). Then compute the first order (forward difference) numerical derivative for different values of $\Delta x$. Plot a log-log plot showing the absolute value of the error against the step size. Do you see the expected scaling of the error with step size $\Delta x$? 

Next try using a second order derivative (centered difference). Add this to your plot and compare with your previous results. How does the error scale with $\Delta x$ now? Can you explain what you see?

Do your results depend on where you calculate the derivative (which value of $x$)? For the first order derivative, do you see the dependence on $f^{\prime\prime}$ predicted by our estimate above?

:::

