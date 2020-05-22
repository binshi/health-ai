### Types of 2D Imaging {#types-of-2d-imaging}

#### X-ray {#x-ray}

The most common type of 2D imaging is x-ray. This technique uses a machine to emit x-rays, which are absorbed differently by different tissues in the body. Bone has_high absorption\_and therefore appears\_bright white_. Soft tissues like the heart and diaphragm absorb a_medium amount\_and appear\_gray_. Air_does not absorb\_any x-rays and thus appears\_black_.

We usually think of x-rays to look for fractures/broken bones, but two of their other most common use cases are for assessing abnormalities in the lungs, and for assessing breast tissue \(mammograms\).

#### Ultrasound {#ultrasound}

Ultrasound is a type of 2D imaging technique that isn't covered in the video. It utilizes high-frequency sound waves beyond the audible limit of human hearing to generate images. Ultrasound waves travel through soft tissues or fluids and bounce back when it hits dense tissues. More waves bounce back if the tissue is denser. The waves that bounce back are captured to generate images. Ultrasound is very safe and commonly used during pregnancy.

#### Microscopy {#microscopy}

Microscopy refers to\_physical slides\_of biological material taken from a patient that can be viewed at the\_cell-level\_through a microscope. These slides often have a stain applied to them that causes different cell structures to appear in different colors. These stains help pathologists tell the difference between cell structures.

#### Fundal Imaging {#fundal-imaging}

The fundus of the eye is the interior surface of the eye, and images can be taken of it to diagnose diabetic retinopathy. In this condition, blood vessels at the back of the eye become damaged, so fundal imaging particularly looks at the integrity of the tiny vessels in the eye.

#### Differences in the imaging techniques {#differences-in-the-imaging-techniques}

Since fundal images and microscopy images are not acquired with a digital machine, they are not inherently digital like x-rays are. As a result, an additional step of digitizing these images is required before applying AI. Once microscopy and fundal images are digitized, much of the AI principals can be applied to them the same way that they can be applied to x-ray images.

The second difference is that X-ray images are stored as single-channel grayscale images, while microscopy and fundal images are stored as red-green-blue \(RGB\) three-channel images.

Another major difference is that x-rays are stored in the DICOM format, which is the standard file format for medical imaging data, while this does not apply to microscopy and fundal images. We will cover more on this later.

[![](https://video.udacity-data.com/topher/2020/April/5e9a409e_l1-clinicalworkflow-/l1-clinicalworkflow-.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/602d9c5b-4079-4738-b9dc-c82b5aa56fca/concepts/10f666bd-93d6-4510-88a9-8af51469a3e3#)

### Medical Imaging Workflows {#medical-imaging-workflows}

#### Picture Archiving and Communication System \(PACS\) {#picture-archiving-and-communication-system-pacs-}

Every imaging center and hospital have a PACS. These systems allow for all medical imaging to be stored in the hospital's servers and transferred to different departments throughout the hospital.

#### Diagnostic Imaging {#diagnostic-imaging}

In diagnostic situations, a clinician orders an imaging study because they believe that a disease\_may be present\_based on the patient's symptoms. Diagnostic imaging can be performed in\_emergency\_settings as well as\_non-emergency\_settings.

#### Screening Imaging {#screening-imaging}

Screening studies are performed on populations of individuals who\_fall into risk groups\_for certain diseases. These tend to be diseases that are relatively common, have serious consequences, but also have the potential of being reversed if detected and treated early. For example, individuals who are above a certain age with a long smoking history are candidates for lung cancer screening which is performed using x-rays on an annual basis.

### Types of 2D Imaging Algorithms {#types-of-2d-imaging-algorithms}

#### Classification {#classification}

The classification algorithm assesses a whole image and returns an output stating\_whether or not\_a disease or abnormality is present in an image. These types of algorithms can be used for binary or multi-class classification, where a single algorithm can classify for the presence or absence of multiple types of findings or diseases.

#### Localization {#localization}

Localization algorithms are intended to aid radiologists in determining\_where\_in an image a particular finding is. These types of algorithms output a set of coordinates that create a\_bounding box\_around a section of the image where a particular type of finding is. These types of algorithms can be very useful for drawing radiologists'\_attention\_to certain types of findings that are difficult to see on imaging.

#### Segmentation {#segmentation}

Segmentation algorithms return\_a set of pixels\_that contain the presence of a particular finding in an image, creating a\_border\_around a particular finding that allows for the calculation of its exact area. Segmentation algorithms are typically used to\_measures the size\_of particular findings or\_count the number\_of findings in an image. They are often used to count cells in microscopy data as well, where each cell in an image is segmented individually.

### Clinical Impact of ML for 2D Imaging {#clinical-impact-of-ml-for-2d-imaging}

You should be aware of the effect on\_clinician workflows\_when you are designing an algorithm that may be inserted into them.

### Performance of ML {#clinical-impact-of-ml-for-2d-imaging}

[![](https://video.udacity-data.com/topher/2020/April/5e9a40c6_l1-performance-2/l1-performance-2.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/602d9c5b-4079-4738-b9dc-c82b5aa56fca/concepts/2ebe640e-ffea-4bc3-95f8-928db4ae5029#)

[![](https://video.udacity-data.com/topher/2020/April/5e9a40d5_l1-performance-1/l1-performance-1.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/602d9c5b-4079-4738-b9dc-c82b5aa56fca/concepts/2ebe640e-ffea-4bc3-95f8-928db4ae5029#)

### Performance Metrics {#performance-metrics}

#### Sensitivity {#sensitivity}

Sensitivity is a metric that tells us among ALL the_positive\_cases in the dataset, how many of them are successfully identified by the algorithm, i.e. the true positive. In other words, it measures the proportion of accurately-identified\_positive cases_.

You can think of highly sensitive tests as being good for_ruling out\_disease. If someone has a negative result on a highly sensitive algorithm, it is extremely likely that they don’t have the disease since a high sensitive algorithm has low\_false negative_.

#### Specificity {#specificity}

Specificity measures ALL the\_negative\_cases in the dataset, how many of them are successfully identified by the algorithm, i.e. the true negatives. In other words, it measures the proportion of accurately-identified\_negative\_cases.

You can think of highly specific tests as being good for_ruling in\_disease. If someone has a positive result on a highly specific test, it is extremely likely that they have the disease since a high specific algorithm has low\_false positive_.

#### Dice coefficient {#dice-coefficient}

The dice coefficient measures the\_overlap\_of algorithm output and true labels. It is used to assess the performance of segmentation and localization.

[![](https://video.udacity-data.com/topher/2020/April/5e9a40f3_l1-stakeholder/l1-stakeholder.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/602d9c5b-4079-4738-b9dc-c82b5aa56fca/concepts/55370da0-f21b-4b74-9642-fa3f3c12d971#)

[![](https://video.udacity-data.com/topher/2020/April/5e9a4106_l1-stakeholderfda/l1-stakeholderfda.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/602d9c5b-4079-4738-b9dc-c82b5aa56fca/concepts/55370da0-f21b-4b74-9642-fa3f3c12d971#)

## Summary {#summary}

### Key Stakeholders {#key-stakeholders}

#### Clinical Stakeholders {#clinical-stakeholders}

Clinical stakeholders are radiologists, diagnosing clinicians and patients. Radiologists are likely the end-users of an AI application for 2D imaging. They care about low disruption to workflow and they play an important advisory role in the algorithm development process. Clinicians have less visibility into the inner workings of an algorithm. They also care about low disruption to workflows and they care about the interpretability of algorithm output. Patients may be the most important stakeholder, and the FDA looks at your algorithm through the lens of protecting the patient from all unnecessary risks. Patients may never know that AI is involved and they care about the timeliness of receiving accurate test results.

#### Industry stakeholders {#industry-stakeholders}

Industry stakeholders include medical device companies, software companies, and hospitals. Many medical device companies typically have accompanying imaging software. They also build their own AI algorithms to run on their hardware. Software companies can act more dynamically because they are not tied to a specific hardware system, but this also poses a regulatory challenge as the FDA wants to know if an algorithm performs the same across all hardware systems, and if not, which ones it is not appropriate for. Hospitals must be sure that they have the adequate infrastructure needed for algorithm deployment. In order to purchase an algorithm, a hospital must be convinced that it will save them money in the long run.

#### Regulatory stakeholder {#regulatory-stakeholder}

The main regulatory stakeholder in the medical imaging world is the Food and Drug Administration \(FDA\). The FDA treats AI algorithms as medical devices. Medical devices are broken down into three classes by the FDA, Class I, Class II, and Class III, based on their potential risks present to the patient. A device's class dictates the safety controls, which in turn dictates which regulatory pathway they must go down. The two main regulatory pathways for medical devices are**510\(k\)**and**Pre-market Approval \(PMA\).**Lower risk devices \(Classes I & II\) usually take a 501\(k\) submission pathway. Higher risk devices and algorithms \(Class III\) must go through PMA.



