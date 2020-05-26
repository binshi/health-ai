This course is going to introduce you to building AI algorithms for problems that involve 3D medical images, predominantly those produced by Computed Tomography \(CT\) and Magnetic Resonance Imaging \(MRI\) scanners.

In this lesson, we will provide some context, motivation for the content which we decided to include, and discuss the structure for this course.

## Course Learning Objectives {#course-learning-objectives}

These are the learning objective for this course:

* Understand what 3D medical images are, who uses them, and for what purposes
* Perform exploratory data analysis on 3D image datasets in common formats such as DICOM and NIFTI
* Apply popular machine learning algorithms for both classification and segmentation tasks using real-world medical imaging datasets
* Learn to integrate trained models into a clinical imaging environment and troubleshoot your deployments
* Provide input into the algorithm validation process as required for field deployments

We believe that mastering those will help you get oriented in the field, and give you a great boost on the path of developing great AI systems.

## Course Concepts {#course-concepts}

We will be touching upon many concepts throughout this course, that span multiple fields. Here is a diagram that may help you build a mental picture as we dive deeper into these concepts and how they play together.

[![](https://video.udacity-data.com/topher/2020/April/5e9bf448_l0-course-overview/l0-course-overview.png "Course overview concepts")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/bfcee54c-56ee-401f-b31d-8756fbdcc710/concepts/dad24bf9-b40e-4ea3-84f2-449846b91e9d#)

The dark blue color is used for those concepts which we spend more time on, while gray ones are mentioned and are relevant, but are not discussed in detail.

Some of these may be familiar to you, and some may be new. We will try to provide links at the end of each lesson to help you explore these in further detail.

## “AI” and “AI Engineers” {#-ai-and-ai-engineers-}

In 1955 some of the fathers of modern information theory and what has become “AI” these days introduced the following definition of “artificial intelligence problem”:

> An attempt will be made to find how to make machines use language, form abstractions and concepts, solve kinds of problems now reserved for humans, and improve themselves. … For the present purpose the artificial intelligence problem is taken to be that of making a machine behave in ways that would be called intelligent if a human were so behaving.  
> [McCarthy, Shannon, Minsky et al. A proposal for the Dartmouth summer research project on artificial intelligence, 1955](http://www-formal.stanford.edu/jmc/history/dartmouth/dartmouth.html)

The summer research project proposal, which introduced this definition, is thought to have laid the foundation of the field of artificial intelligence study.

For the purpose of this course, we think of AI as an:

> Application of Machine Learning techniques to a real-world problem, improving human productivity and capability.

We would like to focus on the “real world” aspect of technology, and teach you the two sides of the implementation of AI for the purpose of 3D medical image analysis:

* the thought processes of clinicians and radiologists and what kind of ML-powered technology would be most impactful to them
* data, tools, and algorithms that need to come together to bring about a machine learning algorithm which could produce an analysis of a 3D medical volume

Our vision is that firmly grasping of both sides makes someone a good AI engineer, that is someone who understands all of the following:

* how data is produced and curated
* what kinds of machine learning algorithms and statistical models can utilize this data to solve clinical problems, and in what way
* how trained models need to be integrated into a clinical environment, and how some of the related fields apply; fields such as human-computer interaction, user experience design, regulatory frameworks, ethics, and data privacy

If you share these views and you want to get to know the field better, there is a good chance that you will find this course quite useful.

AI for 3D medical imaging is garnering a lot of attention recently because of all the advances in the deep learning space. Some of the early applications are very promising.

To get an idea of the growth of this industry, take a look at what the American College of Radiology \(ACR\) has been doing. The ACR is a US professional society that develops guidelines for the use of medical imaging technology.

Recently the ACR has been actively working with clinicians and regulators to define the use cases for AI in radiology and help the FDA find the proper regulatory frameworks for various AI techniques. Two initiatives are worth taking a look now:

* ACR has been developing a comprehensive list of use cases for AI in radiology:
  [https://www.acrdsi.org/DSI-Services/Define-AI](https://www.acrdsi.org/DSI-Services/Define-AI)
* the ACR has been keeping track of FDA cleared tools for radiological imaging:
  [https://www.acrdsi.org/DSI-Services/FDA-Cleared-AI-Algorithms](https://www.acrdsi.org/DSI-Services/FDA-Cleared-AI-Algorithms)

# What’s included? {#what-s-included-}

This course will focus on teaching you the following:

* Basics of biomedical imaging for software engineers and data scientists, complete with clinical motivation
* Application of machine learning techniques for 3D medical imaging data
* Principles of end-to-end design of medical imaging AI systems

The course is structured around an intro lesson, four content lessons, and final project as follows:

| Lesson | Content |
| :--- | :--- |
| 1 | Introduction to 3D Medical Imaging \(this lesson!\) |
| 2 | Clinical Background and Physics of 3D Medical Imaging |
| 3 | 3D medical imaging data formats and dataset analysis |
| 4 | Machine Learning for 3D medical imaging and segmentation hands-on |
| 5 | Deploying machine learning algorithms in the real world |
| Final Project | **Quantifying Hippocampal Volume for Alzheimer's Progression** You will clean up a 3D medical imaging dataset, train a segmentation model, and plug it into a functional medical imaging environment so that it automatically produces AI-powered reports for radiologists to reference. |

# What’s not included? {#what-s-not-included-}

This course will not focus on teaching the following:

* **Deep Learning fundamentals**
  - there are plenty of very deep courses on the subject. If you feel like you need to get a deeper understanding of the subject, we recommend looking for some of the Udacity courses, like the [Deep Learning Nanodegree](https://www.udacity.com/course/deep-learning-nanodegree--nd101) or some of the free resources like Stanford’s [video lectures](https://www.youtube.com/playlist?list=PL3FW7Lu3i5JvHM8ljYj-zLfQRF3EO8sYv) and [tutorial](http://ufldl.stanford.edu/tutorial/supervised/LinearRegression/). Another good resource is the [fast.ai](https://www.fast.ai/) course which has been built by Jeremy Howard who has co-founded a medical imaging AI startup, Enlitic.
* **PyTorch or any other specific libraries**
  . If you want to get a fundamental grasp of PyTorch specifically, we recommend PyTorch’s [tutorial](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) or the [free Udacity course](https://www.udacity.com/course/deep-learning-pytorch--ud188)
  . We will also use NumPy quite a lot and if you need a quick intro into that, the [official tutorial](https://docs.scipy.org/doc/numpy/user/quickstart.html) gives you everything you need to understand the code in this course.
* **High-performance/large-scale computing on massive medical imaging datasets**
  . Medical imaging datasets may become big \(we are talking hundreds of gigabytes\). Using them efficiently would tap into generic software architecture skills for building distributed systems, most likely using the major clouds. Every cloud provider offers a set of resources on the optimal use of their technologies. For example, you can take a look at
  [Microsoft’s design patterns for machine learning at scale](https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/machine-learning-at-scale)
* **Diagnostic medical imaging**
  . While you will hopefully gain some intuition required for an understanding of radiological images and clinicians' thought process better, it takes years of medical school and residency to become proficient with using 3D medical imaging for clinical purposes. [Radiopaedia](https://radiopaedia.org/) is a great resource to look at the variety of sample radiological cases and get familiar with how various conditions present in radiological studies.



