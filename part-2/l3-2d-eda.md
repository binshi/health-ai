[![](https://video.udacity-data.com/topher/2020/April/5e9a448e_l2-outline/l2-outline.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/6da8b250-4ac1-424f-a227-ba653850bef8/concepts/5bff94dd-ac1d-4c8d-a1fa-357c0c426994#)

## Summary {#summary}

In this lesson, we will first learn what DICOM standard is. Then, we'll talk about how to read images and different image properties. Next, we'll talk about how to prepare non-image data and extract information for training machine learning models. Finally, we'll talk about exploring medical metadata.

At the end of this lesson, you'll be a pro at dissecting medical images, starting with the standard file formats that they are stored in, all the way down to the pixel-level data that comprises the image itself.

[![](https://video.udacity-data.com/topher/2020/April/5e9a47ef_l2-study/l2-study.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/6da8b250-4ac1-424f-a227-ba653850bef8/concepts/2b884652-f656-49f3-a52d-fcd0ae6edb9f#)

[![](https://video.udacity-data.com/topher/2020/April/5e9a47e5_l2-dicom/l2-dicom.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/6da8b250-4ac1-424f-a227-ba653850bef8/concepts/2b884652-f656-49f3-a52d-fcd0ae6edb9f#)

## Summary {#summary}

#### DICOM {#dicom}

DICOM is short for “Digital Imaging and Communications in Medicine”, which is the standard for the communication and management of medical imaging information and related data. DICOM files are a medical imaging file that is in the format that conforms to the DICOM standard.

It was developed by the American College of Radiologists in 1993 to allow for interoperability.

A DICOM file contains information about the imaging acquisition method, the actual medical images, and patient information. It has a header component that contains information about the acquired image and an image component that is a set of pixel data representing the actual images

Protected Health Information \(PHI\) is part of DICOM and clinical data and radiologist report are\_not\_part of DICOM

#### DICOM studies and series {#dicom-studies-and-series}

With 2D imaging, a single 2D image is known as a single DICOM series. All image series combined comprise a study of the patient, known as a DICOM study.

## Summary {#summary}

Some metadata may come from the DICOM headers, patient history, and image labels. Once we have all of a dataset's metadata stored in a single place, we'll then want to explore data features.

#### Histograms {#histograms}

Histograms help us look at_distributions\_of\_single variables_. Sometimes we only want to look at distributions within a\_single class\_of our data.

#### Scatterplots {#scatterplots}

Scatterplots are useful for assessing_relationships\_between\_two variables_.

#### Pearson Correlation Coefficient {#pearson-correlation-coefficient}

Pearson Correlation Coefficient measures how two variables are linearly related. The value ranges from -1 to 1. A value of 1 or -1 means the two variables are perfectly linearly related. A value of 0 implies there is no linear relationship between the two variables.

#### Co-Occurrence Matrices {#co-occurrence-matrices}

Co-Occurrence Matrices are useful for assessing how frequently different classifications co-occur together.

## Glossary {#glossary}

* **DICOM:**Digital Imaging and Communications in Medicine \(DICOM\) is the standard for the communication and management of medical imaging information and related data
* **Image artifact: **An object or distortion in an image that reduces its quality
* **Foreign body: **An object in a medical image that is not biological material from the patient, such as a pacemaker or wire
* **Metadata: **A set of data that describes another set of data
* **Pathologist: **A special type of clinician who reads and interprets microscopy and digital pathology data
* **intensity profile: **the distribution of all pixels' intensity values that comprise an image
* **PHI: **any individually identifiable health information, including demographic data, insurance information, and other information used to identify a patient

[![](https://video.udacity-data.com/topher/2020/April/5e9a54c8_l2-conclusion/l2-conclusion.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/6da8b250-4ac1-424f-a227-ba653850bef8/concepts/a7336932-3e6e-49c6-9906-5508da4c0275#)

## Lesson conclusion {#lesson-conclusion}

We first talked about the DICOM standard and what a DICOM file is. Then we talked about the patient study and series. After that, we learned that a DICOM file consists of attributes about patient information, patient study, series, and images.

In the second section, we talked about how to read and extract images using the pydicom package, what we should pay attention to when we look at an image, and how to explore image intensity profile at the pixel level.

In the third section, we discussed how to prepare non-image data for ML using the DICOM header. DICOM header has a wealth of information such as patient data, series, and study and we have to explore DICOM header carefully before developing the algorithm.

In the last section, we talked about metadata. Metadata can come from several sources: DICOM header, patient history, and image labels. We need to extract data features before we train the model. Some useful tools are histograms, scatterplots, Pearson correlation coefficient, and co-occurrence matrix.

## Further reading {#further-reading}

* [This website](https://www.dicomstandard.org/)
  will tell you pretty much everything you ever wanted to know about DICOM
* The
  [github repo](https://pydicom.github.io/)
  for pydicom has lots of documentation, as well as examples of how to use pydicom with DICOM and some sample datasets
* This
  [review article](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6289005/)
  provides a really thorough overview of digital pathology and its history



