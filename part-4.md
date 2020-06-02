## Wearable Devices {#wearable-devices}

Today the landscape of wearable devices is vast and only increasing. In part because of the miniaturization of electronics and sensors, they are much more capable than before. For example, the Apple Watch came out in 2015 and tried to track activity, heart rate, exercise, sleep. The newer model even has an ECG sensor on it for monitoring your heart rhythm. There are shoe sensors, “underwearables” - things like compression shorts, and even wearables for your baby. Pampers has a smart diaper.

But these sensors don’t produce meaningful metrics automatically. For example, the sensors on a FitBit don’t produce a pulse rate or a step count. They sense some underlying signal, for step count, it’d be motion. Then, complex algorithms will process the raw motion data into various activity classifications or step counts.

## What You Will Learn {#what-you-will-learn}

This course will teach you how to build signal processing and machine learning algorithms to process raw wearable data and produce clinically meaningful metrics. We’ll talk about the healthcare aims that can be achieved with wearables. The skills you'll develop and the algorithms you’ll learn in this class will be transferable to a broad spectrum of devices and sensors.

The course focuses on three main sensors for a wrist-wearable:

* the IMU sensor measures motion.
* the PPG sensor measures blood flow at the wrist.
* the ECG sensor measures the electrical activity of the heart.

We’ll begin with an introduction to how these sensors work. Then we will deep-dive into the IMU sensor and learn how to use it to do activity classification. This can help build useful context around how the wearer spends their time, how much exercise they get, how mobile they are, and so on. Next, we’ll look at the ECG sensor and learn how we can use it to detect some cardiovascular conditions. Finally, in the final project, you will use the PPG sensor to derive the pulse rate. And learn techniques to produce good results even while the wearer is exercising, which as you’ll soon learn causes all sorts of problems. You will be working intimately with time-series signals in this course, so before we jump in, we’ll do a quick review of basic time-series signal processing and learn some plotting tools to help us explore the data.

## Key Topics {#key-topics}

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4a_nd320-c4-l0-key-topics/nd320-c4-l0-key-topics.png "Course Overview")Key Topics in this Course](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/4417cb06-0a1d-4dad-9418-2e615308b2f6/concepts/f3e3a7b9-ae01-4cff-a970-969c5894f727#)

## Course Outline {#course-outline}

Lesson 1 - Introduction to Wearable Data

* Welcome / Introduction
* Wearables in a Medical Context

Lesson 2 - Introduction to Signal Processing

* Sampling
* Python plotting in time-domain
* Resampling / Interpolation
* Fourier Transform
* Python plotting in the frequency domain
* Harmonics

Lesson 3 - Introduction to Wearable Sensors

* Wearable Demo
* IMU Sensor
* Accelerometer deep-dive
* PPG Sensor
* ECG Sensor

Lesson 4 - Activity Classifier

* Introduction to Activity Classifiers
* Feature Extraction
* Model Building
* Hyperparameter Tuning

Lesson 5 - ECG Processing

* About the ECG signal
* Pan-Tompkins algorithm
* Atrial Fibrillation detection algorithm

Final Project

* Motion Compensated Pulse Rate
* Resting Heart Rate vs. Age and Sex in a Disease Population

## Apple Heart Study Overview {#apple-heart-study-overview}

If wearables are ever to be used in a medical context, research and clinical trials need to be done with wearable devices. We’re going to look into one such study, the Apple Heart Study \(AHS\). Apple, with Stanford University, recruited 419,000 individuals over 8 months to participate over the internet. Participants just had to download an app over the App Store, provided they already owned an Apple Watch and an iPhone. The goal of the study was to try to detect an abnormal heart rhythm called Atrial Fibrillation \(AF\). AF is one of the most common arrhythmias affecting up to 6 million people in the US and it’s significant because it’s associated with a 4 - 5x increase in the risk of stroke.

## Study Procedure {#study-procedure}

The way the AHS worked was the watch would sometimes notify participants of an irregular pulse rhythm. Once notified, the researchers would send these participants a wearable ECG chest patch for 7 days that would confirm whether they were experiencing AF. 34% of the notified participants in this study were confirmed to have AF from the chest patch. AF can be intermittent, so even if someone has it, there’s no guarantee it would show up during the chest patch period.

