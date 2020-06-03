Sensors are how we measure physical quantities in the world around us. The name of the game for building any kind of sensor is to convert a physical phenomenon into an electrical one - be it a current or voltage - which can then be digitized and stored on a computer. The component that does this is called the transducer.

This is easy to see in this example of how a microphone works. A microphone is basically a drum attached to a coil of wire around a magnet. The drum moves in response to vibrations in the air or sound. As you may have learned in your physics class when the coil of wire moves back and forth over the magnet, the changing magnetic field induces a current in the wire. The A/C current creates an A/C voltage in the circuit that is then measured using an ADC.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d43_nd320-c4-l2-microphone/nd320-c4-l2-microphone.gif "An audio wave hits a drum which oscillates in sync with the audio wave. The drum is attached to a circuit which produces and audio signal as a sine wave. ")Microphone Diagram](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/5bc40cc9-b7e9-4fa2-bb91-b49376f945ff#)

Specifics of how a sensor works is important for truly understanding the signal. You need to know how your accelerometer works to really understand what you’re seeing when you’re plotting an accelerometer trace or when thinking about how to design your algorithm.

In this lesson, we’ll learn how the three sensors work:

* Inertial Measurement Unit \(IMU\)
* Photoplethysmogram \(PPG\)
* Electrocardiogram \(ECG\)

You’ll understand the physical quantity being measured, become familiar with what these signals look like, and learn about common challenges and noise sources that arise when working with these signals.

## Lesson Concepts {#lesson-concepts}

This lesson will describe the sensors and raw signals they collect. Sensors enable a set of algorithms that can compute derived metrics from each of these signals or multiple in combination.

\[!\[\]\([https://video.udacity-data.com/topher/2020/March/5e7a3d49\_nd320-c4-l2-sensor-and-algorithms/nd320-c4-l2-sensor-and-algorithms.png](https://video.udacity-data.com/topher/2020/March/5e7a3d49_nd320-c4-l2-sensor-and-algorithms/nd320-c4-l2-sensor-and-algorithms.png) "The 3 types of sensors \(IMU, PPG, and ECG\) covered in this lesson and the algorithms that can be derived from those sensors.

* An IMU sensor can inform an Activity Classification and Pulse Rate Estimation Algorithms. 
* A PPG sensor can inform a Pulse Rate Estimation Algorithm.
* An ECG sensor can inform a Heart Beat Detection Algorithm which can look for specific heart anomalies/phenomenon like an Arrhythmia Detection Algorithm."\)Sensor & Algorithms\]\([https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/5bc40cc9-b7e9-4fa2-bb91-b49376f945ff\#](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/5bc40cc9-b7e9-4fa2-bb91-b49376f945ff#)\)

### Outline {#outline}

* Wrist Wearable Demo
* IMU Sensor
  * IMU Overview
  * Accelerometer Deep Dive
* PPG Sensor
* ECG Sensor

# Inertial Measurement Unit \(IMU\) {#inertial-measurement-unit-imu-}

The**inertial measurement unit \(IMU\)**is an umbrella term for three specific sensors that describe motion, namely the accelerometer, gyroscope, and magnetometer.

* The **accelerometer **measures linear acceleration.
* The **gyroscope **measures angular velocity
* The **magnetometer **measures absolute orientation.

While an accelerometer might tell you that the device is moving to the left really fast, it won’t tell you which way left actually is. The magnetometer will tell you that moving to the left means going East. Each of these sensors has 3 channels of measurements, each in a perpendicular direction in 3D space and would be labeled x, y, and z. This would be like measuring the acceleration up and down, left and right, and forward and backward. Device manufacturers can orient their accelerometers however they want, so we can’t assume that the z-direction is the vertical direction.

We won’t be dealing with gyroscopes and magnetometers in this course, but we will look at accelerometers. The animation below is a model of an accelerometer. There is a mass attached to springs and the mass moves a plate between two capacitors inside a circuit. This changes the capacitance depending on the location of the plate. Because the displacement of the mass on a spring is proportional to the force it sees, the changing voltage in the circuit is proportional to force and acceleration. If you combine three of these circuits together perpendicularly, you can measure acceleration in 3 dimensions.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d22_nd320-c4-l2-accelorometer/nd320-c4-l2-accelorometer.gif "Accelerometer Diagram")Accelerometer Diagram](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/03257003-8531-4961-b6d2-6707be4d6beb#)

This also means that accelerometers are affected by gravity. For example, if we rotate our accelerometer 90 degrees, we can see gravity pulls the mass down and the accelerometer will see a voltage associated with 1**g-force **or the magnitude of the acceleration due to Earth’s gravity. The accelerometer only measures 0 if it’s in free-fall.

This also means that when the device is stationary, you can measure its orientation. If all the acceleration is in the downward z-direction, then it’s flat on a table. Or if it’s all in the x-direction, then it’s tilted on its side. We’ll take a more in-depth look at this another phenomena in the next lesson where we look at accelerometer traces in detail.

Not all accelerometers are implemented using this capacitor attached to a mass on a spring model, but they follow similar principles. For example, some accelerometers use a force sensitive resistor or a [piezoelectric crystal](https://blog.endaq.com/piezoelectric-accelerometers-how-they-work-and-where-to-buy) to modulate a voltage in response to an acceleration.

* **Inertial Measurement Unit \(IMU\)**: A collection of sensors that measure motion.

* **Accelerometer**: A sensor that measures linear acceleration.

* **Gyroscope**: A sensor that measures angular velocity.

* **Magnetometer**: A sensor that measures magnetic forces.

* **g-force**: The amount of acceleration on a body measured in units of acceleration due to gravity on earth \(or roughly 9.8m/s^2\).

# Accelerometer Deep-Dive {#accelerometer-deep-dive}

Let’s take a more in-depth look at the accelerometer. As a reminder, when we talk about accelerometer magnitude, this means the vector magnitude of the force on the accelerometer in 3D space, as given by this formula.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d1a_nd320-c4-l2-accelerometer-magnitude/nd320-c4-l2-accelerometer-magnitude.png "The Accelerometer Magnitude equals the square root of the sum of the squared of movement in x, y, and z direction. ")Accelerometer magnitude is the square root of the sum of the squared value of movement in each of the 3 axes.](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/917364ba-762e-4f05-bb12-82e16c2092a3#)

Take a look at this accelerometer trace when the wearer is at rest.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d18_nd320-c4-l2-acc-rest/nd320-c4-l2-acc-rest.png "Accelerometer Magnitude - Wearer at Rest")Accelerometer Magnitude - Wearer at Rest](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/917364ba-762e-4f05-bb12-82e16c2092a3#)

The accelerometer magnitude is close to 1 g, which means the total force on the accelerometer is only due to gravity. But if you look at the individual channels, they shift around. In the beginning, they all see the same amount of gravitational force, which means the device is tilted along each axis equally. But then after 15 seconds, the x and y channels drop and the z-channel rises to nearly 1g. This indicates the device has moved and is now lying flat, so only the z-channel feels the force of gravity. By strategically placing accelerometers on the body, you can use this technique to figure out things like posture or sleeping position.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d17_nd320-c4-l2-acc-jogging/nd320-c4-l2-acc-jogging.png "Accelerometer Magnitude - Wearer is Jogging")Accelerometer Magnitude - Wearer is Jogging](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/917364ba-762e-4f05-bb12-82e16c2092a3#)

This is a trace of someone who is jogging. Notice the very periodic shapes in each channel that indicate the cadence of the arm swing, which can be used to figure out how fast someone is walking. By analyzing the specific characteristics of the waveform, you can decompose each stride into various components.

For example, this paper decomposes the accelerometer trace from an ankle into various phases of the gait cycle.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d28_nd320-c4-l2-gait-cycle-phase/nd320-c4-l2-gait-cycle-phase.png "Gait Cycle Phases can be determined by the total acceleration. When the foot is flat, the total acceleration is 5 m/s/s. When the Heel is off the ground, small increases will occur, but with toe-off, the total acceleration will peak near to 20 m/s/s. The swing of the foot phase will be variable around 5 m/s/s. But as the foot strikes the ground, it will max out around 25 m/s/s and have some smaller spikes from 10 - 15 m/s/s as the foot is flat on the ground at 5 m/s/s.")Patterson M., Caulfield B. A novel approach for assessing gait using foot mounted accelerometers; Proceedings of 5th International ICST Conference on Pervasive Computing Technologies for Healthcare; Dublin, Ireland. 23–26 May 2011; pp. 218–221.](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/917364ba-762e-4f05-bb12-82e16c2092a3#)

Discriminating the gait phases like this is an important starting point for several applications such as recovery after an injury or treatment, the classification of daily activities, coaching athletes, and distinguishing between normal and pathological gait. Researchers have found that differences in each person’s gait mean that it can be used to identify individuals.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d1a_nd320-c4-l2-acc-run-break/nd320-c4-l2-acc-run-break.png "Accelerometer Magnitude - Wearer is running with a break in between")Accelerometer Magnitude - Wearer is running with a break in between](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/917364ba-762e-4f05-bb12-82e16c2092a3#)

Here we see a zoomed-out view of the accelerometer trace over 2 minutes. We can see the wearer distinctly changing activities, going from jogging to still to jogging again. You can even tell that they are probably jogging on a treadmill as the speed slowly ramps up and down. And from the sharp cut-off around 50 seconds, maybe we can tell that they jumped off the treadmill or for some reason their treadmill session was cut short. I didn’t collect this dataset, so we can’t know for sure, but it’s interesting that we can learn this level of detail from just an accelerometer alone.

## Further Resources {#further-resources}

This paper describes using the orientation of accelerometers on the body to determine posture.[Gjoreski, Hristijan & Gams, Matjaz. \(2011\). Activity/Posture Recognition using Wearable Sensors Placed on Different Body Locations. 10.2316/P.2011.716-067.](https://pdfs.semanticscholar.org/b8ac/6f5f1a3362f83aef7c75b0b75ab09e17a3c1.pdf).

A review of methods for segmenting the gait cycle.  
[Taborri J, Palermo E, Rossi S, Cappa P. Gait Partitioning Methods: A Systematic Review. Sensors \(Basel\). 2016 Jan 6;16\(1\). doi: 10.3390/s16010066. Review. PubMed PMID: 26751449; PubMed Central PMCID: PMC4732099](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4732099/)

Gait cycle segmentation using an ankle based accelerometer.  
[Patterson, Matt & Caulfield, Brian. \(2011\). A novel approach for assessing gait using foot mounted accelerometers. 2011 5th International Conference on Pervasive Computing Technologies for Healthcare and Workshops, PervasiveHealth 2011. 218 - 221. 10.4108/icst.pervasivehealth.2011.246061.](https://eudl.eu/pdf/10.4108/icst.pervasivehealth.2011.246061).

An article describing how changes in the gait cycle can be learned behavior. In this case, from prior KGB training.  
[Araújo R, Ferreirai JJ, Antonini A, and Bloem B. \(2015\) “Gunslinger’s gait”: a new cause of unilaterally reduced arm swing. The BMJ.](https://www.bmj.com/content/351/bmj.h6141)

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d29_nd320-c4-l2-gait-phases/nd320-c4-l2-gait-phases.png "A chart showing a gait separated from two to eight phases. And as we increase in the number of phases the breakdown of each of the phases increases. This shows how we can break down a gait and thus how we could recognize a gait in our data. ")Source:  
[Taborri J, Palermo E, Rossi S, Cappa P. Gait Partitioning Methods: A Systematic Review. Sensors \(Basel\). 2016 Jan 6;16\(1\). doi: 10.3390/s16010066. Review. PubMed PMID: 26751449; PubMed Central PMCID: PMC4732099](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4732099/)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/a7c67df0-64b2-4c1e-8504-03e279727580#)

# Photoplethysmogram \(PPG\) Sensor {#photoplethysmogram-ppg-sensor}

The**photoplethysmogram \(PPG\)**optically measures blood flow at the wrist. The LEDs in a PPG sensor shine a typically green light into your skin and your red blood cells absorb that green light. The reflected light is then measured by the**photodetector**. When your heart beats and blood perfuses through the wrist, there are more red blood cells that absorb the green light and the photodetector sees a smaller signal. As the heart fills back up with blood and blood leaves your wrist, the more green light is reflected back and the photodetector reading goes up. This oscillating waveform can be used to detect pulse rate. See the illustration below.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d46_nd320-c4-l2-ppg-sensor-diagram/nd320-c4-l2-ppg-sensor-diagram.png "PPG Sensor Diagram")PPG Sensor Diagram](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/7ab1b6d5-a4e8-4c29-b36d-e3411c174959#)

The trough corresponds to a peak in perfusion and occurs when the ventricles of the heart contract, called**systole**. The peak in the waveform occurs when there is the least amount of blood in the wrist or when the heart relaxes and fills with blood, called**diastole**.

## Noise Sources {#noise-sources}

There are many other factors that affect the signal.

### Melanin {#melanin}

Skin absorbs light from the LEDs as well and darker skin absorbs more light. This causes a DC shift and a reduction in SNR in the PPG signal for people with darker skin.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d2a_nd320-c4-l2-melanin/nd320-c4-l2-melanin.png "Effect of Melanin")Effect of Melanin](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/7ab1b6d5-a4e8-4c29-b36d-e3411c174959#)

### Arm Motion {#arm-motion}

Blood is a liquid and when you move your arm around, the blood inside moves as well. This is what the PPG signal looks like while running. You can see the cadence of the arm swing in the FFT of the PPG signal as well as in the accelerometer.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d24_nd320-c4-l2-arm-motion/nd320-c4-l2-arm-motion.png "Effect of Arm Motion")Effect of Arm Motion](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/7ab1b6d5-a4e8-4c29-b36d-e3411c174959#)

# Arm Position {#arm-position}

Even while at rest, if the position of the arm changes, blood flows into or out of the wrist. If you hang your arm down, more blood will flow into it and the DC level of the signal will slowly drop. If you raise your arm up, blood will flow out of your arm and the DC level will increase.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d45_nd320-c4-l2-posture/nd320-c4-l2-posture.png "Effect of Posture Change")Effect of Posture Change](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/7ab1b6d5-a4e8-4c29-b36d-e3411c174959#)

### Finger Movement {#finger-movement}

Moving your fingers around will cause a significant disturbance in the PPG signal because, as the tendons in your wrist move, they cause other structures to move around and may even shift the position of the sensor. All this motion will change the path that light takes to travel through the wrist and change the photodetector reading.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d27_nd320-c4-l2-finger-movement/nd320-c4-l2-finger-movement.png "Effect of Finger Movement")Effect of Finger Movement](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/7ab1b6d5-a4e8-4c29-b36d-e3411c174959#)

All these sources of noise make the PPG signal tricky to deal with, but now that you are aware of them, you can design algorithms that are robust to these events.

* **Photoplethysmogram \(PPG\)**: the optical sensor used to measure pulse rate on a wearable device.

* **Photodetector**: A sensor that measures light.

* **Diastole**: The phase of the cardiac cycle where the heart relaxes and fills with blood.

* **Systole**: The phase of the cardiac cycle where the ventricles contract and pump blood through the arteries.

## Signal Quality Evaluation {#signal-quality-evaluation}

The single most important factor that can make or break any algorithm you build is the underlying quality of the signal. As an algorithms engineer you may have the rare opportunity to influence the design of the hardware that will acquire the signal that will then be the input to your algorithms. Being able to provide actionable quantitative feedback for various hardware prototypes will provide endless returns down the line when you start building algorithms based on that hardware.

We can see an example of this kind of analysis in[this paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6651860/)where the researchers analyzed the effects of different mediums \(water vs. air\) and temperature on the PPG signal quality and pulse rate estimation from a smartphone sensor.

First let’s look at the pulse rate algorithm accuracy.

[![](https://video.udacity-data.com/topher/2020/May/5eb8ee6b_l2-ppg-snr-1/l2-ppg-snr-1.png "Pulse rate estimation accuracy vs. temperature and medium")Pulse rate accuracy acquired from smartphone PPG signals for varying temperature in dry \(red\) and underwater \(blue\) environments.](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/3e41ac7a-3ff9-42f7-8594-e15376d834b9#)

Pulse rate accuracy deteriorates at cooler temperatures and is worse underwater compared to in air.

Using a pulse rate algorithm’s accuracy is a great way to evaluate signal quality, especially if the primary thing you want to do with that signal is estimate pulse rate and if you have the algorithm on hand. However, using an algorithm to estimate signal quality can conflate the algorithm accuracy and its idiosyncrasies with the signal quality. Sometimes you may want to look directly at the signal characteristics themselves. Here, the researchers chose to look at signal amplitude.

[![](https://video.udacity-data.com/topher/2020/May/5eb8ee6b_l2-ppg-snr-2/l2-ppg-snr-2.png "PPG signal amplitude vs. temperature and medium")Smartphone PPG signal amplitude for varying temperature in dry \(red\) and underwater \(blue\) environments.](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/3e41ac7a-3ff9-42f7-8594-e15376d834b9#)

Again we observe that we get higher amplitudes in dry and warmer environments. This plot implies that a higher signal amplitude corresponds to higher signal quality. But as we have seen previously, with PPG signals, this is not always the case. Motion artifact and ambient light can cause high amplitude peaks in the signal that definitely do not correspond to signal quality. Optimizing for signal amplitude might mean that you build hardware that is extremely susceptible to motion artifacts.

In the following exercise you will use a more holistic metric to evaluate PPG signal quality called the signal-to-noise ratio that avoids some of these problems.

## Further Resources {#further-resources}

* To learn more about the PPGs, you can look at this[Wikipedia Entry](https://en.wikipedia.org/wiki/Photoplethysmogram).

* A dissertation on PPG processing techniques that includes an introduction that has great background information on the PPG signal in general.[Schäck, Tim. Photoplethysmography-Based Biomedical Signal Processing Darmstadt, Technische Universität Darmstadt, Jahr der Veröffentlichung der Dissertation auf TU prints: 2019 Tag der mündlichen Prüfung: 21.01.2019.](https://pdfs.semanticscholar.org/c1d1/318d4fa8c8c02b1eaae2ca50c16532b53e15.pdf).

# Electrocardiogram \(ECG\) Sensor {#electrocardiogram-ecg-sensor}

**Electrocardiograms \(ECG or EKG\)**measure the voltage across the chest, which is primarily due to the heart rhythm. ECGs are different from the other raw signals produced by a wearable in that cardiologists can use them in their raw, unprocessed form to diagnose heart problems like a previous heart attack or an irregular heart rhythm.

The ECG measures the voltage created by the electrical activity in the heart. A traditional ECG does this with electrodes on the chest. An**electrode**is a conductive pad that comes in contact with skin and a**lead**is the electrical potential difference between two electrodes.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d25_nd320-c4-l2-clinical-ecg/nd320-c4-l2-clinical-ecg.jpg "Clinic ECG")Clinic ECG](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/b8d60b91-ae4b-488b-8379-99e6fe33f2b5#)

Each pair of electrodes measures a slightly different voltage. The standard 12-lead ECG contains 12 different voltages across different axes of the body.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d15_nd320-c4-l2-12-lead-ecg/nd320-c4-l2-12-lead-ecg.png "12 Lead ECG")12 Lead ECG](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/d7a821f4-d64c-402e-80b7-b293656119a8/concepts/b8d60b91-ae4b-488b-8379-99e6fe33f2b5#)

The ECG has been “somewhat” wearable for a while.**Holter monitors**are ECG machines that are connected to a person’s chest in much the same way a tabletop machine is, but the recording device is portable and attached to the wearer’s hip. Holter monitors are worn during stress tests after a patient has been diagnosed or is suspected of having a heart condition or for short-term monitoring. They’re typically not worn for more than a few days.

Beyond that, there are chest patches that only measure 1-lead of the 12 lead ECG. These chest patches can last from a few days to a couple of weeks. In 2011, Alivecor started making a handheld device that paired with an iPhone to take an ECG. Apple Watch and Verily’s Study Watch both have ECG sensors embedded into the hardware. On a wristwatch, the two electrodes are moved to the wrist and fingers on opposing hands. Because of this, the ECG cannot be monitored continuously and passively like the chest patch or Holter monitor or other sensors on a wearable. However, a wrist-watch can be worn for years, while a chest patch only lasts for a few weeks.



