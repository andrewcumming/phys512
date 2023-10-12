# Homework 4

Due on Tuesday Oct 24 by midnight.

1. **More on SVD**

(a) For the singular value decomposition of a matrix $\mathbf{A} = \mathbf{U}\mathbf{S}\mathbf{V^T}$, show that $\mathbf{A}$ can also be written as

$$\mathbf{A} = \sum_\alpha s_\alpha \mathbf{u_\alpha} \mathbf{v_\alpha^T},$$ (sum)

where $s_\alpha$ are the singular values (that make up the diagonal of $\mathbf{S}$), and $\mathbf{u_\alpha}$ and $\mathbf{v_\alpha}$ are the column vectors that make up the matrices $\mathbf{U}$ and $\mathbf{V}$. This shows that the matrix $\mathbf{A}$ can be decomposed into a linear combination of the matrices formed by the outer product of each pair of $\mathbf{u_\alpha}$ and $\mathbf{v_\alpha}$ vectors. (These are rank-1 matrices since they are constructed from a single column and row vector.)

(b) Construct a $10\times 6$ matrix $\mathbf{A}$ filled with random numbers (uniform between 0 and 1) and compute the component matrices $\mathbf{u_\alpha} \mathbf{v_\alpha^T}$. Plot color maps of the component matrices. Compute the sum in equation {eq}`sum` and confirm that it gives you the original matrix.

(c) Now try truncating the sum in equation {eq}`sum`, i.e. take only the $n$ largest singular values in the sum. Calculate the mean error between the reconstructed matrix and the original matrix as a function of $n$. (For a given $n$, the matrix constructed in this way is a rank-$n$ approximation of the matrix $\mathbf{A}$.)

(d) Next try this on an image. You can use your own or otherwise I've provided one here: [github_logo](https://github.com/andrewcumming/phys512/blob/main/github_logo.png). You can load this into a matrix using

```
from PIL import Image
img = Image.open('github_logo.png')
A = np.asarray(img)[:,:,0]   # the last index selects the RG or B component
```
Plot color maps of the component matrices in this case, and the reconstructed images for different ranks $n$. 

Choose the smallest value of $n$ that in your opinion gives a reasonable reproduction of the image. How much data do you need to store to be able to reproduce this rank-$n$ approximation of the image? What is the compression factor compared to the original image?

2. **Fitting planetary orbits**

(a) Repeat the [exoplanet orbit fitting](https://andrewcumming.github.io/phys512/metropolis_solutions.html#exoplanet-orbit) exercise that we did using MCMC but instead using Levenberg-Marquardt to find the best fitting parameters for the planet. As before, keep the period fixed and find the best-fitting values of the other 5 parameters. Compare your answers with what we found using MCMC.

[Hint: you may find that taking a full Newton step causes problems, for example by trying to jump out of the range $e=0$--$1$. In that case, you can artifically reduce the step, ie. update using $\vec{a}_{n+1} = \vec{a}_n  + \alpha\, \delta\vec{a}$ for some $\alpha < 1$ (I used $\alpha=0.2$).]

(b) In a least-squares fit, the *covariance matrix* $\mathbf{C} = (\mathbf{A^T}\mathbf{A})^{-1}$ gives the errors in the parameters, ie. $C_{ii}$ is the variance in $a_i$, and $C_{ij}$ is the covariance of parameters $a_i$ and $a_j$. Evaluate $\mathbf{C}$ for your best fit and compare with what we found with MCMC. One way to do this is to compute $\mathbf{C}$ from the MCMC samples.

