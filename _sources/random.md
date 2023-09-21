# Random numbers

Random numbers are useful in many applications in computational physics. An obvious one is to simulate noise or stochastic systems, but they also allow efficient sampling of functions for integration or for parameter searches. 

Although there are sources of true randomness (so-called entropy sources) in operating systems, e.g. linked to thermal noise in hardware components, in general you will be dealing with **pseudo random number generators** which are algorithms that produce deterministic sequences of numbers that have the statistical properties of random numbers.

An important concept is the random number **seed**. This provides a starting point for the random sequence, and enables the random sequence to be reproduced, i.e. each seed gives rise to a certain sequence of random numbers. This is important if you want to be able to reproduce results exactly based on random numbers. If you don't specify a seed, the system will generate one, but the exact sequence of numbers will be different. This may or may not be important, e.g. if you are interested only in the statistical properties. It can certainly be good to try different seeds and make sure your results don't depend on choice of seed.

 



