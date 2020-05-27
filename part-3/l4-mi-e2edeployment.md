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

* The presented scenario of**incidental findings**deals with an interesting phenomenon of_selective attention_where humans tend to ignore certain stimuli when multiple are applied. Thus, even trained observers may ignore something otherwise quite obvious, like an adrenal cyst when they know that image was taken with the purpose of evaluating potential vertebral disc degeneration. The famous “[gorilla study](https://www.npr.org/sections/health-shots/2013/02/11/171409656/why-even-radiologists-can-miss-a-gorilla-hiding-in-plain-sight)” represents this marvelously.

> **Note**: when choosing a medical imaging problem to be solved by machine learning, it is tempting to assume that automated detection of certain conditions would be the most valuable thing to solve. However, this is not usually the case. Quite often detecting if a condition is present is not so difficult for a human observer who is already looking for such a condition. Things that bring most value usually lie in the area of productivity increase. Helping prioritize the more important exams, helping focus the attention of a human reader on small things or speed up tedious tasks usually is much more valuable. Therefore it is important to understand the clinical use case that the algorithm will be used well and think of end-user value first.

When it comes to classification and object detection problems, the key to solving those is identifying relevant features in the images, or_feature extraction_. Not so long ago, machine learning methods relied on manual feature design. With the advent of CNNs, feature extraction is done automatically by the network, and the job of a machine learning engineer is to define the general shape of such features. As the name implies, features in Convolutional Neural Networks take the shape of_convolutions_. In the next section, let’s take a closer look at some of the types of convolutions that are used for 3D medical image analysis.

