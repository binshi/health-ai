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

# Fourier Transform In Practice {#fourier-transform-in-practice}

Let's make a signal that is composed of two sine waves at 2 Hz and 3 Hz plus some random noise. We will be using the`np.fft`module to compute the Fourier transform with the two main functions below:

* `rfft`- computes the actual Fourier transform coefficients
* `rfftfreq`- tells us the frequencies for which we are computing the Fourier transform

We then examined`freqs`to see that the FFT samples the Fourier transform uniformly from 0 Hz to the Nyquist frequency, which in this case is 25 Hz because our sampling rate is 50 Hz. We also saw the Fourier transform coefficients and only examined the magnitudes. Plotting the FFT, we see that the signal is composed primarily of two frequencies \(2 and 3Hz\) and a little bit of everything else \(from the random noise\).

We also saw how zero-padding could be used to visualize sinusoids with frequencies that are not present in`freqs`. To visualize at frequencies not in`freqs`, we need to sample twice as often and we can do this by adding 0s to the end of the signal. However, we also see this rippling, which is an artifact of padding the signal with 0s, which is the trade-off when doing zero-padding.

**Inverse Fourier Transform** in which we used the`np.fft`module to compute the inverse Fourier transform with the function`irfft`.

We started with a noisy signal and removed all the frequency components not in the range, in this case, 2Hz and 3Hz. And we saw a recovered signal that looked very close to the signal we saw in the previous video.

But we did the process again from 2.15Hz and 2.95Hz. The recovered signal looked a bit distorted and not what we'd expect. This is because zeroing out Fourier coefficients is not the best way to filter a signal.

We then used`scipy`to**bandpass filter**our signal for us. A bandpass filter will remove all frequency components outside of a given passband. Let's bandpass filter our signal with a passband from 1 Hz to 4 Hz. This way, our desired frequencies of 2.15 Hz and 2.95 Hz are well within the passband. And now, our recovered signal looks very similar to what we want.

# Fourier Transform In Review {#fourier-transform-in-review}

In this class we will be computing the Fourier transform by using a method called the[Fast Fourier Transform](https://en.wikipedia.org/wiki/Fast_Fourier_transform). This is a clever algorithm that is able to compute the Fourier transform in O\(n\*log\(n\)\) time instead of quadratic time. We use`numpy`’s implementation of this algorithm with the functions:

* `rfft`
* `rfftfreq`
* `irfft`

We then saw how we could remove the noise by filtering out frequency components outside of the bandwidth of the signal. The process of removing frequencies from a signal outside a specific band is known as**bandpass filtering**. The band of frequencies that we want to preserve is called the**passband**. -- or**bandpass filtering**our signal. We did this first by manipulating the signal in the frequency domain. After seeing the short-falls of this method, we explored using traditional bandpass filtering techniques that process the signal in the time-domain.

## Further Resources {#further-resources}

[3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw/)is a great YouTube channel that explains mathematical concepts with beautiful animations that make intuitive understanding so much easier. He has a few videos on the Fourier transform, which are absolutely illuminating. I highly recommend exploring this channel, starting with this video,[But what is the Fourier Transform? A visual introduction](https://www.youtube.com/watch?v=spUNpyF58BY).

* **Frequency-domain**: A representation of a signal over frequency instead of time. Instead of representing the signal as a series of numbers in time, the signal is represented by the frequency components that make it up.
* **Bandpass filter**: A function that preserves frequency components of a signal within a band and suppresses the frequency components outside that band.

## Key Takeaways {#key-takeaways}

* We can use the frequency domain to learn properties of a signal in ways that would be difficult in the time domain.
* We use the Fourier transform to take a signal from the time domain to the frequency domain
* We can use `numpy`and`matplotlib`to help us compute and plot a time series signal in the frequency domain.

## Summary {#summary}

The Fourier transform plays a pivotal role in signal processing. An intuitive understanding of what it does and how to use can help accomplish many tasks when processing wearable biosignals. Fundamentally, the Fourier transform gives information about what periodic components are present in the signal. And because many biomechanical processes are periodic, \(eg. running or walking cadence, heart beats, breathing rate\) finding this periodic information in our time series signal can be incredibly useful.

In the previous few concepts we tried to impart that intuitive understanding as well as practical information on how to use the Fourier transform in Python. As you progress through the course, you will see more examples of the Fourier transform in action and your intuitive understanding will grow.

# Plotting Signals in the Frequency Domain {#plotting-signals-in-the-frequency-domain}

When we look at signals in the frequency domain, we lose information about the time-domain. Previously this hadn’t been a problem because we were looking at signals whose frequency components did not change over time. They were**stationary**. However, most signals we deal with will not be stationary. In this case, it is better to visualize the frequency components of a signal over time using either a:

* [Short-Time Fourier Transform](https://en.wikipedia.org/wiki/Short-time_Fourier_transform)
* [Spectrogram](https://en.wikipedia.org/wiki/Spectrogram)

* **Frequency component**: The Fourier transform explains a signal as a sum of sinusoids. Each of these sinusoids is a frequency component of the signal.

* **Stationarity**: A property of a signal where the statistics of a process generating a signal do not change in time. Generally, if the frequency components in a signal change in time, this signal is not stationary.

## Key Takeaways {#key-takeaways}

* We need to use the STFT or spectrogram to visualize a non-stationary signal in the frequency domain effectively.
* We can also use a spectrogram to visualize the effect of a bandpass filter on our signal.
* Again,`matplotlib`and`numpy`are our friends here.

## Further Resources {#further-resources}

Plotting a spectrogram or visualizing the short-time Fourier transform are ways of balancing the trade-off between time resolution and frequency resolution.

Surprisingly, this trade-off is related to the quantum uncertainty principle. If you would like to know more about Quantum Uncertainty Principle, you can watch 3Blue1Brown's video[The more general uncertainty principle, beyond quantum](https://www.youtube.com/watch?v=MBnnXbOM5S4).

This tutorial is a great explanation of this trade-off as well as a description of the[wavelet transform](http://users.rowan.edu/~polikar/WTpart1.html), which is another solution to this problem.

Real periodic signals are rarely sinusoidal. Still, we like to use the Fourier transform to learn about the periodicity of these signals. All periodic signals are composed of a **fundamental frequency**, which is the lowest frequency of the periodic signal, and integer multiples of this frequency called **harmonics**. In this lesson, we see this for ourselves and how the fundamental frequency relates to the signal in time-domain.

Harmonics explain why different instruments sound different despite playing the same note. Check out 12tone's video on[Why Don't All Instruments Sound The Same?](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/27ab2378-4a93-47aa-8740-dc30fc7ced40/concepts/%28https://www.youtube.com/watch?v=Q8ITu0EASL4)for a more in-depth explanation. You can also watch 3Blue1Brown's video[Music And Measure Theory](https://www.youtube.com/watch?v=cyW5z-M2yzw)explaining why certain notes sound good together by looking at multiples of their frequencies.

## New Vocabulary {#new-vocabulary}

* **Harmonics**: the fundamental frequency and integer multiples of the fundamental frequency of periodic signals.

## Summary {#summary}

We covered a lot of material in this lesson. We started with the basics of what a signal is and how to digitally sample one. Then we covered techniques to process digital signals, including interpolation, the Fourier Transform, and harmonics. Along the way, we learned how to visualize signals using matplotlib, and how the spectrogram or STFT can help us more accurately see the Fourier coefficients of a signal as they change in time. The breadth of concepts that we went over could comprise full university classes. We did a lot of exercises to help you feel comfortable using these topics practically, but a lot of this material might be unlike other disciplines of math that you’ve seen before. Don’t hesitate to rewatch the videos and explore the further resources to build your intuition. And I’m hopeful that as we use these concepts in the upcoming lessons, things will start to click more.

## Further Resources {#further-resources}

### Physionet {#physionet}

Physionet is a great resource of freely available biomedical signals. You can try many of the techniques you learn in this class on datasets in[Physionet](https://physionet.org/). This[European ST-T Database](https://physionet.org/content/edb/1.0.0/)from Physionet was used in the previous exercise.

### Plotting {#plotting}

* [Matplotlib](https://matplotlib.org/)
  * the plotting library we use most in this course.
* [Seaborn](https://seaborn.pydata.org/)
  * a wrapper around
    `matplotlib`
    that makes it easier to do higher-level statistical visualization. We will use this a few times in the course.
* [Altair](https://altair-viz.github.io/)
  * Another powerful visualization library in Python
* [Plotly](https://plot.ly/)
  * You can use
    `plotly`
    to create and save visualization in HTML / javascript. This is especially useful when you want to make offline, shareable plots that you can interact with in the browser.

### Interpolation {#interpolation}

* [Interpolation](https://en.wikipedia.org/wiki/Interpolation)
* [Linear Interpolation](https://en.wikipedia.org/wiki/Linear_interpolation)

### Fourier Transform {#fourier-transform}

[3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw/featured)is a great YouTube channel that explains mathematical concepts with beautiful animations that make intuitive understanding so much easier. He has a few videos on the Fourier transform, which are absolutely illuminating. I highly recommend exploring this channel, starting with[this video](https://www.youtube.com/watch?v=spUNpyF58BY).

### Spectrograms {#spectrograms}

Plotting a spectrogram or visualizing the short-time Fourier transform are ways of balancing the trade-off between time resolution and frequency resolution. Surprisingly, this trade-off is related to the quantum uncertainty principle \(see[this 3Blue1Brown video](https://www.youtube.com/watch?v=MBnnXbOM5S4)\). This tutorial is a great explanation of this trade-off as well as a description of the[wavelet transform](http://users.rowan.edu/~polikar/WTpart1.html), which is another solution to this problem.

### Harmonics {#harmonics}

Harmonics explain why different instruments sound different despite playing the same note. Check out[this video](https://www.youtube.com/watch?v=Q8ITu0EASL4)from YouTube channel[12tone](https://www.youtube.com/channel/UCTUtqcDkzw7bisadh6AOx5w)for an explanation.

[3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw)also has[a video](https://www.youtube.com/watch?v=cyW5z-M2yzw)explaining why certain notes sound good together by looking at multiples of their frequencies.

## Glossary {#glossary}

* **Transducer**: Part of a sensor that converts a physical phenomenon into an electrical one \(e.g., voltage\)
* **Analog-to-Digital Convert \(ADC\)**: A device \(usually embedded in the sensor\) that converts an analog voltage into an array of bits.
* **Bit depth**: The number of bits an ADC uses to create a sample. A 16-bit ADC produces a 16-bit number for each sample.
* **Noise floor**: The total amount of noise in the sensor, including electrical interference from the environment and other parts of the device, thermal noise, and quantization noise.
* **Dynamic range**: The physical range of the sensor. Values outside of this range will show up as clipping in the digital signal.
* **Sampling rate**: The frequency at which a sensor measures a signal.
* **Hz**: The units of the sampling rate. 1Hz means 1 sample per second.
* **Nyquist frequency**: Half of the sampling frequency. Signal components above this frequency will get aliased in the sampled signal.
* **Frequency component**: The Fourier transform explains a signal as a sum of sinusoids. Each of these sinusoids is a frequency component of the signal.
* **Aliasing**: The effect that causes frequency components greater than the Nyquist frequency to become indistinguishable from frequencies below the Nyquist frequency.
* **Bandwidth**: A range of frequencies within a band.
* **Interpolation**
  : A method for estimating new data points within a range of discrete known data points.
* **Resampling**: The process of changing the sampling rate of a discrete signal to obtain a new discrete representation of the underlying continuous signal.
* **Frequency domain**: A representation of a signal over frequency instead of time. Instead of representing the signal as a series of numbers in time, the signal is represented by the frequency components that make it up.
* **Time-domain**: The typical representation we are used to for signals where the signal is represented by values in time.
* **Bandpass filter**: A function that preserves frequency components of a signal within a band and suppresses the frequency components outside that band.
* **Passband**: The band of a bandpass filter where frequency components will be preserved.
* **Stationarity**: A property of a signal where the statistics of a process generating a signal do not change in time. Generally, if the frequency components in a signal change in time, this signal is not stationary.



