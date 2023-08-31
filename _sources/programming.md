# Programming best practices

No matter what your previous programming experience, hopefully you will learn a lot by doing the exercises and homeworks in this course. We'll have a discussion about some best practices for writing code in the first class.

Here are a few things to keep in mind:

- **Don't write code unless you have to**. Is there an existing library or open source code that does what you need to do? if so, you don't need to reinvent the wheel. For this course, we will use many libraries from NumPy and SciPy. You can also search on Github to find open source codes.

- **Readability is really important**. Even if you think that no one else will ever have to read your code, you will thank yourself when you come back to the code after a year and can't remember why you wrote it the way you did! Readability can mean including helpful comments (using `#` in Python), but is also helped by the code layout, variable naming etc. *Very important for this course*: you need to write readable code for the homeworks so that the TA's can understand it and grade it!

- **Write once**, also known as DRY (don't repeat yourself). It's often tempting to cut and paste code when you have to do something multiple times. An simple example is when you are making multiple plots in a matplotlib script. Instead, put the code into a function or inside a loop. Having one copy of the code in one place will save endless debugging headaches.

- **If you get stuck, someone else almost certainly had the same problem.** Google, Stack Overflow, or AI code generators such as ChatGPT or Github Copilot will often quickly help you find the answer (but may also give you wrong answers, especially the AI models!). *Note that any code that you hand in for the course that is not your own needs to come with a citation. If you use an existing code snippet from Stack Overflow or Chat GPT you need to cite your sources.*

- **Use vector operations wherever possible**. Python is a high level language and that can introduce a lot of overhead behind the scenes -- for example, Python has to figure out the type of a variable because the type of variable (integer, float etc) is not explicitly given in Python programming. That takes time, especially in a loop. Try the following exercise to see what I mean.

```{admonition} Questions for discussion
- What are the advantages and disadvantages of using existing code over writing it yourself?
- What makes for a good comment? 
- What are best practises for code layout or variable naming?
- What other best practices have I missed?
```


````{admonition} Exercise
Create an array of $N=10^6$ angles $\theta$ equally distributed between 0 and $2\pi$. Now calculate the corresponding vector $\sin\theta$ using (1) a for loop in which you loop over the theta values and calculate $\sin\theta$ for each one, and (2) the vector operation `np.sin(theta)`. How long does it take in each case?

Paste your answer in this sheet:
https://docs.google.com/spreadsheets/d/1nDgjUhGySHeA5bnI_73m4KB_fJnwJF5tyykO4cy8P3c/edit?usp=sharing


**Hint:** To time your code, you can use 
```
import time

t0 = time.time()

# your code here

t1 = time.time()
print('That took', t1-t0, ' seconds')
```
(see also `timeit` for more sophisticated code timing).

````

**Further reading** There are many books on best programming practices and styles. One of the classics that was recently updated is

- [The pragmatic programmer: your journey to mastery](https://mcgill.on.worldcat.org/search/detail/1112609085?queryString=ti%3A%28pragmatic%20programmer%29&databaseList=283%2C638&origPageViewName=pages%2Fadvanced-search-page&clusterResults=&groupVariantRecords=&expandSearch=true&translateSearch=false&queryTranslationLanguage=&lang=en&scope=wz%3A12129) by Thomas and Hunt (2020 second edition) (the link is to the McGill library ebook).

