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



