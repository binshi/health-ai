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

# DICOM Entity-Relationship Model {#dicom-entity-relationship-model}

DICOM standard defines Information Entities that represent various real-world entities and relationships between them. The cornerstone of the DICOM standard are the following objects and relationships:

**Patient**is, naturally, the patient undergoing the imaging study. A patient object contains one or more_studies_.

**Study**- a representation of a “medical study” performed on a patient. You can think of a study as a single visit to a hospital for the purpose of taking one or more images, usually within. A Study contains one or more_series_.

**Series**- a representation of a single “acquisition sweep”. I.e., a CT scanner took multiple slices to compose a 3D image would be one image series. A set of MRI T1 images at different axial levels would also be called one image series. Series, among other things, consists of one or more_instances_.

**Instance**- \(or Image Information Entity instance\) is an entity that represents a single scan, like a 2D image that is a result of filtered backprojection from CT or reconstruction at a given level for MR. Instances contain pixel data and metadata \(Data Elements in DICOM lingo\).

There are many more entities defined by the DICOM standard, but we will focus on these for the purposes of this course. You can look up the comprehensive list in[Section A.1.2 of Part 3](http://dicom.nema.org/medical/dicom/2020a/output/html/part03.html#sect_A.1.2)of the standard.

One of the important things to capture here is that per the DICOM standard, 3D medical images are stored as files on a file system where each file represents an instance of Image DICOM Information Entity. In the case of 3D medical images, each such instance shares context with other instances that belong to the same series and same study. Thus, each DICOM file stores metadata that describes attributes of study and series that the respective instance is a part of, and this metadata is replicated across other instances that belong to the same study. DICOM files are usually stored with\_.dcm\_extension and are usually grouped in directories \(but they don’t have to be\) to represent data from series, studies and patients. Relationships between individual .dcm files are defined by the metadata stored within them.

# New Vocabulary {#new-vocabulary}

**SOP**- Service-Object Pair. DICOM standard defines the concept of an Information Object, which is the representation of a real-world persistent object, such as an MRI image \(DICOM Information Objects consist of Information Entities\). The standard also defines the concept of Services that could be performed on Information Objects. One such service is the Storage service \(we will touch on others later in the course\), and a DICOM image stored as a file on a file system is an instance of Storage service performed on an Image Information Object. Such Service-Object Pairs have unique identifiers that help unambiguously define what type of data we are dealing with. A list of SOP Classes can be found in[Part 4 of the Standard](http://dicom.nema.org/MEDICAL/DICOM/2020a/output/chtml/part04/sect_B.5.html). This list is a useful reference for all possible data types that could be stored per the DICOM standard.

**Data Element**- a DICOM metadata “field”, which is uniquely identified by a tuple of integer numbers called_group id\_and\_element id_. The convention is to write the element identifier as group id followed by the element id in parentheses like so: \(0008,0020\) - this one is the DICOM Element for Study Date. DICOM data elements are usually called “tags”. You can find the list of all possible DICOM tags in[Part 6, Chapter 6 of the standard](http://dicom.nema.org/medical/dicom/2020a/output/chtml/part06/chapter_6.html).

**VR**- Value Representation. This is the data type of a DICOM data element. DICOM standard imposes some restrictions on what form the data can take. There are short strings, long strings, integers, floats, datetime types, and more. You can find the reference for DICOM data types in[Part 5, Section 6 of the standard](http://dicom.nema.org/medical/dicom/2020a/output/chtml/part05/sect_6.2.html)

**Data Element Type**- identifiers that are used by Information Object Definitions to specify if Data Elements are mandatory, conditional or optional. Data Element Type reference can be found in[Part 5, Section 7 of the standard](http://dicom.nema.org/medical/dicom/2020a/output/html/part05.html#sect_7.4)

**IOD**- Information Object Definition. Information Object Definition specifies what metadata fields have to be in place for a DICOM Information Object to be valid. Scanner manufacturers follow the relevant parts of the DICOM standard when saving the digital data acquired by the scanner. When parsing DICOM data, it is often useful to reference the relevant IODs to see what data elements could be expected in the particular class of information objects, and what they mean. For example, in Part 3 of the standard, you can find[MR Image IOD](http://dicom.nema.org/medical/dicom/2020a/output/chtml/part03/sect_A.4.3.html)and[CT Image IOD](http://dicom.nema.org/medical/dicom/2020a/output/chtml/part03/sect_A.3.3.html)which we will use in this course quite a bit. You might have noticed that the table with all DICOM data elements does not really provide any description of what these elements mean. The reason for that is that elements may mean slightly different things depending on what Information Object Definition uses them, therefore, to find the real meaning of the element you need to look them up in the respective IOD.

Like DICOM, NIFTI, which stands for Neuroimaging Informatics Technology Initiative, is an open standard that is available at[https://nifti.nimh.nih.gov/nifti-2](https://nifti.nimh.nih.gov/nifti-2). The standard has started out as a format to store neurological imaging data and has slowly seen a larger adoption across other types of biomedical imaging fields.

Some things that distinguish NIFTI from DICOM, though are:

* NIFTI is optimized to store serial data and thus can store entire image series \(and even study\) in a single file.
* NIFTI is not generated by scanners; therefore, it does not define nearly as many data elements as DICOM does. Compared to DICOM, there are barely any, and mostly they have to do with geometric aspects of the image. Therefore, NIFTI files by themselves can not constitute a valid patient record but could be used to optimize storage, alongside some sort of patient info database.
* NIFTI files have fields that define units of measurements and while DICOM files store all dimensions in mm, it’s always a good idea to check what units of measurement are used by NIFTI.
* When addressing voxels, DICOM uses a
  [_right-handed coordinate system_](https://en.wikipedia.org/wiki/Right-hand_rule)
  for X, Y and Z axes, while NIFTI uses a
  _left-handed coordinate system_
  . Something to keep in mind, especially when mixing NIFTI and DICOM data.

# Further Resources {#further-resources}

* Background and history of the NIFTI:
  [https://nifti.nimh.nih.gov/background/](https://nifti.nimh.nih.gov/background/)
* The most “official” reference of NIFTI data fields could be found in this C header file, published on the standard page:
  [https://nifti.nimh.nih.gov/pub/dist/src/niftilib/nifti1.h](https://nifti.nimh.nih.gov/pub/dist/src/niftilib/nifti1.h)
  or on this, slightly better-organized page:
  [https://nifti.nimh.nih.gov/nifti-1/documentation/nifti1fields](https://nifti.nimh.nih.gov/nifti-1/documentation/nifti1fields)
* A great blog post on NIFTI file format:
  [https://brainder.org/2012/09/23/the-nifti-file-format/](https://brainder.org/2012/09/23/the-nifti-file-format/)



