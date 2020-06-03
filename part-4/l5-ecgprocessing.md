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

# Pan-Tompkins QRS Complex Detection {#pan-tompkins-qrs-complex-detection}

Previously, we learned that ventricular contraction corresponds with the QRS complex. Finding these QRS complexes in the ECG signal is important for a number of algorithmic goals. It directly gives you information about the heart rate and heart rhythm and provides important context when doing more complex processing.

The way we find these QRS complexes is by using an algorithm called the Pan-Tompkins algorithm.

For most event detection tasks like this, we start with preprocessing steps that will boost the signal and suppress the noise. For Pan-Tompkins, we start with the bandpass filter that selects for frequencies in the QRS complex and suppresses frequencies outside of that band. This is followed by a one-sample difference, which is analogous to a derivative operation. This will preserve the steep slopes of the QRS complex and attenuate shallower ones elsewhere in the waveform. Next, we do an element-wise square which non-linearly amplifies the larger portions of our signal and makes everything positive. Finally, we do a moving sum over a fixed window length. This takes advantage of the fact the QRS complex has a fixed width of 150ms. If we tune the moving sum window to 150ms, we can fully take advantage of all the energy in the QRS complex, while attenuating other spikes that are shorter or longer than 150ms.

The detection steps are fairly simple. We simply find the peaks in our pre-processed waveform and then threshold them to select for the QRS complexes.

Next, we take a look at the code for this algorithm, and we plot the output.

# Pan-Tompkings In Code {#pan-tompkings-in-code}

We saw a basic implementation of the Pan-Tompkins algorithm and visualized how the pre-processing steps change the waveform. On noisy signals, with small QRS complexes, we saw just how well the pre-processing steps could improve our signal. Still, we saw that sometimes it would not be enough and more complex detection rules need to be implemented.

In the following exercise, we implement more sophisticated detection rules for just this purpose.

The overall goal here isn’t necessarily to understand the Pan-Tompkins algorithm per se; it’s really to get to a place where we can solve this kind of problem in any domain where you have some knowledge of the underlying physiology and the signal characteristics and the noise characteristics and you can do for your task what Pan-Tompkins algorithm does for QRS complex detection. As you learn more about these tools and operations that can boost signal and suppress noise in your specific domain, you’ll be able to build a better intuition for designing your own algorithm on your own problem.

**Notebook**

After improving the detection rules, we substantially improve the precision and recall of our detector substantially. Watch the video to see how we implement the three strategies for more robust detection:

* Refractory Period Blanking
* Adaptive Thresholding
* T-Wave Rejection

The Pan-Tompkins algorithm is a great example of an event detection algorithm. This kind of algorithm is especially common in biomedical time series processing. By diving deep into this algorithm, you’ve seen how knowledge of the underlying physiology, the signal characteristics, and the noise characteristics can help in the algorithm design. As you learn more about the tools and operations that can boost the signal and suppress noise in a specific domain, you will have better intuition for designing algorithms to solve these types of problems.

# Atrial Fibrillation {#atrial-fibrillation}

We’ve discussed atrial fibrillation previously in the context of the Apple Heart Study and the Framingham Study. Now we’ll learn what atrial fibrillation actually is. Atrial Fibrillation is a type of**arrhythmia**, which is an irregular heart rhythm.

Recall, in a normal heart rhythm, the sinus node generates the impulse that causes the atria to contract. This impulse is propagated to the AV node and then throughout ventricles, causing the ventricles to contract. This process results in a very regular rhythm called**sinus rhythm**.

In**Atrial Fibrillation**, instead of the SA node being the sole location that begins the depolarization of the atria, there are multiple locations around the atria that will spontaneously and haphazardly generate an impulse. Each of these impulses causes a partial contraction of the atria, but no single impulse depolarizes the entire atria, so there is no coherent contraction of the atria. Occasionally, one of these impulses will reach the AV node, which will then cause a ventricular contraction. But this occurs at random times, so ventricular contractions occur irregularly.

When can we examine features of the ECG signal to detect atrial fibrillation? First, there is no P-wave because there is no coherent depolarization of the atria to cause an electrical disturbance large enough to create the P-wave. Instead, we see a fibrillating wave in the T-Q segment. Second, the QRS complexes are very irregular.

We mentioned earlier that atrial fibrillation is associated with an increased risk of stroke. Because the atria are not contracting completely, stagnant pools of blood will form in the atria. These pools can form blood clots that are then circulated through the bloodstream. As these clots pass through progressively smaller and smaller arteries, they may eventually obstruct blood flow to the brain and cause a stroke.

Now we have seen how atrial fibrillation occurs physiologically and why it’s a potentially dangerous condition. Next, we will build an algorithm that automatically detects atrial fibrillation and other arrhythmias from the ECG signal.

* **Arrhythmia:**An irregular heart rhythm.

* **Sinus Rhythm:**The normal, regular heart rhythm, paced by the sinus node.
* **Atrial fibrillation:**An irregular rhythm caused by multiple, haphazard depolarizations across the atria.



