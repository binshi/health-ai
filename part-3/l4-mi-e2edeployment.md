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

