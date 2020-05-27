New Vocab

* **Imaging modality**: a device used to acquire a medical image
* **MRI scanner**: Magnetic Resonance Imaging scanner
* **Contrast resolution**: the ability of an imaging modality to distinguish between differences in image intensity
* **Spatial resolution**: the ability of an imaging modality to differentiate between smaller objects

EG: SPECT/PET, Ultrasound, MRI, CT

![](/assets/Screenshot 2020-05-27 at 7.42.53 AM.png)![](/assets/Screenshot 2020-05-27 at 7.45.28 AM.png)![](/assets/Screenshot 2020-05-27 at 7.46.50 AM.png)![](/assets/Screenshot 2020-05-27 at 7.59.38 AM.png)![](/assets/Screenshot 2020-05-27 at 7.59.29 AM.png)![](/assets/Screenshot 2020-05-27 at 7.58.56 AM.png)Lesson 7 Exercise – Sample Solution

#### Part 1: Choosing a clinical case. {#part-1-choosing-a-clinical-case-}

I chose to focus on the case -[COVID-19 Compatible Chest CT Pattern](https://www.acrdsi.org/DSI-Services/Define-AI/Use-Cases/COVID-19-Compatible-Chest-CT-Pattern).

#### Part 2: Background research. {#part-2-background-research-}

I then focused on[Performance of radiologists in differentiating COVID-19 from viral pneumonia on chest CT](https://pubs.rsna.org/doi/10.1148/radiol.2020200823)paper by Bai, H.X., Hsieh, B., et. al. for my background research.

#### Part 3: Framing the problem as a machine learning task. {#part-3-framing-the-problem-as-a-machine-learning-task-}

Per the ACR-DSI, the assigned task is to provide a likelihood of a diagnosis compatible with COVID-19 using chest CT data. I believe an initial step in solving this clinical problem, determining whether any abnormal findings are present on a chest CT, would be well-framed as an object detection task. The output would be a bounding box delineating the areas of abnormality that could indicate viral pneumonia. Based on recent literature, such findings could include consolidation, bilateral and peripheral disease, linear opacities, “crazy-paving” patterns, and the “reverse halo” sign. While these findings are not-specific for COVID-19, their detection could guide the ordering clinician to be more vigilant in follow-up testing for the disease. A negative STAT rapid influenza/RSV PCR tests and positive Real-Time Reverse Transcriptase Polymerase Chain Reaction \(rRT-PCR\) would confirm the diagnosis.

## X-rays {#x-rays}

The main operating agents of a CT scanner are X-rays, which are a form of electromagnetic radiation. A reminder on electromagnetic spectrum below:

\[!\[\]\([https://video.udacity-data.com/topher/2020/April/5e9bf43d\_l1-em-spectrum/l1-em-spectrum.png](https://video.udacity-data.com/topher/2020/April/5e9bf43d_l1-em-spectrum/l1-em-spectrum.png) "Electromagnetic spectrum

em\_spectrum.png"\)Electromagnetic spectrum\]\([https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/d8617924-a6e1-4c49-9bdb-3388e95fc3b7/concepts/23e69e45-ff28-439f-bf0c-4d142d5802f8\#](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/d8617924-a6e1-4c49-9bdb-3388e95fc3b7/concepts/23e69e45-ff28-439f-bf0c-4d142d5802f8#)\)

X-rays are a form of ionizing radiation, which means that they carry enough energy to detach electrons from atoms. This presents certain health risks, but the short wavelength of this part of the electromagnetic spectrum allows the radiation to interact with the many structures that compose a human body, thus allowing us to measure the amount of photons that reach detectors and make deductions about the structures that were in the way of photons as they were traveling from the source to the detector, with a high precision.

## CT scanners {#ct-scanners}

![](/assets/Screenshot 2020-05-27 at 8.20.26 AM.png)

![](/assets/Screenshot 2020-05-27 at 8.19.35 AM.png)

As you have seen, the CT scanner operates by projecting X-rays through the subject’s body. 

**CT scanner**: computed tomography scanner

X-rays get absorbed or scattered by the anatomy and thus detectors measure the amount of this attenuation that happens along each path that the ray is taking. A collimator shapes the beam and ensures that the X-rays only pass through a narrow slice of the object being imaged. Rotation of a source inside a gantry makes sure that projections happen from different angles so that we can get a good 2D representation of the slice. The moving table ensures that multiple such slices are imaged. A collection of slices makes up a 3-dimensional CT image.

