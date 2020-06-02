[![](https://video.udacity-data.com/topher/2020/May/5eb8ee68_l1-diagram-of-overarching-conceptsv2/l1-diagram-of-overarching-conceptsv2.png "Signal Processing for this course will cover Sampling, Fourier Transform, Interpolation, and Harmonics. ")We will be covering four major areas of signal processing theory in this lesson.](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/c7d2f97a-2f59-41c7-8933-faab2a3c6b7d#)

## Summary {#summary}

This lesson will be a whirlwind tour through sampling theory, signal processing, the Fourier transform, and related topics that you will need to know to complete this course. Along the way, we will learn how to plot and visualize our signals using basic python plotting functions. We will be using the following packages:

* [numpy](https://numpy.org/)
* [scipy](https://www.scipy.org/)
* [matplotlib](https://matplotlib.org/)
* [scikit-learn](https://scikit-learn.org/stable/)

If you are familiar with these libraries and concepts already, then this will be mostly a review. If this is all brand new to you, don’t worry, this lesson assumes zero prior knowledge and doesn’t overwhelm you with the theory. In fact, you won’t see a single equation in this lesson. You will, however, see a lot of code.



A signal is simply a series of numbers \(e.g. \[3, 4, 6, 2, 4\] is a signal!\). Typically these numbers represent a voltage that changes with respect to some physical phenomenon. When the voltage signal changes in time, it’s called a time series signal. Signal processing helps us analyze this sequence of numbers so we can learn more about the physical phenomenon it represents.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4f_nd320-c4-l1-sinusoid-basics/nd320-c4-l1-sinusoid-basics.png "Sinusoid Basics")Sinusoid Basics](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/e1e88d78-b8f6-4742-909c-50da547c5566#)

We can use the figure above to explore some properties of signals. The time-invariant part of the signal is the DC level or mean value \(black line\), here it’s at 10 Volts. The AC component \(blue line\) is the part of the signal that varies with time. There are many ways to measure the AC component -- e.g., standard deviation, variance, or interquartile range. In this case, we measured the peak-to-peak amplitude, which is 6 volts.

This signal is also periodic, meaning that it repeats itself over and over again. The**period**is the amount of time it takes to make one repetition, which in this case is half a second. The**frequency**is the inverse of the period, or the number of repetitions per second, which is 2**hertz \(Hz\)**.

[![](https://video.udacity-data.com/topher/2020/March/5e81dbec_phase-shift/phase-shift.png "TODO")Phase Change](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/e1e88d78-b8f6-4742-909c-50da547c5566#)

The last property of a periodic signal is the**phase shift**, which is similar to a time shift. If two signals are time shifted by one full period, there is no difference between them. For this reason, we express this shift by the fraction of the period that they are shifted. If a signal is shifted by a quarter of a period, we can say the phase shift is 90 degrees \(360 being a full period\). You can also measure phase shift in radians and there are 2 pi radians in a full period. So this phase shift would be half pi radians.

## New Vocabulary {#new-vocabulary}

* **Period**
  : The amount of time it takes to make one repetition.
* **Frequency**
  : The amount of repetitions in a given time period, usually 1 second is the time period.
* **Hertz \(Hz\)**
  : The units of the sampling rate. 1Hz means 1 sample per second.
* **Phase Shift**
  : The shift between two similar periodic signals expressed in fractions of a period \(multiplied by 2𝛑 radians or 360°\).



