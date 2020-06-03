In this lesson, you will build an algorithm that processes the ECG signal to find the heartbeats. You will then build an algorithm that takes these heartbeat locations and detects atrial fibrillation. We will learn about the physiology of the heart during normal function as well as during atrial fibrillation. This will help us build better features for our algorithms.

## Lesson Outline {#lesson-outline}

* Heart Physiology
* QRS Complex Detection
  * Pan-Tompkins Algorithm
  * Extending Pan-Tompkins
* Atrial Fibrillation Physiology
* Arrhythmia Detection
  * Computing in Cardiology Challenge 2017
  * Data Exploration
  * Feature Extraction
  * Modelling

## Lesson Concepts {#lesson-concepts}

Below is a diagram describing the concepts that we will cover in this lesson. Specifically, we will build a system where two algorithms are cascaded together. Each algorithm is capable of producing clinically meaningful results, but the QRS complex detection algorithm’s output is fed into the arrhythmia detector.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d14_nd320-c4-l4-lesson-concepts/nd320-c4-l4-lesson-concepts.png "Lesson Concepts")Lesson Concepts](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/048a9529-ee90-4e0f-bf00-413bb3e18983/concepts/3a10a13f-d928-433a-a654-6b54a821c9f1#)



