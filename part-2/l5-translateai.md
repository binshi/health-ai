[![](https://video.udacity-data.com/topher/2020/April/5e9b9d0c_l4-overview/l4-overview.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/7d14ac87-b711-44a9-95b7-0c79ae6c8d25/concepts/22e3e479-b45b-458d-b6a0-109d104c58da#)

In today’s lesson, we’ll start with a high-level overview of the FDA regulatory process and talk about the intended use followed by a discussion on how to identify and disclose algorithmic limitations. Then, we’ll talk about performance statistics that you will want to use when labeling your device. And finally, how to build an FDA validation plan.

[![](https://video.udacity-data.com/topher/2020/April/5e9b9f9c_l1-stakeholderfda/l1-stakeholderfda.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/7d14ac87-b711-44a9-95b7-0c79ae6c8d25/concepts/c8caca94-2178-43af-97eb-5b63a131d2db#)

#### Intended use {#intended-use}

The FDA will require you to provide an intended use statement and an indication for use statement. The intended use statement tells the FDA exactly\_what\_your algorithm is used for. Not what it could be used for. And FDA will use this statement to define the risk and class of your algorithm.

#### Indication for use {#indication-for-use}

You can use the indications for use statement to make more\_specific suggestions\_about how your algorithm could be used. Indications for use statement describes precise situations and reasons\_where and why\_you would use this device.

![](/assets/Screenshot 2020-05-24 at 8.46.54 AM.png)

#### Algorithm limitations {#algorithm-limitations}

When the FDA talks about limitations, they want to know more about scenarios where your algorithm is not safe and effective to use. In other words, they want to know where our algorithm will_fail_.

#### Computational limitations {#computational-limitations}

If your algorithm needs to work in an emergency workflow, you need to consider computational limitations and inform the FDA that the algorithm does not achieve fast performance in the absence of certain types of computational infrastructure. This would let your end consumers know if the device is right for them.

#### Medical device reporting {#medical-device-reporting}

After your algorithm is cleared by the FDA and released, the FDA has a system called\_Medical Device Reporting\_to continuously monitor. Any time one of your end-users discovers a malfunction in your software, they report this back to you, the manufacturer, and you are required to report it back to the FDA. Depending on the severity of the malfunction, and whether or not it is life-threatening, the FDA will either completely recall your device or require you to update its labeling and explicitly state new limitations that have been encountered.

# Translate Performance into Clinical Utility {#translate-performance-into-clinical-utility}

[![](https://video.udacity-data.com/topher/2020/April/5e9ba677_l4-pre/l4-pre.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/7d14ac87-b711-44a9-95b7-0c79ae6c8d25/concepts/453a4e07-8266-4c58-8ced-882a3cb9dd37#)

[![](https://video.udacity-data.com/topher/2020/April/5e9ba683_l4-prc/l4-prc.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/7d14ac87-b711-44a9-95b7-0c79ae6c8d25/concepts/453a4e07-8266-4c58-8ced-882a3cb9dd37#)

[![](https://video.udacity-data.com/topher/2020/April/5e9ba68d_l4-f1/l4-f1.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/7d14ac87-b711-44a9-95b7-0c79ae6c8d25/concepts/453a4e07-8266-4c58-8ced-882a3cb9dd37#)

## Summary {#summary}

#### Precision {#precision}

Precision looks at the number of positive cases accurately identified by an algorithm divided by all of the cases identified as positive by the algorithm_no matter whether they are identified right or wrong_. This metric is also commonly referred to as the positive predictive value.

#### Precision and recall {#precision-and-recall}

A high precision test gives you more confidence that a positive test result is actually positive since a high precision test has low false positive. This metric, however, does not take false negatives into account. So a high precision test could still miss a lot of positive cases. Because of this, high-precision tests don’t necessarily make for great stand-alone diagnostics but are beneficial when you want to\_confirm\_a suspected diagnosis.

When a high recall test returns a negative result, you can be confident that the result is truly negative since a high recall test has low false negatives. Recall does not take false positives into account though, so you may have high recall but are still labeling a lot of negative cases as positive. Because of this, high recall tests are good for things like screening studies, where you want to make sure someone\_doesn’t\_have a disease or worklist prioritization where you want to make sure that people\_without\_the disease are being de-prioritized.

Optimizing one of these metrics usually comes at the expense of sacrificing the other.

#### Threshold {#threshold}

CNN models output a probability ranging from 0-1 that indicates how likely the image belongs to a class. We will need a cut-off value called threshold to assist in making the decision if the probability is high enough to belong to one class. Recall and precision vary when a different threshold is chosen.

#### Precision-recall curve {#precision-recall-curve}

Precision-recall curve plots recall in the x-axis and precision in the y-axis. Each point along the curve represents precision and recall under a different threshold value.

#### F1 score {#f1-score}

For binary classification problems, the F1 score combines both precision and recall. F1 score allows us to better measure a test’s accuracy when there are class_imbalances_. Mathematically, it is the harmonic mean of precision and recall.

[![](https://video.udacity-data.com/topher/2020/April/5e9ba9c8_l4-fda/l4-fda.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/7d14ac87-b711-44a9-95b7-0c79ae6c8d25/concepts/f77a9787-4260-47e6-8aea-7071a4d10c3b#)

## Summary {#summary}

### FDA validation plan {#fda-validation-plan}

#### FDA validation set {#fda-validation-set}

You'll need to perform a standalone clinical assessment of your tool that uses an\_FDA validation set\_from a real-world\_clinical setting\_to prove to the FDA that your algorithm works. You will run this FDA validation set through your algorithm just ONCE.

You’ll need to identify a clinical partner who you can work with to gather the “BEST” data for your validation plan. This partner will collect data from a real-world clinical setting that you describe so that you can then see how your algorithm performs under these specifications.

#### Collect the FDA validation set {#collect-the-fda-validation-set}

You need to identify a clinical partner to gather the FDA validation set. First, you need to describe who you want the data from. Second, you need to specify what types of images you’re looking for.

#### Establish the ground truth {#establish-the-ground-truth}

You need to gather the ground truth that can be used to compare the model output tested on the FDA validation set. The choice of your ground truth method ties back to your\_intended use\_statement. Depending on the intended use of the algorithm, the ground truth can be very different.

#### Performance standard {#performance-standard}

For your validation plan, you need evidence to support your reasoning. As a result, you need a performance standard. This step usually involves a lot of literature searching.

Depending on the use case for your algorithm, part of your validation plan may need to include assessing\_how fast\_your algorithm can read a study.



## Further reading {#further-reading}

* The FDA's [website](https://www.fda.gov/medical-devices/digital-health/software-medical-device-samd) should be your go-to for how they view software as a medical device
* [Greenlight Guru](https://www.greenlight.guru/blog) produces an excellent blog about the FDA process for medical devices
* Qualio produces a Quality Management System \(QMS\) for actually getting through the FDA process and their [blog](https://www.qualio.com/blog)
  is also a great resource



