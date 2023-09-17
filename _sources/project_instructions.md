# Project Instructions

The project component of the course will make up 30% of your grade. The **due date** will be the **last day of class (Dec 5)**. 

Working in a group of 2 or 3, you should solve a physics problem of interest to you using computational methods. You can write your own code, use existing code and libraries, or a combination of the two. The project can involve a theory calculation or modelling data or both. 

The topic must be approved by the instructor **by Friday Oct 6**. Before this date, you should send a short (couple of paragraphs) abstract to the instructor in a Slack message with all group members included. The abstract should describe the topic and the numerical approach that you plan to take. 

**What you should submit**

Each group should submit one Github repository containing the code and a write up of the project which explains the physics problem you are looking at, the methods you used to tackle it, your results and conclusions. 

The write up should be a PDF file of no more than 10 pages. A suggested structure for the write up is
- Introduction
- Problem Description
- Code Description
- Code Tests
- Results and Analysis
- Discussion and Further Improvements
- Conclusion
- A statement of contributions that describes the contributions of each group member to the project.

Your report should be prepared in LaTeX and for any code snippets that you include you should use code formatting as described here: https://www.overleaf.com/learn/latex/Code_listing

The code you submit in your repository should be clearly commented and in a state where it is ready to run to reproduce the results you show in your write up. Note however that the write up should include all your key results (e.g. important plots that you make) -- we should be able to understand and see your results without having to run the code.

**Grading**

The grade for the project will be based on how well you

- explain the physics problem that you are trying to solve, why it is interesting and why computational techniques are needed to solve it

- used appropriate numerical tools to tackle your problem, whether this is to implement an algorithm yourself or apply an existing code to your problem

- explain what you have done and how the code works so that another student in the course could understand what you did and reproduce your work.

- demonstrate critical thinking while carrying out the project. For example, by checking whether your code was producing the right answer by comparing with analytic solutions in appropriate limits or comparing with previous published results, or by discussing whether the results make sense physically.



**Project topic**

There are many places to look to get ideas. For example, you can look at the projects that have been done at the [McGill Physics Hackathon](https://mcgill-physics-hackathon-2022.devpost.com/project-gallery), or Gezerlis' book has some interesting projects at the end of each chapter that could be good starting points. 

To give you a sense of an appropriate scope for the project, here are a few specific project ideas:

- Use molecular dynamics to simulate particles interacting with a Lennard-Jones potential and investigate for example the transition from liquid to solid, or the response of a solid lattice to impurities.

- Integrate the geodesic equations in the Schwarzschild metric and produce an image of a black hole surrounded by an accretion disk (as in the movie *Interstellar*).

- (This one is from last year's course) Write an N-body code that calculates the forces by computing the gradient of the potential. The potential should be found by gridding the particle positions into a density, and convolving that density with the (softened) potential from a single particle. The acceleration is then found by taking the gradient of the potential. You will probably wish to use a leapfrog solver with ﬁxed timestep. Use it to investigate a situation with at least hundreds of thousands of particles.


