# Linear least squares

Linear least squares refers to the problem of minimizing $\chi^2$ with a model that can be written as a linear combination of *basis functions* $f_j(x)$ as 

$$m(x) = \sum_{j=1}^M a_j f_j(x)$$

where $a_j$ are the model parameters. For example if we wanted to fit a polynomial we would choose $f_j(x) = x^{j-1}$ and minimize $\chi^2$ to find the polynomial coefficients $\{a_j\}$.

We'll follow the analysis of Numerical Recipes with some small changes in notation. Write out $\chi^2$ and minimize it:

$$\chi^2 = \sum_{i=1}^N \left[ {y_i - \sum_{j=1}^M a_j f_j(x_i)\over \sigma_i}\right]^2$$

$${\partial \chi^2\over\partial a_k} = 0 \Rightarrow \sum_{i=1}^N {1\over\sigma_i^2}\left[ y_i - \sum_{j=1}^M a_j f_j(x_i)\right] f_k(x_i) = 0$$

$$\sum_{i=1}^N {y_i\over\sigma_i}{f_k(x_i)\over\sigma_i} - \sum_{i=1}^N\sum_{j=1}^M a_k {f_j(x_i)\over\sigma_i }{f_k(x_i)\over \sigma_i} = 0$$

At this point it is useful to define the *design matrix*

$$A_{ij} = {f_j(x_i)\over \sigma_i}$$

and also the vector $\vec{d}$ which is the data weighted by the errors at each point:

$$d_i = {y_i\over\sigma_i}.$$

Then we have 

$$\sum_{i=1}^N A_{ik} b_i - \sum_{i=1}^N\sum_{j=1}^M a_k A_{ij}A_{ik} = 0$$

$$\sum_{i=1}^N (A^T)_{ki} b_i  - \sum_{j=1}^M \sum_{i=1}^N  (A^T)_{ji}A_{ik} a_k  = 0$$

So in matrix form, we have 

$$\mathbf{A^T} \mathbf{d} = \mathbf{A^T}\mathbf{A}\mathbf{a}.$$

These are known as the **normal equations** for the linear least squares problem. 

We've assumed here that the noise is uncorrelated between measurements (the noise on one measurement does not depend on the noise on a previous measurement). The normal equations can be generalized to include correlated noise, but we'll leave this for later and for now focus on how we can solve these equations.

```{admonition} Exercise:

Show that $\chi^2$ can be written in our matrix notation as 

$$\chi^2 = (\mathbf{d} - \mathbf{A}\mathbf{a})^T  (\mathbf{d} - \mathbf{A}\mathbf{a}).$$

```