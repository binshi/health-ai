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

Protected Health Information \(PHI\) is part of DICOM and clinical data and radiologist report are_not_part of DICOM

#### DICOM studies and series {#dicom-studies-and-series}

With 2D imaging, a single 2D image is known as a single DICOM series. All image series combined comprise a study of the patient, known as a DICOM study.

