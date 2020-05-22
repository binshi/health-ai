### Types of 2D Imaging {#types-of-2d-imaging}

#### X-ray {#x-ray}

The most common type of 2D imaging is x-ray. This technique uses a machine to emit x-rays, which are absorbed differently by different tissues in the body. Bone has_high absorption_and therefore appears_bright white_. Soft tissues like the heart and diaphragm absorb a_medium amount_and appear_gray_. Air_does not absorb_any x-rays and thus appears_black_.

We usually think of x-rays to look for fractures/broken bones, but two of their other most common use cases are for assessing abnormalities in the lungs, and for assessing breast tissue \(mammograms\).

#### Ultrasound {#ultrasound}

Ultrasound is a type of 2D imaging technique that isn't covered in the video. It utilizes high-frequency sound waves beyond the audible limit of human hearing to generate images. Ultrasound waves travel through soft tissues or fluids and bounce back when it hits dense tissues. More waves bounce back if the tissue is denser. The waves that bounce back are captured to generate images. Ultrasound is very safe and commonly used during pregnancy.

#### Microscopy {#microscopy}

Microscopy refers to_physical slides_of biological material taken from a patient that can be viewed at the_cell-level_through a microscope. These slides often have a stain applied to them that causes different cell structures to appear in different colors. These stains help pathologists tell the difference between cell structures.

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

In diagnostic situations, a clinician orders an imaging study because they believe that a disease_may be present_based on the patient's symptoms. Diagnostic imaging can be performed in_emergency_settings as well as_non-emergency_settings.

#### Screening Imaging {#screening-imaging}

Screening studies are performed on populations of individuals who_fall into risk groups_for certain diseases. These tend to be diseases that are relatively common, have serious consequences, but also have the potential of being reversed if detected and treated early. For example, individuals who are above a certain age with a long smoking history are candidates for lung cancer screening which is performed using x-rays on an annual basis.



### Types of 2D Imaging Algorithms {#types-of-2d-imaging-algorithms}

#### Classification {#classification}

The classification algorithm assesses a whole image and returns an output stating_whether or not_a disease or abnormality is present in an image. These types of algorithms can be used for binary or multi-class classification, where a single algorithm can classify for the presence or absence of multiple types of findings or diseases.

#### Localization {#localization}

Localization algorithms are intended to aid radiologists in determining_where_in an image a particular finding is. These types of algorithms output a set of coordinates that create a_bounding box_around a section of the image where a particular type of finding is. These types of algorithms can be very useful for drawing radiologists'_attention_to certain types of findings that are difficult to see on imaging.

#### Segmentation {#segmentation}

Segmentation algorithms return_a set of pixels_that contain the presence of a particular finding in an image, creating a_border_around a particular finding that allows for the calculation of its exact area. Segmentation algorithms are typically used to_measures the size_of particular findings or_count the number_of findings in an image. They are often used to count cells in microscopy data as well, where each cell in an image is segmented individually.

### Clinical Impact of ML for 2D Imaging {#clinical-impact-of-ml-for-2d-imaging}

You should be aware of the effect on_clinician workflows_when you are designing an algorithm that may be inserted into them.

