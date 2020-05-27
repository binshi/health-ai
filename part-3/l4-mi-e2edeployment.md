# Lesson Summary {#lesson-summary}

* In this lesson, we will do a quick refresher on how **convolutional neural networks **operate and specifically dive into the **different types of convolutions **that underlie the operation of these networks.
* We will talk about some ways to approach **segmentation and classification **problems.
* After that, we will jump in and **train our own segmentation network**.
* Then we will discuss some of the technical methods for **evaluating performance of CNNs for medical image analysis **, and talk about the clinical aspect of evaluating performance.

But before we jump in, I would like to take a brief detour and align on the basic definitions. We will be using terms like Artificial Intelligence, Computer Vision, Deep Learning, Machine Learning, and it’s important to get on the same page with regards to how they are related. Here is a diagram that represents this:

[![](https://video.udacity-data.com/topher/2020/April/5e9bf445_l3-ai-ml-dl-cv/l3-ai-ml-dl-cv.png "\[AI\_ML\_DL\_CV.png\]
Relationship between AI, ML, Deep Learning, Computer Vision")Relationship between AI, ML, Deep Learning, Computer Vision](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/bb342b71-8ca8-4e51-8309-6333cbea25a0/concepts/52bbcc3f-f2da-4c37-89b4-32a535a2c776#)

As discussed in the introductory lesson,**Artificial Intelligence**could be defined as “_Application of Machine Learning techniques to a real-world problem, improving human productivity and capability._”. Artificial intelligence is a field that draws heavily upon the field of**Machine Learning**, which deals with building algorithms that do not have to be explicitly coded but rather can adjust parameters from data. Machine learning includes many tools and techniques for working with a vast variety of data and one of the techniques is the**Deep Learning**, which is all about building deep neural networks.

However, to improve human productivity and capability, the field of AI relies on many other fields - such as robotics, HCI, systems design, cloud computing, and so on, including bioinformatics, as discussed in the previous lessons. Developing AI products for medical imaging relies heavily on the field of**Computer Vision**, which has been around for many decades, and which studies methods for automated image analysis.

Applying deep learning techniques by using methods developed by computer vision scientists has resulted in many rapid advances, including the invention of**Convolutional Neural Networks**\(CNN\), which are the methods used for image analysis in this course.

We assume that you have basic familiarity with the principles underlying CNNs and the problems that CNNs can solve for general-purpose imaging - classification and segmentation in particular. In this lesson, we will provide a quick refresher on these two problems and will focus on how these problems are cast in the domain of 3D medical imaging.

We have seen some of the examples of problems that lend themselves well to solutions via automated classification or object detection algorithm.

* Detecting**brain hemorrhages**, or bleedings in the brain is particularly important in emergency scenarios when brain damage can happen within minutes. Often, radiologists have a backlog of images that they are going through, and it is not obvious which ones should be prioritized. An algorithm that will spot time-critical conditions will help with such prioritization

* Screening and monitoring scenarios, such as the presented scenario of**screening for lung nodules**, can be quite tedious because objects that are sought can hide well, and meticulous scrolling through slices is required. Pointing human attention to areas which are likely to be suspicious is helpful and saves time

* The presented scenario of**incidental findings**deals with an interesting phenomenon of\_selective attention\_where humans tend to ignore certain stimuli when multiple are applied. Thus, even trained observers may ignore something otherwise quite obvious, like an adrenal cyst when they know that image was taken with the purpose of evaluating potential vertebral disc degeneration. The famous “[gorilla study](https://www.npr.org/sections/health-shots/2013/02/11/171409656/why-even-radiologists-can-miss-a-gorilla-hiding-in-plain-sight)” represents this marvelously.

> **Note**: when choosing a medical imaging problem to be solved by machine learning, it is tempting to assume that automated detection of certain conditions would be the most valuable thing to solve. However, this is not usually the case. Quite often detecting if a condition is present is not so difficult for a human observer who is already looking for such a condition. Things that bring most value usually lie in the area of productivity increase. Helping prioritize the more important exams, helping focus the attention of a human reader on small things or speed up tedious tasks usually is much more valuable. Therefore it is important to understand the clinical use case that the algorithm will be used well and think of end-user value first.

When it comes to classification and object detection problems, the key to solving those is identifying relevant features in the images, or_feature extraction_. Not so long ago, machine learning methods relied on manual feature design. With the advent of CNNs, feature extraction is done automatically by the network, and the job of a machine learning engineer is to define the general shape of such features. As the name implies, features in Convolutional Neural Networks take the shape of_convolutions_. In the next section, let’s take a closer look at some of the types of convolutions that are used for 3D medical image analysis.

We have discussed some methods for feature extraction such as 2D, 2.5D, and 3D convolutions.

A convolution is an operation that applies a convolutional filter to each pixel of an image. A more detailed explanation could be found in[this great Wikipedia article](https://en.wikipedia.org/wiki/Kernel_%28image_processing)\#Convolution\).

A simple 2D convolution with a 3x3 kernel could be visualized by the following animation:

[![](https://video.udacity-data.com/topher/2020/April/5e9bf445_l3-conv/l3-conv.gif "\[conv.gif\]
Animation of image convolution")Animation of image convolution with a 3x3 convolutional filter \[\[1,0,1\],\[0,1,0\],\[1,0,1\]\]](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/7ab3170c-e20f-4a47-8425-7ba7482c0eca/modules/c2693991-fbab-4ea4-9ef2-a01b62b7a88e/lessons/bb342b71-8ca8-4e51-8309-6333cbea25a0/concepts/470859d7-57a1-4964-a7d9-152814f4fa99#)

We have just discussed several types of convolutions that could be used for feature extraction of 3D medical images:

**2D Convolution**is an operation visualized in the image above, where a convolutional filter is applied to a single 2D image. Applying a 2D convolution approach to a 3D medical image would mean applying it to every single slice of the image. A neural network can be constructed to either process slices one at a time, or to stack such convolutions into a stack of 2D feature maps. Such an approach is fastest of all and uses least memory, but fails to use any information about the topology of the image in the 3rd dimension.

**2.5D Convolution**is an approach where 2D convolutions are applied independently to areas around each voxel \(either in neighboring planes or in orthogonal planes\) and their results are summed up to form a 2D feature map. Such an approach leverages some dimensional information.

**3D Convolution**is an approach where the convolutional kernel is 3 dimensional and thus combines information from all 3 dimensions into the feature map. This approach leverages the 3-dimensional nature of the image, but uses the most memory and compute resources.

Understanding these is essential to being able to put together efficient deep neural networks where convolutions together with downsampling are used to extract higher-order semantic features from the image.

Next up, we will take a closer look at how convolutions operate by running through a notebook and performing an exercise.

Note that we did not spend too much time on the actual methods for building networks for classification and object detection, and as you will see further in the lesson, there is more focus on segmentation, especially on performance metrics and coding exercises.

The reason for that is that classification problems in 3D medical imaging can leverage a lot of techniques used for 2D image classification, and the course on AI for 2D medical image analysis, which is a part of this nanodegree, already provides an excellent deep dive into some of the approaches for classification and object detection problems. We talked about some of the differences between 2D and 3D classification problems such as 3D and 2.5D convolutions and hopefully, through our convolutions exercise, got you a feel of how these work and how you would code one yourself. But if you want to ground yourself better in applying CNNs for classification and object detection problems, I suggest going through the course on AI for 2D medical image analysis.

# Further Resources {#further-resources}

* If you think you’re lost in convolutions - check out this
  [2D Visualization of a Convolutional Neural Network](https://www.cs.ryerson.ca/~aharley/vis/conv/flat.html)
  by Adam W. Harley.
* [A guide to convolution arithmetic for deep learning](https://arxiv.org/pdf/1603.07285.pdf)
  is a great overview of the arithmetic of the various convolution operations.
* The paper I had mentioned in the slides where the authors use 2.5D convolutions for a variety of pathology classifiers:
  [H. R. Roth et al., "Improving Computer-Aided Detection Using Convolutional Neural Networks and Random View Aggregation," in IEEE Transactions on Medical Imaging, vol. 35, no. 5, pp. 1170-1181, May 2016.](https://arxiv.org/pdf/1505.03046.pdf)
* Another paper by the same authors presenting a 2.5D convolutions approach for lymph node detection:
  [Roth, Holger R., et al. “A New 2.5D Representation for Lymph Node Detection Using Random Sets of Deep Convolutional Neural Network Observations.” Medical Image Computing and Computer-Assisted Intervention – MICCAI 2014 Lecture Notes in Computer Science, 2014, pp. 520–527., doi:10.1007/978-3-319-10404-1\_65.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4295635/)
* This paper is comparing 3D and 2D network architectures for lung nodule classification problem:
  [Kang G, Liu K, Hou B, Zhang N \(2017\) 3D multi-view convolutional neural networks for lung nodule classification. PLoS ONE 12\(11\): e0188290. https://doi.org/10.1371/journal.pone.0188290](https://doi.org/10.1371/journal.pone.0188290)

# Segmentation in 3D medical imaging:

* **Longitudinal follow up**
  : Measuring volumes of things and monitoring how they change over time. These methods are very valuable in, e.g., oncology for tracking slow-growing tumors.
* **Quantifying disease severity**: Quite often, it is possible to identify structures in the organism whose size correlates well with the progression of the disease. For example, the size of the hippocampus can tell clinicians about the progression of Alzheimer's disease.

* **Radiation Therapy Planning**: One of the methods of treating cancer is exposing the tumor to ionizing radiation. In order to target the radiation, an accurate plan has to be created first, and this plan requires careful delineation of all affected organs on a CT scan

* **Novel Scenarios**: Segmentation is a tedious process that is not quite often done in clinical practice. However, knowing the sizes and extents of the objects holds a lot of promise, especially when combined with other data types. Thus, the field of**radiogenomics**refers to the study of how the quantitative information obtained from radiological images can be combined with the genetic-molecular features of the organism to discover information not possible before.



