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

# Arrhythmia Detection: Dataset {#arrhythmia-detection-dataset}

In the next few concepts, we’ll be building an arrhythmia classifier. The data we will use comes from the[Computing in Cardiology \(CinC\) Challenge 2017 dataset](https://physionet.org/content/challenge-2017/1.0.0/)hosted on Physionet.

The dataset contains thousands of short ECG snippets \(30s - 60s\) from the AliveCor mobile ECG monitor. The original challenge was to build a 4-class classifier for sinus rhythm, atrial fibrillation, alternative rhythm, and noisy record. We will throw out the noisy records and build a two-class classifier distinguishing between sinus rhythm and another rhythm \(atrial fibrillation included\).

From exploring our data, we see that we have 1.5x more sinus rhythm records than other rhythm records. Most of the records are 30 seconds long; some are 60 seconds long, and a few are somewhere in between.

We plot the data and visualize the QRS complex detections provided in the dataset. The QRS complex detector still detects QRS complexes during periods of high noise, but these detections are suspect.

**In the next few concepts, we’ll be building an arrhythmia classifier**. The data we will use comes from the[Computing in Cardiology \(CinC\) Challenge 2017 dataset](https://physionet.org/content/challenge-2017/1.0.0/)hosted on Physionet.

The dataset contains thousands of short ECG snippets \(30s - 60s\) from the AliveCor mobile ECG monitor. The original challenge was to build a 4-class classifier for sinus rhythm, atrial fibrillation, alternative rhythm, and noisy record. We will throw out the noisy records and build a two-class classifier distinguishing between sinus rhythm and another rhythm \(atrial fibrillation included\).

From exploring our data, we see that we have 1.5x more sinus rhythm records than other rhythm records. Most of the records are 30 seconds long; some are 60 seconds long, and a few are somewhere in between.

We plot the data and visualize the QRS complex detections provided in the dataset. The QRS complex detector still detects QRS complexes during periods of high noise, but these detections are suspect.

# Arrhythmia Detection: Features {#arrhythmia-detection-features}

As with activity classification, we will featurize our raw signal and throw those features into a classifier to detect an abnormal heart rhythm. Our input signal will be the time between two QRS complexes, known as the RR interval \(for the R-wave\). This is also called the**inter-beat-interval**. Our input signal will be derived from the output of the Pan-Tompkins algorithm. We will include some of the features in

> [Behar, Rosenberg, Yaniv, Oster. Rhythm and Quality Classification from Short ECGs Recorded Using a Mobile Device. Computing in Cardiology Challenge 2017.](https://www.google.com/url?q=http://www.cinc.org/archives/2017/pdf/165-056.pdf&sa=D&ust=1580267302117000&usg=AFQjCNEKkpmUb8YRX5AjMTMAbTxJRJbbCw)

and

> [Bonizzi, Driessens, Karel. Detection of Atrial Fibrillation Episodes from Short Single Lead Recordings by Means of Ensemble Learning. Computing in Cardiology Challenge 2017.](https://www.google.com/url?q=http://www.cinc.org/archives/2017/pdf/169-313.pdf&sa=D&ust=1580267302117000&usg=AFQjCNHKpTlpKBK9593XqAV23YBN1lmO8Q)

which were entries in the CinC challenge. Recall that the RR interval time series is irregularly sampled heartbeat because we get a new measurement at each heartbeat, not at some fixed interval in time. As you’ll soon see, many of the features we want can be computed on an irregular time series, but some, especially frequency domain features, cannot. In that case, we’ll first have to make this uniform using interpolation before we can compute our frequency domain features.

So we can separate our features into time domain features and frequency domain features. The first few are summary statistics like min, max, median, mean, standard deviation. Then we want to compute the number of outliers. And we’ll say an outlier is an RR interval that is greater than 1.2 times the average RR interval in that record.

Next, we want to compute the root-mean-square of the difference between adjacent RR intervals. And lastly, we want the percent of RR interval differences that are greater than 50 milliseconds.

Before we compute our frequency domain features, we will need to regularize the RR interval time series so that we can take the FFT of it. We want to interpolate the RR interval time series onto a regular 4 Hz time grid. Then the features we want will be the largest FFT magnitude component between 0.04 Hz and 0.15 Hz. We also want to know at which frequency this occurs. And we want both of these values for the band between 0.15 Hz and 0.4 Hz as well.

I will leave the feature computation code for you to do in the next exercise and in the next video we will pick this back up with the implementation.

## Glossary {#glossary}

* **inter-beat-interval: **The time between successive heart beats. Also called the RR interval.

In this lesson, we saw a good example of how algorithms can be cascaded together. Lower level algorithms operate on the raw data and produce a set of more meaningful and versatile metrics. These metrics can then be inputs into various higher-level algorithms that are more informative about the underlying physiology of the wearer, their health, or their disease state. This allows us to independently iterate on different parts of this stack. Making the underlying algorithms more accurate will generally improve the higher-level algorithms, but this is not always the case. It’s important to remember that any change made in the lower level algorithms will have downstream effects.

We also saw how having a solid understanding of the physiological processes that generate our signal can have a profound impact on how we design our algorithm.

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

## Further Resources {#further-resources}

[Dale Dubin’s Rapid Interpretation of EKG’s](https://www.amazon.com/Rapid-Interpretation-EKGs-Sixth-Dubin/dp/0912912065/)is one of the best resources for quickly understanding the ECG signal.

The original Pan-Tompkins algorithm paper.  
[Pan J, Tompkins WJ. A real-time QRS detection algorithm. IEEE Trans Biomed Eng. 1985;32\(3\):230–236. doi:10.1109/TBME.1985.325532](https://ieeexplore.ieee.org/document/4122029)

These two papers were the inspiration for the classifier that we built. They used a lot more features and included information from the raw ECG signal as well. See how they built 4-class classifiers and performance was evaluated in this challenge.

* [Behar, Rosenberg, Yaniv, Oster. Rhythm and Quality Classification from Short ECGs Recorded Using a Mobile Device. Computing in Cardiology Challenge 2017.](http://www.cinc.org/archives/2017/pdf/165-056.pdf)

* [Bonizzi, Driessens, Karel. Detection of Atrial Fibrillation Episodes from Short Single Lead Recordings by Means of Ensemble Learning. Computing in Cardiology Challenge 2017.](http://www.cinc.org/archives/2017/pdf/169-313.pdf)

This paper describes some important features that have been shown to be good for atrial fibrillation detection based on the irregularity of RR intervals.  
[Sarkar S, Ritscher D, Mehra R. A detector for a chronic implantable atrial tachyarrhythmia monitor. Biomedical Engineering IEEE Transactions on 2008;55\(3\):1219–1224.](https://ieeexplore.ieee.org/document/4360105)

One of the most famous databases for ECG arrhythmia detection is the[MIT-BIH database](https://physionet.org/content/mitdb/1.0.0/).[This article](http://ecg.mit.edu/george/publications/mitdb-embs-2001.pdf)is a great overview of the database and its impact.

## Glossary {#glossary}

* **Atria**: The upper chambers of the heart that pass blood to the ventricles.
* **Ventricles:**The main chambers of the heart that pump blood throughout the body
* **Depolarization:**The movement of charges across a cell membrane that causes the inside of the cell to become less negatively charged.
* **Repolarization:**The movement of charges across the cell membrane that restore the negative resting charge inside the cell.
* **Cardiac conduction system:**A group of specialized cardiac cells that send signals to the heart, causing it to contract. The main components that we discussed in this course were the sinoatrial \(SA\) node and the atrioventricular \(AV\) node. Other components include the bundle of His, left and right bundle branches, and the Purkinje fibers, which propagate the signal from the AV node throughout the ventricles.
* **Sinus node:**The natural pacemaker of the heart. Responsible for generating the impulse that causes the heart to beat.
* **AV node:**Part of the cardiac conduction system that propagates the impulse from the atria to the ventricles after a delay.
* **Refractory Period:**The period after depolarization where a cell cannot depolarize again.
* **Sinus Rhythm:**The normal, regular heart rhythm, paced by the sinus node.
* **Arrhythmia:**An irregular heart rhythm.
* **Atrial fibrillation:**An irregular rhythm caused by multiple, haphazard depolarizations across the atria.
* **inter-beat-interval:**The time between successive heartbeats. Also called the RR interval.



