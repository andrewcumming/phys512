# Non-linear least squares

Now let's discuss what happens if we have a non-linear model $m(x,\vec{a})$ that we want to fit to a set of measurements $y_i(x_i)$. We want to minimize 

$$\chi^2 = \sum_{i=1}^N \left[{y_i - m(x_i, \vec{a})\over \sigma_i}  \right]^2.$$

We'll first discuss Newton's method and its application to least-squares, and then a generalization of this method, Levenberg-Marquardt.

## Newton's method

The simplest way to see how Newton's method works is to consider a one-dimensional function $f(x)$. We would like to find the value of $x$ that minimises $f(x)$. Given a current guess for the location of the minimum, $x_n$, we first write down a quadratic expansion around $x_n$:

$$f(x) = f(x_n) + (x-x_n) \left.{df\over dx}\right|_{x_n} + {(x-x_n)^2\over 2} \left.{d^2f\over dx^2}\right|_{x_n}.$$

We find an updated value for the location of the minimum $x_{n+1}$ by minimizing this quadratic expansion: 

$${df\over dx}=0 \Rightarrow \left.{df\over dx}\right|_{x_n} + (x_{n+1} -x_n) \left.{d^2f\over dx^2}\right|_{x_n}=0$$

$$\Rightarrow x_{n+1} = x_n - \left(\left.{d^2f\over dx^2}\right|_{x_n}\right)^{-1} \left.{df\over dx}\right|_{x_n}.$$ (Newton)

To the extent the function is locally well-approximated by a quadratic, this update should take us closer to the minimum. Newton's method is to iteratively apply equation {eq}`Newton` until it converges, for example when the fractional change in $x$ is smaller than a threshold.

Following through the same argument in multi-dimensions, the quadratic expansion becomes

$$f(\vec{x}) = f(\vec{x_n}) + (\vec{x}-\vec{x_n})\cdot\vec{\nabla} f(\vec{x_n}) + {1\over 2}(\vec{x}-\vec{x_n})\cdot \mathbf{H} \cdot (\vec{x}-\vec{x_n}),$$

where $\mathbf{H}$ is the *Hessian matrix*

$$(\mathbf{H})_{ij} = {\partial^2 f\over \partial x_i\partial x_j}.$$

The update becomes

$$\vec{x_{n+1}} = \vec{x_n} - \mathbf{H}^{-1}\cdot\vec{\nabla} f(\vec{x_n}).$$ (newtonmultid)

## Application to least-squares fitting

To apply this to least-squares, we first need the gradient of $\chi^2$ with respect to the parameters, $\vec{\nabla} \chi^2$, whose $k$-th component is given by 

$${\partial \chi^2\over\partial a_k} = -2 \sum_i \left[{y_i - m(x_i, \vec{a})\over \sigma_i}  \right] {1\over \sigma_i} {\partial m(x_i, \vec{a})\over\partial a_k}.$$

If we define the matrix 

$$A_{ik} \equiv {1\over \sigma_i} {\partial m(x_i, \vec{a})\over\partial a_k}$$

and the residual vector $\mathbf{r}$ with $i$-th component

$$r_i = {y_i - m(x_i, \vec{a})\over \sigma_i}$$

then we have

$${1\over 2} {\partial \chi^2\over\partial a_k} = - \sum_i A_{ik} r_i$$ (gradchi2)

or in matrix form

$${1\over 2} \vec{\nabla} \chi^2 = - \mathbf{A^T}\mathbf{r}.$$ (gradchi2vec)

We can find the Hessian matrix by taking another derivative of equation {eq}`gradchi2`, giving  

$${1\over 2} {\partial^2 \chi^2\over\partial a_k \partial a_\ell} = \sum_i A_{ik}A_{i\ell} + \sum_i r_i {\partial^2 m(x_i)\over \partial a_k \partial a_\ell}.$$

The second term involving the second derivative of the model is usually dropped with the argument that near the best-fitting solution, $r_i$ should be small, especially when summed over $i$ and positive and negative values cancel. Neglecting this term, we get

$${1\over 2} {\partial^2 \chi^2\over\partial a_k \partial a_\ell} = (\mathbf{A^T}\mathbf{A})_{k\ell}.$$ (hessian)

Now plugging equations {eq}`gradchi2vec` and {eq}`hessian` into the Newton's method update (eq. {eq}`newtonmultid`), we find

$$\vec{a}_{n+1} = \vec{a}_n - (\mathbf{A^T}\mathbf{A})^{-1} \mathbf{A^T} \mathbf{r}.$$ (newtonupdate)

Starting with an initial guess for the parameters, we can apply this rule iteratively until (hopefully) we converge on the best fit (ie. the set of parameters that minimizes $\chi^2$).

````{admonition} Exercise: Lorentzian fit
Let's apply this method to fit a set of data points with a Lorentzian

$$ f(x) = {a_0\over a_1 + (x-a_2)^2}.$$

The following function takes a vector of parameters for the Lorentzian $\vec{a}=(a_0,a_1,a_2)$ and a set of $x$ values and returns $f(x)$ and the matrix $\mathbf{A}$: 

```
    def lorentz(a, x):
        # Calculates the Lorentz function and analytic derivatives 
        f = a[0] / (a[1] + (x-a[2])**2)
        A =  np.zeros([len(x), len(a)])
        A[:, 0] = 1 / (a[1] + (x-a[2])**2)
        A[:, 1] = - a[0] / (a[1] + (x-a[2])**2)**2
        A[:, 2] = 2 * a[0] * (x-a[2]) / (a[1] + (x-a[2])**2)**2
        return f, A
```

In this function, we populate the matrix $\mathbf{A}$ with the derivatives of the Lorentzian with respect to each of the parameters. As usual, for simplicity we assume constant errors $\sigma_i$ so that they cancel out of the equations.

Now generate a set of measurements including some Gaussian noise:

```
a0 = np.array((1.2, 2, 0.3))   # 'true' parameters of the Lorentzian
ndata = 100   # number of measurements 
x = np.linspace(-10, 10, ndata)
y, _ = lorentz(a0, x) 
y = y + 0.03 * np.random.normal(size = ndata)
```

- Implement Newton's method to fit the data points $y(x)$ with a Lorentzian function. Start with a guess for the parameters and use equation {eq}`newtonupdate` to iteratively improve the guess. As a stopping criterion, you could monitor the size of the residual and stop when it falls below a certain value (e.g. `np.sqrt(np.mean(r**2))`). [Depending on your initial guess, you might also get cases where the algorithm does not converge, in which case it will never reach your stopping criterion. One way to look out for these cases is to stop when the error gets larger after an iteration rather than getting smaller.]

- Once you converge on an answer for the best-fit parameters, plot the corresponding Lorentzian on top of your data points to confirm visually that you are getting a good fit. Also, compare your best-fitting parameters with the input parameters used to generate the measurements.

- Next, modify the `lorentz` function so that it calculates the $\mathbf{A}$ matrix using finite differences rather than analytic derivatives. Check that you get the same answer as before.

````
