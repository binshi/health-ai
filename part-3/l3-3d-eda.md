In this lesson we will cover the following:

* Basics of DICOM and NIFTI file formats
* A short introduction into viewing 3D medical images and some of the tools that you can use for viewing. This will be followed by an **exercise**
  where you will be asked to load a 2D DICOM image and convert it into PNG, applying proper windowing transform.
* Some important parameters to consider when analyzing datasets. This will be followed by an **exercise**
  where you will need to load a 3D volume and extract slices from it.
* A walkthrough of a Notebook with an approach to doing exploratory data analysis on a single 3D CT volume, and then an EDA for a dataset of multiple volumes. After that, you will be asked to do another **exercise**
  by working out an approach to an EDA of your own, curating a “dirty” DICOM volume dataset.

As you progress through the material, you may be wondering at times as to why you would need to know the intricacies of DICOM and NIFTI standards. I assure you that understanding how the standard is organized is crucial to understanding the data that you are dealing with. DICOM standard defines not only how pixels are stored, but also how the metadata is stored. A trove of valuable information is contained in DICOM metadata and this information could be used for all sorts of AI tasks - curating dataset, doing rapid QA, routing images to proper models, ensuring confidentiality compliance, etc. Also, if the content still seems too far removed, you might want to jump right into the exercises, and if they become confusing, come back to the lecture material, and use that to put you through.

# The DICOM Standard {#the-dicom-standard}

DICOM stands for “Digital Imaging and Communications in Medicine”. It is a standard that defines how medical imaging \(primarily\) data is stored and moved over the network. It’s been around since the '80s and eventual adoption of this standard by all manufacturers of medical imaging equipment has been a huge enabler for medical data interoperability and clinical research in general.

It is an open standard that is maintained by the National Electrical Manufacturers Association \(NEMA\) and is available at[http://dicom.nema.org/](http://dicom.nema.org/). The standard is updated a few times per year, as we update our views on how medical imaging data needs to be stored, and all prior versions are also available. When referencing the DICOM standard in your documentation, it is important to be clear about whether you are referencing the current or past versions of the standard. In this course, I will be referencing version 2020a of the standard.

DICOM is a vast and quite complex standard, and you don’t need to know it all unless you are developing a medical imaging modality or storage software. However, since you are very likely to get data as DICOM, it is important to know what is there, and how to look up things.

In this lesson, we will focus on the storage portion of the standard - the part that defines how the data acquired by the scanners is stored as files on filesystems, and what metadata accompanies these files.

