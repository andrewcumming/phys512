# Getting set up

## To do

Here is a summary of what you need to do to get set up for the course:

- Join the **Slack** workspace for the class. You should have received email from Simon via myCourses (check your spam folder!). 

- Fill out the **when2meet poll** that Simon sent in the same email so we can find a time for the **Debug den**.

- Mkae a **Github account** if you don't already have one, and make a private repository where you will submit your homework (as a Jupyter notebook for each homework).  If you are making a new account, choose your username carefully, your Github account is a great place to build your computing portfolio, e.g. for future job searches!

- Make sure you have **Python 3** installed and the **NumPy**, **SciPy**, and **Matplotlib** libraries.

- We will be doing interactive exercises in class, so you should **bring your laptop to every class**. The classroom likely has limited power outlets available, so please charge your battery before class if needed!

**If you need help with installation, setting up a Github repository, or if you have questions about NumPy and Matplotlib basics, let the TAs know, they can help you in the Debug den sessions.**


## Python

In this course, we will use [Python 3](https://docs.python.org/3/). This will build on the exposure to Python you have already had in earlier courses. Python has the advantages of a high level language, for example, concise code, rapid prototyping, no compilation step, interactive use, while also having access to efficient numerical libraries that are implemented in C or Fortran under the hood. You may be interested in exploring the use of other languages for numerical computations, but for this course all submitted material must use Python 3.

We will assume that you have access to 

- [NumPy](https://numpy.org/doc/stable/)
- [SciPy](https://docs.scipy.org/doc/scipy/tutorial/index.html#user-guide)
- [Matplotlib](https://matplotlib.org/stable/#)
- [Jupyter notebook](https://jupyter.org)

If you are not already set up to run Python, a good way to install it is through [Anaconda](https://www.anaconda.com/download) which will also install all the libraries you need. You may have Python installed but be missing SciPy for example, in which case you can try `pip install scipy`.

How you interact with Python is up to you. A really great way to code is in a Jupyter notebook. Homeworks should be submitted as Jupyter notebooks in your Github repository.  

It might be helpful to have a look at some NumPy tutorials to remind you how to create and manipulate arrays etc., and to remind yourself how to make a basic plot in Matplotlib if needed. The above are to the documentation for each library, which have basic introductory tutorials.  Otherwise, there are many tutorials you can find online. One place to look is at [Mike Zingale's class AST 390: Computational Astrophysics at Stonybrook](https://zingale.github.io/computational_astrophysics/intro.html)
which has a section covering NumPy and Matplotlib. 

```{admonition} Exploring NumPy
Just to get you started with NumPy, here are some expressions you can use to begin with:

Creating arrays

- `a = np.array([1.0,2.0])`
- `np.ones(10)`
- `np.zeros(10)`
- `np.ones_like(a)`
- `np.arange(2.0,10.0,0.1)`
- `np.linspace(2.0,10.0,num=100)`

Slicing arrays

- `a[1:10]`
- `a[:-1]`
- `a[::2]`
- `a[::-1]`

To see what operations are available for an object

`dir(a)`

It is helpful to be aware of when a variable is a reference (pointer) to an array and when an array is copied or not:

- `B=A`   points to same object (`A is B` will return True)
- `B=A[:]`   shallow copy/view (same memory)
- `B=A.copy()`    deep copy   (`A is B` will return False)
```


## Version control and Github

In this course, we will make use of `git` which is a *version control* system (in particular, you will use this to submit your homework and will work in a collaborative group for your project).  Version control systems let you

- keep track of changes that you make and revert back to previous versions

- merge contributions from others (and deal with conflicts) and make contributions yourself to other projects

- see who contributed what code and when it was added

- work on updates to the code in separate "branches"

- if your central repository is based on Github, it provides a backup

You should become familiar with the basic git operations

- `git init` or `git clone`

- `git status` and `git log`

- `git add`

- `git commit -m` or `git commit -am`

- `git push`

- the `.gitignore` file

A place to get started is

- [Step-by-step: How to Start with Git and Create a Repository in GitHub](https://herewecode.io/blog/create-repository-github/)

which is a concise description of creating a repository on github and then cloning it to a local directory.

You can also look at the section on Github in [Mike Zingale's class AST 390: Computational Astrophysics at Stonybrook](https://zingale.github.io/computational_astrophysics/git/version-control.html) and the tutorials on Github, [Github quickstart](https://docs.github.com/en/get-started/quickstart/).
