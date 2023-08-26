# PHYS 512 Computational Physics with Applications: Winter 2023 Course outline

## Description and goals

A course in computational methods in physics. Computing is important in all aspects of modern physics, whether it be solving non-linear equations for a theoretical calculation or analyzing and modelling data sets in an experiment. By taking this course, you will 

- become familiar with a range of computational techniques that can be applied to physics problems, and know how to get started on tackling a problem numerically.

- understand the essential ideas underlying standard computational methods, and become familiar with their implementation in Python, in particular in the NumPy and SciPy libraries.

- gain experience in solving a range of different physics problems numerically.

There are no prerequisites, but some previous experience in Python writing basic programs and plotting data etc. is recommended (for example, as seen in earlier lab courses).

## Instructor and TAs

Prof. [Andrew Cumming](https://www.physics.mcgill.ca/~cumming/), andrew.cumming@mcgill.ca, Rutherford 310.

The TAs are Valentin Boettcher, Simon Guichandut, and Matthew Lundy.


## Time and place

Class will be held on Tuesdays and Thursdays from 1-2.30pm in WONG 0190.

The first class is Thursday August 31st and the final class is on Tuesday Dec 5th. There is no class on Tuesday October 10 (Reading break) or Thursday November 30 (makeup day with a Monday schedule). 


## List of topics

A preliminary list of topics is given below [The ordering of topics may change, and we may not cover all of the subtopics, depending on time available.]

- *Computing basics.* Getting set up. NumPy and SciPy libraries. Github. Matplotlib. Programming best practices. Floating point arithmetic, roundoff error. Speed of computation. 

- *Differentiation, integration, and interpolations of functions.* Finite differences, optimal choice of interval, automatic differentiation. Polynomial interpolation and splines. Integration: Simpson's rule, Gaussian quadrature, singularities and indefinite integrals.

- *Random variables.* Generating random numbers. Transformation and rejection methods. Monte Carlo integration. MCMC and Metropolis-Hastings.

- *Matrices and solving linear equations.* Linear systems of equations and eigenvalue problems. Least squares fitting. Condition number. Gaussian elimination, LU decomposition, SVD, QR decomposition, conjugate gradient for sparse matrices.

- *Solving non-linear equations.* Non-linear least squares. Root finding and Newton's method. Levenberg-Marquardt. MCMC.

- *Fourier transforms and spectral analysis.* Discrete FT and FFT, convolution theorem, aliasing and Nyquist theorem. Image processing. Matched filters.

- *Ordinary differential equations.* Initial value and boundary value problems. Runge Kutta methods for ODEs. Stiff equations and implicit methods. Shooting and relaxation methods for boundary value problems. Symplectic integrators.

- *Partial differential equations.* Examples of PDEs: Poisson's equation by FFT, wave equation, advection and fluid flow. Finite difference and finite volume, stability, numerical dissipation, Lax method, implicit and flux-conservative schemes.

- *A brief introduction to machine learning.* Example application to high energy physics data.

These topics will be illustrated with a range of different physics applications, such as simulating the propagation of electromagnetic waves in plasmas, numerical solutions of the Schrodinger equation, thermal instabilities and non-linear oscillators, fitting gravitational wave data, and other examples.


## Useful books and other resources

There is no required textbook for the course, but an excellent Python-based book available through the McGill library is 

- [Numerical Methods in Physics with Python](https://mcgill.on.worldcat.org/search/detail/1162187759?queryString=ti%3A%28numerical%20methods%20physics%29&databaseList=283%2C638&origPageViewName=pages%2Fadvanced-search-page&clusterResults=&groupVariantRecords=&expandSearch=true&translateSearch=false&queryTranslationLanguage=&lang=en&scope=wz%3A12129) by Alex Gezerlis (the link is to the McGill library ebook)

Other books you could look at are:

- [Computational Physics](http://www-personal.umich.edu/~mejn/cp/) by Mark Newman. Also an excellent Python-based book. It is only available in paper format from the library, but I've linked to the book website which has some sample chapters that you can look at.
- [An Introduction to Computational Physics](https://mcgill.on.worldcat.org/search/detail/63814390?queryString=tao%20pang%20computational&expandSearch=true&translateSearch=false&databaseList=283%2C638&clusterResults=true&groupVariantRecords=false) by Tao Pang (the link is to the McGill library ebook). Excellent coverage of the topics in this course, with code samples in Fortran.
- [Computational Physics](https://www.physics.purdue.edu/~hisao/book/) by Giordano and Nakanishi. An interesting book that is organized by physics topic rather than numerical method.
- [Numerical Recipes: The Art of Scientific Computing](http://numerical.recipes/book.html) by Press, Teukolsky, Vetterling and Flannery. The classic book on numerical methods with really good explanations. The downside is that the C/Fortran code comes with a very restrictive license. Available to read online (although with annoying popups unless you purchase a license).

There are also other courses on computational physics that have materials online:

- [Mike Zingale's class AST 390: Computational Astrophysics at Stonybrook](https://zingale.github.io/computational_astrophysics/intro.html)

- [Computational Physics tutorials at the University of Toronto](https://computation.physics.utoronto.ca)

- [Course materials for PHY407 at UofT](https://github.com/PHY407-UofT)

Here are some other useful links:

- [SciPy documentation](https://docs.scipy.org/doc/scipy/tutorial/index.html#user-guide)

- [NumPy documentation](https://numpy.org/doc/stable/)

- [Matplotlib documentation](https://matplotlib.org/stable/#)

## Assessment

Your grade will be based on 

- Homeworks (30%), given out every 1-2 weeks during the term. The lowest homework score will be dropped when calculating the final grade. 
- Project (30%). The project component will involve developing a code to investigate a physics problem of interest to the student. The project will be due at the end of term. More information will be provided in the first week of classes.
- Take-home final exam (40%). The take-home exam will be available for a 72 hour period and designed to be completed in 3 hours.

Lecture notes and assignments will be made available through this website. Homework and project submissions will be through each students private Github repository. Grades will be distributed in myCourses.

## McGill policy statements

McGill University values academic integrity. Therefore all students must understand the meaning and consequences of cheating, plagiarism and other academic oﬀences under the [Code of Student Conduct and Disciplinary Procedures](https://www.mcgill.ca/secretariat/files/secretariat/code_of_student_conduct_and_disciplinary_procedures.pdf) (approved by Senate on 29 January 2003) (See McGill's guide to academic honesty](https://www.mcgill.ca/students/srr/honest) for more information).

In accord with McGill University's [Charter of Students' Rights](https://www.mcgill.ca/secretariat/files/secretariat/charter_of_student_rights_last_approved_october_262017.pdf), students in this course have the right to submit in English or in French written work that is to be graded. This does not apply to courses in which acquiring proficiency in a language is one of the objectives.

In the event of extraordinary circumstances beyond the University's control, the content and/or evaluation scheme in this course is subject to change. 

Additional policies governing academic issues which aﬀect students can be found in the [McGill Charter of Students' Rights](https://www.mcgill.ca/secretariat/files/secretariat/charter_of_student_rights_last_approved_october_262017.pdf).
