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

* **Period**: The amount of time it takes to make one repetition.
* **Frequency**: The amount of repetitions in a given time period, usually 1 second is the time period.
* **Hertz \(Hz\)**: The units of the sampling rate. 1Hz means 1 sample per second.
* **Phase Shift**: The shift between two similar periodic signals expressed in fractions of a period \(multiplied by 2𝛑 radians or 360°\).

# Digital Sampling {#digital-sampling}

The goal of digital sampling is to take an analog continuous-time signal, typically a voltage, and to quantize it and discretize it in time so that we can store it in a finite amount of memory and use the magic of computers to process it. The component that does this is called an**analog-to-digital converter**or an ADC, this is an example of a \*\*transducer, you will learn more about transducers in a future lesson. It is important to learn about the few fundamental ways the ADC changes the analog signal. In our head, it’s easy sometimes to pretend that we’re dealing with an ideal analog signal, but this can get us into trouble, and it’s important to know more detail about how signals are sampled to avoid pitfalls later on.

An ADC encodes a range of physical values to a set of discrete numbers. In this example, the analog signal varies over time between -3V and +3V and we are using a 4-bit ADC, which means that the ADC has 4 bits to encode the range from -3 to +3 \(ie. the**bit-depth**of our sensor is 4\). The 4 bits indicate that there are 16 discrete values and we can see the effect of this quantization in the digitized signal.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4d_nd320-c4-l1-quant-noise/nd320-c4-l1-quant-noise.png "Quantization Noise")Quantization Noise](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/26df90b7-2f41-423f-b945-b7c79e2b2b94#)

But typically, ADCs have many more bits and you won’t see quantization noise because it will be overpowered by other noise sources.

* Latent thermal energy in the system.
* Electronic noise from within the sensor.
* Electronic noise from the surroundings and the building itself. All these types of noise contribute to what we call the
  **noise floor**
  . Even when the incoming signal is perfectly flat, you will see some noise in the output. If you ever see a flat line at 0 in the output, it’s because your sensor is broken.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4c_nd320-c4-l1-noise-floor/nd320-c4-l1-noise-floor.png "Noise Floor")Noise Floor](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/26df90b7-2f41-423f-b945-b7c79e2b2b94#)

This noise is additive, so you’ll see it on top of whatever incoming signal you have.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4b_nd320-c4-l1-additive-noise/nd320-c4-l1-additive-noise.png "Additive Noise. When you combine the previous 2 signals, quantization and noise floor, you will get an additive signal.")Additive Noise](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/26df90b7-2f41-423f-b945-b7c79e2b2b94#)

ADCs have a fixed range on the input. So for this example, our ADC was limited to -3V and +3V. This is known as the**dynamic range**of our sensor. When the input signal exceeds the dynamic range of the sensor, in this case from -4 to +4, everything greater than 3 will be clipped and set to 3 and everything smaller than -3 will be clipped to -3. We call this effect clipping, oversaturation, or undersaturation.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4e_nd320-c4-l1-signal-clipping/nd320-c4-l1-signal-clipping.png "Signal Clipping")Signal Clipping](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/26df90b7-2f41-423f-b945-b7c79e2b2b94#)

And finally, it’s important to remember that digital signals are sampled periodically in time. We often plot them as these continuous signals by connecting the dots in between, but they are better represented as a sequence of individual points. In this example, there are 30 samples in this second, so we would say the**sampling rate**is 30 samples per second or 30 Hz.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4e_nd320-c4-l1-sampling-rate/nd320-c4-l1-sampling-rate.png "Sampling Rate")Sampling Rate](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/26df90b7-2f41-423f-b945-b7c79e2b2b94#)

A sampling rate of 125 Hz means that there are 125 samples per second. The inverse of this is 1 / 125 seconds per sample or 8 milliseconds.

## New Vocabulary {#new-vocabulary}

* **Transducer**: Part of a sensor that converts a physical phenomenon into an electrical one \(e.g., voltage\)
* **Analog-to-Digital Convert \(ADC\)**: A device \(usually embedded in the sensor\) that converts an analog voltage into an array of bits.
* **Bit depth**: The number of bits an ADC uses to create a sample. A 16-bit ADC produces a 16-bit number for each sample.
* **Noise floor**: The total amount of noise in the sensor, including electrical interference from the environment and other parts of the device, thermal noise, and quantization noise.
* **Dynamic range**: The physical range of the sensor. Values outside of this range will show up as clipping in the digital signal.
* **Sampling rate**: The frequency at which a sensor measures a signal.

## Further Research {#further-research}

* Sampling Signal Processing [Wikipedia](https://en.wikipedia.org/wiki/Sampling_%28signal_processing%29)

# Time-domain Plotting Continued {#time-domain-plotting-continued}

Plotting a signal in the**time-domain**just means that the x-axis in our plots is time. This is probably the way you naturally visualize signals. This is in contrast to the frequency domain, which we will see later in this lesson.

We also practice more complicated visualizations like plotting event detections on top of a continuous signal as well as visually comparing two similar signals.

## New Vocabulary {#new-vocabulary}

* **Time-domain**: The typical representation we are used to for signals where the signal is represented by values in time.

## Key Takeaways {#key-takeaways}

* Plotting your data is a great way to check your assumptions about the data you have.
* Matplotilb makes it easy to plot time series signals and events in time together.

## Further Resources {#further-resources}

### Physionet {#physionet}

Physionet is a great resource of freely available biomedical signals. You can try many of the techniques you learn in this class on datasets in[Physionet](https://physionet.org/). This[European ST-T Database](https://physionet.org/content/edb/1.0.0/)from Physionet was used in the previous exercise.

### Plotting {#plotting}

Listed below are the packages we will be using throughout to visualize our datasets.

* [Matplotlib](https://matplotlib.org/)- the plotting library we use most in this course.
* [Seaborn](https://seaborn.pydata.org/)- a wrapper around `matplotlib`that makes it easier to do higher level statistical visualization. We will use this a few times in the course.
* [Altair](https://altair-viz.github.io/)- Another powerful visualization library in Python
* [Plotly](https://plot.ly/)- You can use plotly to create and save visualization in HTML / javascript. This is especially useful when you want to make offline, shareable plots that you can interact with in the browser.

# Interpolation {#interpolation}

I**nterpolation **which is a technique that allows us to work with multiple signals that are sampled differently in time. We saw 2 signals that are both 1 Hz sine waves, but the one that is sampled at 60 Hz has many more data points than the one sampled at 25 Hz. After plotting and verifying the lengths of the signals, it might appear that`s2_interp`and`s1`are the same, but it is most certainly not! By plotting the original and the interpolated signal together we can see that linear interpolation estimates points in between existing points by using a weighted average of the original points.

Previously we had only discussed uniformly sampled signals where the signal is sampled at fixed intervals in time, but sometimes we may encounter signals that are sampled haphazardly in time. This is troubling because a lot of signal processing techniques that we are about to learn require that the signal is sampled uniformly. We can fix this again with linear interpolation. When we compare the 2 signals, one uniformly and one not uniformly sampled, we can see that they follow the same continuous signal but the location of those samples are at different times. Using the np.interp function you can recover the signal in which the now non-uniformly sampled signal will have a signal point like the uniform signal. But you may notice artifacts at the edge of the resampled signal, and there is more error when the gap between existing samples is larger.

**Interpolation**: A method for estimating new data points within a range of discrete known data points

**Resampling**: The process of changing the sampling rate of a discrete signal to obtain a new discrete representation of the underlying continuous signal.

## Key Takeaways {#key-takeaways}

* Deriving instantaneous heart rate from R peak locations
* Using interpolation to normalize a non-uniformly sampled signal
* Using interpolation to align estimates with reference data streams

## Further Resources {#further-resources}

* [Interpolation](https://en.wikipedia.org/wiki/Interpolation)
* [Linear Interpolation](https://en.wikipedia.org/wiki/Linear_interpolation)

# Fourier Transform {#fourier-transform}

The theory of fourier transform is that any signal can be represented as a sum of sinusoids. Then we saw how this theory can be put into action by recreating a real accelerometer signal using only the addition of sinusoids. The frequency of the specific sinusoids that make up a signal can tell us important information that we can use to build algorithms to process that signal.

e Fourier transform allows us to describe any signal as a summation of sinusoids. The frequencies of the sinusoids that comprise a signal represent the signal’s **frequency components**. The range of frequency components for a signal is called its **bandwidth**.

We then discuss the **Nyquist frequency **and the limits this imposes on the sampling rate and the **bandwidth **of the signals that we sample.

If we try to sample a signal that has higher frequency components than the Nyquist frequency, we will see **aliasing**, which means those high-frequency components will show up at mirrored lower frequencies.

* **Frequency component**: The Fourier transform explains a signal as a sum of sinusoids. Each of these sinusoids is a frequency component of the signal.

* **Nyquist frequency**: Half of the sampling frequency. Signal components above this frequency will get aliased in the sampled signal.
* **Bandwidth**: A range of frequencies within a band.
* **Aliasing**: The effect that causes frequency components greater than the Nyquist frequency to become indistinguishable from frequencies below the Nyquist frequency.



