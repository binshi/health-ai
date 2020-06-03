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



# Heart Physiology {#heart-physiology}

Before we can understand all the parts of the ECG wave, we need to first learn more about how the heart works. The heart is made up of four chambers, two atria and two ventricles. The**atria**pump blood into the ventricles and then the**ventricles**pump blood throughout the body. Each heart cell is polarized, meaning there is a different electrical charge inside and outside of the cell. At rest, the inside of the cell is negatively charged compared to the outside. When the cell**depolarizes**, positive charges outside of the cell flow inside and makes the interior of the cell positively charged relative to the outside. This depolarization causes the cell to contract. The movement of charges across a heart cell’s membrane is the source of the electrical activity that gets measured by an ECG.

The conduction of electrical impulses through the heart follows a regular pattern orchestrated by the**cardiac conduction system**. Each heartbeat starts with an impulse in the sinus \(SA\) node. The**sinus node**is the natural pacemaker of the heart and is responsible for setting the heart’s natural rhythm. The impulse then propagates throughout the atria and causes the atria to contract. Then, the impulse enters the**atrioventricular \(AV\) node**, which delays the propagation of the signal to the ventricles. This gives the atria time to pump blood into the ventricles. After a brief pause, the signal passes through the AV node and into the ventricles, causing the ventricles to contract. This is the largest electrical disturbance in the cardiac cycle. After the ventricles contract, they**repolarize**, meaning the ions move back across the cell wall to their initial resting state.

Each part of this sequence is responsible for creating a distinct marker-- or wave --in the ECG signal. The P-wave corresponds to the depolarization of the atria. The pause between the P-wave and the Q-wave is caused by the AV node delaying the electrical impulse to the ventricles. When the ventricles contract, we see the large QRS complex. And finally, after some time, the ventricles repolarize, causing the T-wave. The atria repolarize around the same time as the ventricles finish contracting so that activity is not visible in an ECG because it is obscured by the S-wave.

This cycle repeats every heartbeat and results in a very regular and uniform looking signal.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d13_nd320-c4-l4-cardiac-cycle-animation/nd320-c4-l4-cardiac-cycle-animation.gif "Cardiac cycle animation")Cardiac cycle animation Source: By Kalumet - selbst erstellt = Own work, CC BY-SA 3.0,[https://commons.wikimedia.org/w/index.php?curid=438140](https://commons.wikimedia.org/w/index.php?curid=438140)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/048a9529-ee90-4e0f-bf00-413bb3e18983/concepts/92cb3182-7877-420b-b916-6f96347b5c2c#)

* **Atria**: The upper chambers of the heart that pass blood to the ventricles.

* **Ventricles:**The main chambers of the heart that pump blood throughout the body
* **Depolarization:**The movement of charges across a cell membrane that causes the inside of the cell to become less negatively charged.
* **Repolarization:**The movement of charges across the cell membrane that restore the negative resting charge inside the cell.
* **Cardiac conduction system:**A group of specialized cardiac cells that send signals to the heart, causing it to contract. The main components that we discussed in this course were the sinoatrial \(SA\) node and the atrioventricular \(AV\) node. Other components include the bundle of His, left and right bundle branches, and the Purkinje fibers, which propagate the signal from the AV node throughout the ventricles.
* **Sinus node:**The natural pacemaker of the heart. Responsible for generating the impulse that causes the heart to beat.
* **AV node:**Part of the cardiac conduction system that propagates the impulse from the atria to the ventricles after a delay.



