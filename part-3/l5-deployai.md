# Lesson Summary {#lesson-summary}

In our final lesson, we will focus on what it takes to build an AI algorithm like some of those we talked about in the previous lesson into a real-world system. We will discuss the following:

* Basics of **DICOM networking**
* How **hospital networks **operate and where would AI algorithms fit in
* Requirements for integration of AI algorithms
* **Tools for simulating and debugging **clinical environments:
  * scripting via DCMTK
  * OHIF - the zero-footprint medical image viewer
  * A deeper-dive into Slicer for annotation
* **Medical imaging viewer as radiologist’s tool**
  * Mazen will show us how radiologists interact with viewers
* **Medical Device Regulations **with the US FDA as an example. We will talk about the regulation of medical devices by the government and how it applies to AI algorithms.
* Data privacy, HIPAA, and anonymization.

The content of this lesson is meant to give you an idea of what it takes to bring an AI system into the real world. This is where the job of data scientist typically ends and collaboration of cross-disciplinary teams is involved. However, your activities as an AI engineer should be informed by what happens with the system in the real world, and likewise, there are aspects of operation and deployment where the input of an AI engineer is instrumental. Here we try to help you understand what kinds of things to pay attention to.

When it comes to moving medical images around the hospital, the DICOM standard comes to the rescue. Alongside the definition of the format for storing images and metadata \(which we have looked at in detail in previous lessons, it defines the networking protocol for moving the images around.

In the following few concepts, we will be talking about DICOM networking services, which are defined in[Part 7 and Part 8 of the standard](http://dicom.nema.org/medical/dicom/2020a/output/chtml/part07/PS3.7.html).

To summarize, here are the key things to remember about DICOM networking:

1. There are two types of DICOM networking: DIMSE \(DICOM Message Service Element\) and DICOMWeb. The former is designed to support data exchange in protected clinical networks that are largely isolated from the Internet. The latter is a set of RESTful APIs \([link to the Standard](https://www.dicomstandard.org/dicomweb/)\) that are designed to communicate over the Internet. DIMSE networking does not have a notion of authentication and is prevalent inside hospitals.
2. DICOM DIMSE networking defines how DICOM \_Application Entities \_talk to each other on protected networks
3. DICOM Application Entities that talk to each other take on roles of **S**ervice **C**lass **U**sers and **S**ervice **C**lass **P**roviders.
4. SCPs typical respond to requests and SCUs issue them
5. Full list of DIMSE services could be found in the [Part 7 of the DICOM Standard](http://dicom.nema.org/medical/dicom/current/output/chtml/part07/sect_7.5.html#sect_7.5.1), ones that you are most likely run into are:
   * C-Echo - “DICOM ping” - checks if the other party can speak DICOM
   * C-Store - request to store an instance
6. An Application Entity is defined by three parameters:
   * Port
   * IP Address
   * Application Entity Title \(AET\) - an alphanumeric string

Clinical networking is an industry on its own and an AI engineer will probably be exposed to a very small subset of that. So, it’s important to understand the basics - what are the systems that are important to a clinical network and how do they all fit together.

![](/assets/Screenshot 2020-05-28 at 7.55.36 AM.png)![](/assets/Screenshot 2020-05-28 at 7.56.15 AM.png)

# New Vocabulary {#new-vocabulary}

**PACS**- Picture Archiving and Communication System. An archive for medical images. A PACS product typically also includes “diagnostic workstations” - software for radiologists that is used for viewing and reporting on medical images.  
**VNA**- Vendor Neutral Archive. A PACS that is not tied to a particular equipment manufacturer. A newer generation of PACS. Often deployed in a cloud environment.  
**EHR**- Electronic Health Record. A system that stores clinical and administrative information about the patients. If you’ve been to a doctor’s office where they would pull your information on a computer screen and type up the information - it is an EHR system that they are interacting with. EHR system typically interfaces with all other data systems in the hospital and serves as a hub for all patient information. You may also see the acronym “EMR”, which typically refers to the electronic medical records stored by the EHR systems.  
**RIS**- Radiology Information System. Think of those as “mini-EHRs” for radiology departments. These systems hold patient data, but they are primarily used to schedule patient visits and manage certain administrative tasks like ordering and billing. RIS typically interacts with both PACS and EHR.

In addition to DICOM protocol there are two more \(among many\) that you might run into:

**HL7**- Health Level 7. A protocol used to exchange patient data between systems as well as data about physician orders \(lab tests, imaging exams\)  
**FHIR**- Fast Healthcare Interoperability Resources. Another protocol for healthcare data exchange. HL7 dates back to the '80s and many design decisions of this protocol start showing their age. You can think of FHIR as the new generation of HL7 built for the open web.

## Requirements for Integration of AI Algorithms

When you start thinking of deploying your AI algorithms, you will want to set some requirements as to data that is sent to these algorithms, and the environment they operate in. When defining those, you may want to think of the following:

* Series selection. As we’ve seen, modalities typically use C-STORE requests to send entire studies. How are you going to identify images/series that your algorithms will process?
* Imaging protocols. There are lots of ways images can be acquired - we’ve talked about MR pulse sequences, and there are just physiological parameters, like contrast media or FoV. How do you make sure that your algorithm processes images that are consistent with what it has been trained on?
* Workflow disruptions. If the algorithm introduces something new into the radiologists' workflow - how is this interaction going to happen?
* Interfaces with existing systems. If your algorithm produces an output - where does it go? What should the systems processing your algorithm’s output be capable of doing?

## Clinical Network Architecture Summary

In this section, we have discussed some of the basics of the DICOM networking, including services such as C-ECHO and C-STORE. We looked at how clinical networks are built and what actors are there. We considered possible integration points for AI algorithms and some of the requirements to keep in mind when integrating the AI tools.

## Further Resources {#further-resources}

* As usual, DICOM standard is a great reference for DICOM networking:
  [http://dicom.nema.org/medical/dicom/2020a/output/chtml/part07/sect\_7.5.html\#sect\_7.5.1](http://dicom.nema.org/medical/dicom/2020a/output/chtml/part07/sect_7.5.html#sect_7.5.1)
* This paper from NIH offers \(a bit dated, but still very valid\) overview of the networking portion of DICOM:
  [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC61235/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC61235/)
* This book has a comprehensive overview of the standard and is a must-have for any deep DICOM development: Digital Imaging and Communications in Medicine \(DICOM\). A Practical Introduction and Survival Guide. Authors: Pianykh, Oleg S.
  [http://www.springer.com/us/book/9783642108495](http://www.springer.com/us/book/9783642108495)

Here, we used some of the tools from the[DCMTK toolkit](https://dcmtk.org/)to emulate the operation of a DICOM network. We have done the following:

* Used `dcmdump`tool to view the DICOM metadata of a DICOM file
* Used `storecsp`command to bring up an SCP listening to incoming C-STORE requests. The following is the command line that we used: `storescp 109 -v -aet TESTSCP -od . --sort-on-study-uid st`. This starts listening on port 109, with verbose logging, with AE Title “TESTSCP”, putting all incoming files into the current directory and organizing studies into directories named as study instance UIDs, with prefix _st_
* Used `echoscu`command to verify that our SCP instance is alive and listening by running the following on the command line: `echoscu localhost 109 -v`
* Used `storescu`command to issue a set of C-STORE requests to our SCP and send several DICOM studies. We used the following command to accomplish this:`storescu localhost 109 -v -aec TESTSCU +r +sd .`. Here, `-aec`
  parameter specifies the AE title that our SCU will use to identify itself \(some SCPs might only receive data from known AE titles\); `+r`parameter tells our tool to process directories recursively and `+sd`parameter specifies a directory to send.

## Tools of the Trade - Radiologists' Tools

We have observed the use of a clinical-grade medical image viewer system used by a clinician to analyze a radiological study. Note the following things that happened in this walkthrough:

* Inspection of several MR pulse sequences side-by-side, drawing upon the fact that tissues present differently in different types of series
* Measurement of a lesion was taken. Note how exactly the measurement was done - through taking two linear measurements of the largest diameters. This is a fairly standard way of taking measurements of lesions
* Image inspection tools were used - zoom, pan and window center/width adjustments
* The analysis ended with a “report” - a formal description of important findings on the image. Such a report would be converted into text, stored in EHR over HL7 and presented to the ordering physician when they review the results of the test

Note some challenges that were immediately seen as well:

* It is not that easy or practical to measure volumes of tumors. Such measurement was substituted by several linear measurements
* I was looking for a change of the tumor over time, but it was not really easy to see what exactly changed as images belonged to different studies and were not exactly matching up, spatially
* Windowing is used to map the high dynamic range of medical imaging to the grayscale range of your monitor, and clinicians use it to focus on the specific anatomy.

OHIF, Open Health Imaging Foundation is an organization supported by both academic and commercial collaborators, which aims to deliver an open-source framework for building modern \(as of 2020\) web-based medical imaging applications.

As you have probably noticed throughout this lesson, a lot of imaging IT technologies have been created back in the '70s and '80s in the pre-Internet, pre-UI era. This legacy lives on and it is not uncommon these days to find widely used clinical medical imaging applications that have the UX language of the '90s. OHIF set out to take an open-source project for Javascript-based medical image interactions, called Cornerstone and build a state-of-the-art responsive web application on it, using the latest and greatest in web development. A lot of viewers that have been commercialized by many recent AI startups are based on Cornerstone or OHIF.

If you are looking to prototype a UX solution for your AI technology, this would be a solid choice.

OHIF website:[http://ohif.org/](http://ohif.org/)  
Cornerstone GitHub repository:[https://github.com/cornerstonejs/cornerstone](https://github.com/cornerstonejs/cornerstone)

## Tools of the Trade - Viewers - 3D Slicer

So far, you should be familiar with 3D Slicer - we have talked about this tool early on and maybe you even used it to view some of the images. Here, we will take a deeper dive, look at some features in more detail and particularly focus on segmentation.

Here, we took a closer look at how to work with segmentation masks in Slicer, walking you through loading a NIFTI volume and corresponding mask, displaying one on top of the other, and creating your own segmentation mask.

## Segmentation formats {#segmentation-formats}

Let me take a bit of a sidetrack here and say a few words on formats for storing segmentations since this is how your segmentation ground truth data may come in and this is what you would be using a Slicer-like tool with. There are a few that are commonly used:

* **NIFTI**, which you are already familiar with, allows you to define what essentially is a scalar field - every point in some rectangular subset of a 3D space has a value \(intensity\) associated with it. Thus, a segmentation mask could be stored in NIFTI by using “one-hot” encoding, as we’ve seen in the machine learning lesson. Such a mask would assign one class label to all voxels inside the structure and another one outside. Due to convenience, NIFTI masks are very widespread in the ML community.

* **DICOM RT**is a DICOM IOD for “[Radiation Therapy Structure Set](http://dicom.nema.org/medical/Dicom/2020a/output/chtml/part03/sect_A.19.3.html#table_A.19.3-1)”. We had mentioned radiation therapy in this course before - it is the treatment of cancers with radiation, and it relies on accurate mapping of the human anatomy which serves as an input into the radiation machine \(typically called linac\). The DICOM standard has several separate IODs that are specific just to radiation therapy space and one such IOD is the RT Structure Set, which is designed to store contours of the human anatomy, which will be used to target radiation. DICOM RT, unlike NIFTI, stores information about contours, i.e., curves within given slices, to define where structure boundaries are.

* **DICOM Segmentation**is[another DICOM IOD](http://dicom.nema.org/medical/Dicom/2020a/output/chtml/part03/sect_A.51.3.html)for segmentations. It is specifically used for storing structure delineations for general purpose use, and this one is more similar to NIFTI in that segmentation masks are stored allocating a class to every voxel.

A couple of other notable formats which are not specific to medical imaging, but are still sometimes used are:

* [NRRD](https://en.wikipedia.org/wiki/Nrrd) - generic format for storing multidimensional raster data, and
* [HDF5](https://en.wikipedia.org/wiki/Hierarchical_Data_Format) - format for storing hierarchical multimodal data \(including multidimensional raster data, like segmentations\)

Hopefully, this section gave you a better understanding of some of the tools that you might be integrating with or using in your endeavors. In the next and final section, we will touch on the regulatory landscape that you will likely be dealing with, and for now, allow me to link to some resources.

# Further Resources {#further-resources}

* DCMTK - the swiss-army-knife for DICOM debugging: [https://dcmtk.org/dcmtk.php.en](https://dcmtk.org/dcmtk.php.en)
* Cornerstone - the open-source Javascript framework for viewing medical images: [https://github.com/cornerstonejs/cornerstone](https://github.com/cornerstonejs/cornerstone)
* OHIF - the open-source radiological image viewer: [http://ohif.org/](http://ohif.org/)
* Orthanc \([https://www.orthanc-server.com/](https://www.orthanc-server.com/)\) is a tool that we have not really discussed in the lesson, but will use in the final project. Orthanc is a free open-source implementation of a medical imaging archive that provides many features similar to a clinical PACS when it comes to storage
* Radiant \([https://www.radiantviewer.com/](https://www.radiantviewer.com/)\) is another freeware viewer that has been used by Mazen in the clinical viewers video

## Regulatory Landscape

In this final section, I will cover the basics of some of the regulatory standards that control the use of software in medicine. This video introduced the concepts of a “_medical device_”, which is central to many regulatory systems across the world. This is the concept that regulatory agencies use to draw the line between software that has to comply with regulations and the one that does not.

Here is how the US Foods and Drugs Administration \(the main government regulatory body in all things healthcare\)[defines a medical device](https://www.fda.gov/medical-devices/classify-your-medical-device/how-determine-if-your-product-medical-device):

> ... An instrument, apparatus, implement, machine, contrivance, implant, in vitro reagent, or other similar or related article, including a component part, or accessory which is:
>
> 1. recognized in the official National Formulary, or the United States Pharmacopoeia, or any supplement to them,
> 2. intended for use in the diagnosis of disease or other conditions, or in the cure, mitigation, treatment, or prevention of disease, in man or other animals, or
> 3. intended to affect the structure or any function of the body of man or other animals, and which does not achieve its primary intended purposes through chemical action within or on the body of man or other animals …
>
>    For comparison, this is how  
>    [European Medical Device Regulation \(MDR\)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.2017.117.01.0001.01.ENG&toc=OJ:L:2017:117:TOC)  
>    defines the term:
>
> ‘medical device’ means any instrument, apparatus, appliance, software, implant, reagent, material or other article intended by the manufacturer to be used, alone or in combination, for human beings for one or more of the following specific medical purposes:
>
> * diagnosis, prevention, monitoring, prediction, prognosis, treatment or alleviation of disease,
> * diagnosis, monitoring, treatment, alleviation of, or compensation for, an injury or disability,
> * investigation, replacement or modification of the anatomy or of a physiological or pathological process or state,
> * providing information by means of in vitro examination of specimens derived from the human body, including organ, blood and tissue donations,

You can see the pattern here - something that is built with the purpose of diagnosing, preventing, or treating the disease is potentially a medical device that should conform with certain standards of safety and engineering rigor. The degree of this rigor that the regulatory bodies require depends on the risk class of the said device. Thus, for a device with lesser risk class \(like a sterile bandage\) often it is sufficient to just notify the respective regulatory body that device is being launched into the market while with high-risk devices \(like an implantable defibrillator\) there are requirements to clinical testing and engineering practices.

While “medical device” may sound like something that does not have much to do with software, the regulatory bodies actually include software in this notion as well, often operating with concepts of “software-as-a-medical-device”. Sometimes a distinction is made between “medical device with embedded software” \(like a CT scanner\) or “software-only medical device” \(like a PACS\).

Key thing that determines whether something is a medical device or not, and what class it is, is its ”intended use”. The same device may have different risk classes \(or not qualify as a medical device at all\) depending on what use you have in mind for it. A key takeaway here is that the presence of an “AI algorithm” in a system that is used by clinicians does not automatically make it a medical device. You need to articulate the intended use of the system before you try to find out what the regulatory situation is for your product.

A couple of additional notes:

* Further in this section, I will be using USA regulations as an example, just to keep my examples simple. For fundamental things, regulations across all countries are quite similar.
* This section is not legal advice - it is meant to provide a general understanding of the process and the place of an AI engineer in it. If you are seeking to classify your medical device or prepare a 510\(k\) package, you could use this section for reference, but you should consult a trained professional to evaluate legal risks and determine the correct regulatory strategy

Regulatory process typically involves two big steps:

1. Submitting a document package - called “510\(k\)” for Class II medical devices or “PMA” for Class III devices. This document package needs to include engineering artifacts providing evidence that you have followed the process and your process resulted in certain deliverables. For example, a PMA package has to include things like “Design Review notes” or “Software Verification plans”.
2. Establishing a Quality Management System. This system is a set of processes that are designed to ensure that you maintain a level of quality in your engineering and operations that is commensurate with the risk that your device presents to patients and operators. For example, the QMS might define the need for a “post-launch surveillance” process that would ensure that you keep track of usage of the device in the field and have a feedback mechanism that has you reviewing potential risks that have been realized in the field and responding to them.

The former communicates your intent to launch a product to the regulatory body, and the FDA would review your documentation package to ensure that you have followed the prescribed procedures while developing it. The latter establishes certain engineering processes.

Note that the FDA or other agencies do not actually tell you\_what\_exactly do you have to produce. The rules are designed to ensure that you have the right process. It is up to you to decide how to apply this process to what you are doing.

An aspect of a QMS that is probably the most relevant to an AI engineer is the_validation process_. A QMS might define the need to perform product validation before you release a new version of a product, which means that you need to provide evidence that your software indeed performs. If the product has an AI component at its heart, you may need to provide input along the following lines:

* What is the intended use of the product?
* How was the training data collected?
* How did you label your training data?
* How was the performance of the algorithm measured and how was the real-world performance estimated?
* What data will the algorithm perform well in the real world and what data it might not perform well on?

As the owner of an AI algorithm, you would be best positioned to answer these questions and your input would be instrumental.

# Further Resources {#further-resources}

* FDA actually [publishes all clearances](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfPMN/pmn.cfm) that it issues, so you can take a look at how people described their medical devices and what the intended use was. For example, here is Arterys’ submission for their Medical Imaging Cloud AI Platform:
  [https://www.accessdata.fda.gov/cdrh\_docs/pdf19/K192437.pdf](https://www.accessdata.fda.gov/cdrh_docs/pdf19/K192437.pdf)
* FDA’s Quality Management System requirements are available online as [21 CFR Part 820](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfCFR/CFRSearch.cfm?CFRPart=820). European and Canadian regulators require compliance with [ISO 13485 standard](https://www.iso.org/standard/59752.html) \(which is not publicly available\)

## Regulatory Landscape: HIPAA, Anonymization

Privacy laws vary from one country to another, but most of them are similar to HIPAA \(USA\) and GDPR \(Europe\).

If you want to learn more about it, there is more detail in the "EHR Data" course in this nanodegree. Consider this a bit of a recap with a focus on applying de-identification methods to DICOM data.

There are a lot to privacy laws, but something that has the greatest impact on an AI engineer are the requirements towards de-identifying the data that is coming in. De-identification is important because HIPAA Privacy rule \(and GDPR\) institute quite strict controls over “Protected Health Information” while at the same time[HIPAA has this to say about De-identified Health Information](https://www.hhs.gov/hipaa/for-professionals/privacy/laws-regulations/index.html?language=es):

> De-Identified Health Information. There are no restrictions on the use or disclosure of de-identified health information. De-identified health information neither identifies nor provides a reasonable basis to identify an individual. There are two ways to de-identify information; either: \(1\) a formal determination by a qualified statistician; or \(2\) the removal of specified identifiers of the individual and of the individual’s relatives, household members, and employers is required, and is adequate only if the covered entity has no actual knowledge that the remaining information could be used to identify the individual.

Essentially, a lot of restrictions and controls are removed for data that is considered de-identified. If you read this definition carefully, you will see that HIPAA suggests two methods for de-identification. Method \#2 is actually somewhat straightforward as HIPAA lists things that are considered private in[45 CFR 165.514](https://www.law.cornell.edu/cfr/text/45/164.514), and we will practice with some of these in our final exercise.

Method \#1 is worth a note here, though. You may wonder what statisticians have to do with privacy, but it might make sense if you remember that statisticians are what machine learning engineers used to be called before ML became widespread :\) On a serious note, there is more to de-identifying data than removing unique identifiers and other things that can identify a person. Thus, age alone is hardly good enough to identify a person. But what if the age of an individual is high, you know the country where they live, and you have access to additional information such as a newspaper article that interviews the longest-living person from that country who happens to have the same age as that in your “de-identified” dataset? Surely, that would be sufficient to identify that individual. This is where statistics becomes important and a prudent approach would be to prove, with statistical guarantees, the chances of re-identifying an individual based on patterns in the dataset. This is getting very close to the concept of[differential privacy](https://en.wikipedia.org/wiki/Differential_privacy)and is outside the scope of this course. However, I will post some links in the final section.

When it comes to DICOM medical images, anonymization typically boils down to cleaning out the DICOM metadata tags. However, depending on the dataset, you might also want to take a closer look at the pixels. Thus, you may see text that has been burnt directly into the images \(not common in CT and MR, but quite common in XRays\) or facial features \(as we’ve seen in some of the Slicer visualizations throughout the course\). As always, a good AI engineer will inspect the data and provide input if the data has potentially identifying characteristics.

In this lesson we have covered the following:

* Basics of DICOM networking
* How hospital networks operate and where would AI algorithms fit in
* Requirements for integration of AI algorithms
* Tools for simulating and debugging clinical environments:
  * scripting via DCMTK
  * OHIF - the zero-footprint medical image viewer
  * A deeper-dive into Slicer for annotation
* Medical imaging viewer as radiologist’s tool
* Medical Device Regulations with the US FDA as an example
* Data privacy, HIPAA, and anonymization.

Let me leave you with some useful resources here before we move on to the final project.

# Further Resources {#further-resources}

* FDA’s guidance for software validation: [https://www.fda.gov/media/73141/download](https://www.fda.gov/media/73141/download)
* A very profound analysis done by the University of Cambridge of various regulatory frameworks, as it pertains to algorithms as medical devices: [https://www.phgfoundation.org/documents/algorithms-as-medical-devices.pdf](https://www.phgfoundation.org/documents/algorithms-as-medical-devices.pdf)
* American College of Radiology has been hosting very good [webinars on all things medical imaging and AI](https://www.acr.org/Member-Resources/rfs/Journal-Club)
* An [article by American College of Radiology](https://www.acadrad.org/wp-content/uploads/2019/06/NIBIB-workshop-1.pdf) with some greatly structured thoughts on the roadmap for AI development in radiology
* A question that presents somewhat of a challenge to medical device regulators is what to do about machine learning systems that are continuously learning. As you have seen, the traditional regulatory frameworks assume a strict step-by-step process whereby evidence of device performance is collected and submitted once, and if updates are needed, it is submitted again, with 90 days or so review cycles. That doesn’t quite work well for continuously learning systems. FDA has recently
  [published some thoughts on what a framework for such systems could look like](https://www.fda.gov/media/122535/download)
  .



