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

# Physical principles of MR scanners {#physical-principles-of-mr-scanners}

MR \(magnetic resonance\) scanner is arguably quite a bit more complex from the underlying computation and physics standpoint than a CT machine, and also quite a bit more flexible.

Let me take you through a very basic introduction into how these wonderful machines operate and produce images.

![](/assets/Screenshot 2020-05-27 at 8.27.35 AM.png)

MR scanner leverages a basic physical property of protons \(charged elementary particles that make up atoms\) to align themselves along the vector of magnetic fields. This effect is particularly pronounced in protons that make up hydrogen atoms. Hydrogen atoms make up water molecules, and water makes up to 50-70% of a human body.

The thing with protons is that they possess a property called spin which could be thought of as spinning around an axis. In a normal environment, the direction of this axis is randomly distributed across different protons. In the presence of a strong magnetic field, though, the proton spins get aligned along the direction of the magnetic field, and start precessing \(think of what a spinning top that’s lost some of its momentum is doing\):

[![](https://video.udacity-data.com/topher/2020/April/5e9bf442_l1-proton/l1-proton.gif "Proton.gif
A proton orienting itself along the lines of a magnetic field")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/d8617924-a6e1-4c49-9bdb-3388e95fc3b7/concepts/0b3c6e1a-2ae1-435f-9764-78ab788ee614#)

> Fun fact: “strong magnetic field” means really strong. Look up “[MRI metal chair experiment](https://www.youtube.com/watch?v=6BBx8BwLhqg)” on YouTube to get a sense of what that is like. Such a static magnet can rip metal from a patient’s body - that is why anyone going into a scan will get asked if they have any magnetic implants and will be asked to remove any jewelry. That is also why certain patients \(e.g. with metal shards left over from a trauma\) would not be able to get imaged in an MRI scanner.

When an external radiofrequency pulse is applied, of a frequency proportional to the frequency of precession, the protons respond to this pulse in unison, or_resonate_, and flip the orientation of their spins. Once this pulse is gone, they return to their original orientation \(along the static magnetic field\).

The way in which protons return to their original orientation is different and depends on the tissue type that protons are a part of.

Since many protons are returning to their original orientation at once, they generate electrical currents in the coils that are placed nearby. Due to the resonance effect these currents are not insignificant and can be measured - these measurements constitute the data about the tissue being studied which is collected by the MRI scanner.

Gradient fields are used to vary the static magnetic field, and thus precession frequency, spatially. This allows the MR scanner to isolate a part of the body \(i.e. a slice\) that is being imaging. Further gradient fields are used to isolate information coming from specific locations within a slice.

[![](https://video.udacity-data.com/topher/2020/April/5e9bffe5_l1-em-fields/l1-em-fields.png "Electromagnetic fields of an MR scanner")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/d8617924-a6e1-4c49-9bdb-3388e95fc3b7/concepts/0b3c6e1a-2ae1-435f-9764-78ab788ee614#)

## K-space and Reconstruction {#k-space-and-reconstruction}

The currents measured by RF coils get turned into a digital format, and represented as vectors in “**K-space**”. The concept of K-space goes back to the wave theory in physics and basically defines a space of vectors that describe characteristics of electromagnetic waves.

In our case, these wave vectors carry information about the characteristics of the matter in the space that has been measured. Essentially, these vectors record the spatial frequency of signal intensity, and thus, through the process that involves an Inverse Fourier Transform and a lot of de-noising and other optimizations, get turned into a familiar 2D image that represents a slice through a human body with different anatomy having different pixel intensity. This process is referred to as **image reconstruction **in MR physics. Typically, image reconstruction is performed on a computer that is directly embedded into an MR scanner, and the problem of optimizing or scaling image reconstruction alone is a very interesting one.

> If you are familiar with Fourier Transforms, but the concept of “spatial frequency” still seems way too mind-boggling - don’t worry, it could be. However, image analysis through FTs is a common technique in image processing. I have a link to some good materials on the subject later which will hopefully help you understand this concept a bit better if you are interested.

Similar to a CT scan, multiple slices imaged with a pre-set spatial interval through a human body are combined to obtain the 3D image. Note, however, that due to greater control over the electromagnetic fields, MR scanners can obtain data directly for a 3D volume in a single “sweep”, without having to go slice-by-slice.

## Pulse Sequences {#pulse-sequences}

We can vary the combination of gradient fields, RF pulses, and aspects of the signal that is getting measured. Together, these are called a**pulse sequence**.

Two very common sequences are called “T1-weighted” and “T2-weighted” sequences \(technically - these two are looking at different aspects of the same combination of electromagnetic fields\). T1 produces greater contrast resolution for fat, and T2 produces greater detail in fluids. Quite often, a contrast medium is used along with a T1 sequence to make certain structures stand out. Thus, the gadolinium agent is often used in neuroradiology to improve the visibility of things like tumors and hemorrhages.

Many more sequences exist, including many which are proprietary to scanner manufacturers, and there is a field of medical physics that deals exclusively with pulse sequence design.

### New Vocabulary {#new-vocabulary}

* **K-space data**: “raw” data generated by an MRI scanner. Images need to be reconstructed from it

As you have seen, MRI scanners are probably some of the most complex inventions made by humankind. The design of one touches on hard problems in the fields of robotics, quantum physics, signal processing, mechanical, electrical, and software engineering.

MRI scanners are very versatile machines that can provide great non-invasive insight into what is happening inside a human body, and they lend themselves to a lot of interesting AI problems.

# Further Resources {#further-resources}

* Facebook’s research on using AI to accelerate the MRI reconstruction process:[Overview](https://ai.facebook.com/blog/accelerating-mri-reconstruction/)and a paper about[fastMRI: An Open Dataset and Benchmarks for Accelerated MRI](https://arxiv.org/pdf/1811.08839.pdf).

* If you want to understand details of MR physics better, here is a paper on the subject, oriented at clinicians:[Ridgway, J. P. \(2010\). Cardiovascular magnetic resonance physics for clinicians: part I. Journal of Cardiovascular Magnetic Resonance, 12\(1\). doi: 10.1186/1532-429x-12-71](https://www.researchgate.net/publication/49645994_Cardiovascular_magnetic_resonance_physics_for_clinicians_Part_I).

* If you would like to understand the process of MRI image reconstruction a bit better, here is an excellent in-depth overview:[Hansen, M. S., & Kellman, P. \(2014\). Image reconstruction: An overview for clinicians. Journal of Magnetic Resonance Imaging, 41\(3\), 573–585. doi: 10.1002/jmri.24687](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4276738/).

* Finally, if you want to try and wrap your head around spatial frequency decomposition for image analysis, here is a good overview from the University of New Mexico:[https://www.cs.unm.edu/~brayer/vision/fourier.html](https://www.cs.unm.edu/~brayer/vision/fourier.html)

As you have seen,**windowing**is a fairly straightforward color mapping procedure. An interesting thing to keep in mind about windowing is that historically we have been mapping linear scales in which scanners acquire data to the linear grayscale which has the limitation of being only able to display 255 colors. People have been coming up with ideas on how to circumvent this limitation.

One such idea was building special medical monitors that use more than 8 bits per single color channel or are optimized specifically for grayscale representations \(e.g., look up Barco monitors that have 10- and 12-bit grayscale technology\). If you think that HDR TVs are new, think again - the medical imaging industry has been building those for decades!

Another idea would be trying to use all colors of the display or somehow optimizing the representation to fit the grayscale range better. We will post a link to some interesting experiments in this area in the next lesson, once you’ve had the chance to learn how medical images are stored.

### New Vocab {#new-vocab}

* **Windowing**: mapping high dynamic range of medical images onto the screen-space gray color scale
* **MPR**: multi-planar reconstruction - extraction of non-primary imaging planes from a 3D volume

![](/assets/Screenshot 2020-05-27 at 8.53.22 AM.png)

![](/assets/Screenshot 2020-05-27 at 8.55.45 AM.png)

As you have seen,**3D reconstruction**is creating a 3D image or individual voxels using 2D slices. And we learned about the 2 major methods to do this, either surface meshes or volume meshes. Most 3D medical images will use volume meshes.

### New Vocab {#new-vocab}

* **3D reconstruction**: constructing a 3D model from multiple slices of 3D medical imaging data



