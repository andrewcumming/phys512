# Signal processing

We've focused on solving differential equations with Fourier methods, but of course a main application of Fourier techniques is in signal processing. A topical example of this is detecting black hole inspirals in gravitational wave data. 

```{admonition} Exercise: gravitational wave inspirals

Work through the three parts of the LIGO tutorial [Find an inspiral](https://gwosc.org/tutorial06/). Try to understand as much as you can about the different aspects of the analysis. You may need to `pip install h5py` to be able to read in the data files.

Here are some questions to answer as you work through it:

- why does the template have that shape? In particular, they say "The template has been tapered at the beginning and end, so that it can be more easliy Fourier transformed." what is meant by that?

- they are using `mlab.psd` from matplotlib to calculate the PSD (and ASD). What is `mlab.psd` doing? What determines the range of frequencies it evaluates?

- how is the cross-correlation defined? What do you think is happening inside the function `np.correlate`?  Why is there an offset between the results for the bandpassed data and the raw data?

- in the optimal matched filter section, try plotting the estimated noise power spectrum on top of the power spectrum of the full data. Do you see evidence of the signal?

- what is happening in the two lines below the comment ``# -- Calculate the matched filter output`` that calculate `optimal` and `optimal_time`?

[[Solution]](https://andrewcumming.github.io/phys512/LIGO.html)
```
