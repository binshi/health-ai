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

## Summary {#summary}

I think this is a landmark study in the use of wearables for clinical research, but I don’t think we should overvalue that 34% of participants were confirmed to have AF or how the Apple Watch's use as a screening or diagnostic tool. The reason for this is because the study deviated from typical clinical trials that try to prove the effectiveness of devices or drugs in a few significant ways:

* Not everyone received an ECG chest patch, so we don’t know the false-negative rate.
* The inclusion criteria \(Apple Watch and iPhone users\) created a biased study population.

If wearable research is biased towards a more affluent population, the interventions or discoveries made may be more specifically tailored for this population and may be less effective on a less affluent population. This is similar to why researchers think long and hard about how to enroll minority populations in their clinical trials.

Despite the caveats above, this study broke new ground in significant ways:

* Recruited almost half a million people for the study in 8 months.
* These types of studies lose a lot of participants to follow-up \(e.g., over 2000 participants were notified of having irregular pulse rates but only collected data from 450 ECG chest patches.\)
* How participants would react to a smartwatch notifying them of an abnormal heart rhythm while it was happening.

I think the study is super exciting in that it's a first attempt to use the capability of wearables to do long term monitoring in a medical context. There’s obviously a lot of hype around studies like this and I want us to be excited about the right things.

## Classification Accuracy {#classification-accuracy}

From activity classification to AF detection, many tasks in wearable healthcare involve classification. In the case of the AHS, participants were classified by the “irregular pulse” notification into AF and non-AF groups. One way of discussing**classification accuracy**in binary classification is by looking at precision and recall.

* **precision**: the proportion of all true positive cases that are detected positive by the classifier
* **recall**: the proportion of all positives detected by the classifier that are true positives

This is demonstrated graphically below:

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d4b_nd320-c4-l0-precision-recall-wiki/nd320-c4-l0-precision-recall-wiki.png "Precision and Recall")Precision and Recall  
Source: Walber. Precision and recall. Nov 2014. CC-BY-SA-4.0[Link](https://commons.wikimedia.org/wiki/File:Precisionrecall.svg)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/4417cb06-0a1d-4dad-9418-2e615308b2f6/concepts/fd2dfaee-57d9-43a6-8791-87603e962db7#)

In the AHS study, 404 participants got a new diagnosis of AF from 929 who had an irregular pulse notification. And 3070 participants got a new diagnosis of AF from 293,015 who didn't get an irregular pulse notification. If the irregular pulse notification is used to classify participants that will and will not receive an AF diagnosis, what is the precision and recall? \(Round to the nearest %\)

True Positives = 404 False Positives = 929 - 404 = 525 False Negatives = 3070

Precision = True Positives / \(True Positives + False Positives\) = 404 / 929 = 43% Recall = True Positives / \(True Positives + False Negatives\) = 404 / 3474 = 12%

## Further Research {#further-research}

Wearables are part of Digital Health, learn more about it[here](https://en.wikipedia.org/wiki/Digital_health).

The wikipedia page for[precision and recall](https://en.wikipedia.org/wiki/Precision_and_recall)

The Framingham Study is a landmark study in cardiovascular health. Many interesting papers came out of it, and I highly recommend exploring them. Start[here with the Wikipedia page](https://en.wikipedia.org/wiki/Framingham_Heart_Study)

If you would like to read up on the AHS on your own, you can find the clinical trial study record detail[here](https://clinicaltrials.gov/ct2/show/NCT03335800).

### Relevant Papers {#relevant-papers}

* **Apple Heart Study **- Perez MV, Mahaffey KW, Hedlin H, et al. "Large-scale assessment of a smartwatch to identify atrial fibrillation." \_N Engl J Med \_2019;381:1909-1917. [Link](https://www.nejm.org/doi/full/10.1056/NEJMoa1901183)
* **Apple Heart Study Response **- Campion Edward W., Jarcho John A.. \(2019\) "Watched by Apple." \_N Engl J Med \_381:20, 1964-1965. [Link](https://www.nejm.org/doi/full/10.1056/NEJMe1913980)
* **Framingham Study **- Wolf PA, Abbott RD, Kannel WB. "Atrial fibrillation as an independent risk factor for stroke: the Framingham Study." \_Stroke \_1991;22:983-988. [Link](https://www.ahajournals.org/doi/10.1161/01.STR.22.8.983)
* **Wearable Health**- Montgomery, K., Chester, J.,  &  Kopp, K. \(2018\). "Health Wearables: Ensuring Fairness, Preventing Discrimination, and Promoting Equity in an Emerging Internet-of-Things Environment." _Journal of Information Policy_
  , 8, 34-77. doi:10.5325/jinfopoli.8.2018.0034 [Link](https://www.jstor.org/stable/10.5325/jinfopoli.8.2018.0034#metadata_info_tab_contents)

## New Vocabulary {#new-vocabulary}

* **Inclusion Criteria **: Characteristics that potential study subjects must have for them to be included in the study.
* **Exclusion Criteria**: Characteristics that disqualify potential study subjects from participating in a clinical study. e.g., Some common ones are being under 18 or pregnant.
* **Classification Accuracy**: A metric for evaluating the performance of a classifier -- the fraction of classifications that are correct. For rare events \(like atrial fibrillation\), this metric is unsuitable. For example, a classifier that classifies every data point as healthy would have a classification accuracy of 99%, as around 1 percent of the population has atrial fibrillation, but would be relatively useless.
* **Precision**: The fraction of positive classifications that are correct.
* **Recall**: The fraction of positive elements that are classified correctly as positive.

## Glossary {#glossary}

* **Inclusion Criteria**: Characteristics that potential study subjects must have for them to be included in the study.
* **Exclusion Criteria**: Characteristics that disqualify potential study subjects from participating in a clinical study. e.g., Some common ones are being under 18 or pregnant.
* **Classification Accuracy**: A metric for evaluating the performance of a classifier -- the fraction of classifications that are correct. For rare events \(like atrial fibrillation\), this metric is unsuitable. For example, a classifier that classifies every data point as healthy would have a classification accuracy of 99%, as around 1 percent of the population has atrial fibrillation, but would be relatively useless.
* **Precision**: The fraction of positive classifications that are correct.
* **Recall**: The fraction of positive elements that are classified correctly as positive.
* **Primary Endpoint**: The metric being used to answer the question that the study seeks to ask. For drug trials, this would be markers of the disease the drug seeks to treat. For example, the primary endpoint for a study on the effectiveness of a new statin in preventing heart attacks would be the number of heart attacks in the test group compared to a control group. The number and types of participants enrolled in a study are designed with the primary endpoint in mind.



