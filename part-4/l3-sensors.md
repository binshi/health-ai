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



