In this lesson, we will do a quick refresher on how **convolutional neural networks **operate and specifically dive into the **different types of convolutions **that underlie the operation of these networks.

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

* **Longitudinal follow up**: Measuring volumes of things and monitoring how they change over time. These methods are very valuable in, e.g., oncology for tracking slow-growing tumors.
* **Quantifying disease severity**: Quite often, it is possible to identify structures in the organism whose size correlates well with the progression of the disease. For example, the size of the hippocampus can tell clinicians about the progression of Alzheimer's disease.

* **Radiation Therapy Planning**: One of the methods of treating cancer is exposing the tumor to ionizing radiation. In order to target the radiation, an accurate plan has to be created first, and this plan requires careful delineation of all affected organs on a CT scan

* **Novel Scenarios**: Segmentation is a tedious process that is not quite often done in clinical practice. However, knowing the sizes and extents of the objects holds a lot of promise, especially when combined with other data types. Thus, the field of**radiogenomics**refers to the study of how the quantitative information obtained from radiological images can be combined with the genetic-molecular features of the organism to discover information not possible before.

A U-Net architecture has been very successful in analyzing 3D medical images and has spawned multiple offshoots. You will get a chance to get more familiar with it in the exercise that follows, but if you would like to understand the principles better, I recommend that you check out the webpage on U-net created by one of the authors of the original paper, Olaf Ronneberger:[https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/index.html](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/index.html). You will find the link to the original paper and a few materials explaining how and why this architecture works.

Now, let’s move on to the exercise where you will have a chance to train your own segmentation network.

I hope you enjoyed the exercise. You can find the solution for the**Exercise 2: Segmentation Hands On**[here](https://github.com/udacity/nd320-c3-3d-med-imaging/tree/master/3d-imaging-end-to-end-deep-learning-applications/exercises/2-segmentation-hands-on/solution). If you managed to complete it - you are officially well versed with DICOM, NIFTI, and PyTorch to begin training and designing your own neural networks for medical imaging classification, object detection, and segmentation problems! However, a few important pieces remain. For example, you have noticed that we used simple cross-entropy loss as our cost function. Is this the best cost function possible? Also, after you have your segmentation - how do you efficiently compare it to your ground truth and evaluate performance? We will talk about these things further in this lesson.

A couple of final remarks below.

# Further Resources {#further-resources}

* If you looked closer at the code for the segmentation network that you have trained in this exercise, you should have noticed the ConvTranspose2D layers and you might be wondering what those are. Remember the upsampling path in the U-net? This is how this upsampling is done. Going into details of those would be straying too far from this course, so if you are curious to learn more about transposed convolutions, how they work, and why, you can read up this blog post:
  [Up-sampling with Transposed Convolution](https://medium.com/activating-robotic-minds/up-sampling-with-transposed-convolution-9ae4f2df52d0)
  by Naoki Shibuya.
* A deep dive into the general approach to building segmentation networks:
  [Long, Jonathan et al. “Fully convolutional networks for semantic segmentation.” 2015 IEEE Conference on Computer Vision and Pattern Recognition \(CVPR\) \(2015\): 3431-3440.](https://arxiv.org/pdf/1605.06211v1.pdf)
* [A UNet page by the author, Olaf Ronneberger, with a nice video explaining its principles of operation](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/index.html)
* [UNet and overall CNN/DeepLearning content](http://deeplearning.net/tutorial/unet.html)
* [Another great explanation of UNet](https://spark-in.me/post/unet-adventures-part-one-getting-acquainted-with-unet)
* An overview of radiogenomics:
  [Bodalal, Z., Trebeschi, S., Nguyen-Kim, T.D.L. et al. Radiogenomics: bridging imaging and genomics. Abdom Radiol 44, 1960–1984 \(2019\). https://doi.org/10.1007/s00261-019-02028-w](https://link.springer.com/article/10.1007/s00261-019-02028-w)

I hope you enjoyed the exercise. You can find the solution for the**Exercise 2: Segmentation Hands On**[here](https://github.com/udacity/nd320-c3-3d-med-imaging/tree/master/3d-imaging-end-to-end-deep-learning-applications/exercises/2-segmentation-hands-on/solution). If you managed to complete it - you are officially well versed with DICOM, NIFTI, and PyTorch to begin training and designing your own neural networks for medical imaging classification, object detection, and segmentation problems! However, a few important pieces remain. For example, you have noticed that we used simple cross-entropy loss as our cost function. Is this the best cost function possible? Also, after you have your segmentation - how do you efficiently compare it to your ground truth and evaluate performance? We will talk about these things further in this lesson.

Some of the challenges in creating the ground truth for segmentation have to do with the fact that it is rarely routinely created in clinical practice. Radiation oncology is one of the few fields where segmentation is generated as part of the treatment path, but normally segmentation projects require custom labeling efforts.

One of the things to keep in mind when dealing with a labeled \(segmented\) dataset is that interpretation of radiological images is ambiguous and quite often, two independent clinicians \(observers\) would not label things in the same way. This phenomenon is called Interobserver Variability and has been studied in the literature.

* This is the paper that I have mentioned where the authors present results of measuring the variability between radiation oncologists segmenting structures in the head and neck region:
  [Mukesh, M et al. “Interobserver variation in clinical target volume and organs at risk segmentation in post-parotidectomy radiotherapy: can segmentation protocols help?.” The British journal of radiology vol. 85,1016 \(2012\): e530-6. doi:10.1259/bjr/66693547](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3587102/)
* While I worked on Microsoft’s Project InnerEye we also did our own IOV study for how conformant people are in contouring pelvic anatomy in prostate cancer patients, and included it into our paper which you can read here:
  [Macomber, M. W., Phillips, M., Tarapov, I., Jena, R., Nori, A., Carter, D., … Nyflot, M. J. \(2018\). Autosegmentation of prostate anatomy for radiation treatment planning using deep decision forests of radiomic features. Physics in Medicine & Biology, 63\(23\), 235002. doi: 10.1088/1361-6560/aaeaa4](https://www.researchgate.net/publication/328485616_Autosegmentation_of_prostate_anatomy_for_radiation_treatment_planning_using_deep_decision_forests_of_radiomic_features)

When it comes to tooling for creating ground truth,[3D Slicer](https://www.slicer.org/)is a popular free tool used in the research community, and I will walk you through using it for creation and review of segmentation labels in the next lessons.[MITK](http://www.mitk.org/wiki/MITK)is another one. However, many medical imaging startups and larger companies use tools of their own.

![](/assets/Screenshot 2020-05-27 at 8.26.44 PM.png)![](/assets/Screenshot 2020-05-27 at 8.27.27 PM.png)![](/assets/Screenshot 2020-05-27 at 8.28.11 PM.png)![](/assets/Screenshot 2020-05-27 at 8.29.11 PM.png)![](/assets/Screenshot 2020-05-27 at 8.31.25 PM.png)

We have discussed four metrics that you can use to evaluate the performance of your segmentation models. As usual, a great explanation of these can also be found on Wikipedia which I’m linking here if you are looking for additional details:

* [Sensitivity and Specificity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)
* [Dice Similarity Coefficient](https://en.wikipedia.org/wiki/Sørensen–Dice_coefficient)
* [Jaccard Index](https://en.wikipedia.org/wiki/Jaccard_index)
* [Hausdorff Distance](https://en.wikipedia.org/wiki/Hausdorff_distance)

Note these metrics as they are very handy as you are publishing your model’s validation reports, but also they could be used to construct more elaborate cost functions.

# Evaluating Performance as a Clinician {#evaluating-performance-as-a-clinician}

Below, Mazen will present a clinician’s perspective on assessing the performance of assistive systems in general, not only segmentation models. It is important to understand this perspective for a data scientist so that you can speak with clinicians in common terms.

As a clinician, I need to make decisions about the presence of conditions or selecting the course of treatment. For that, clinicians operate in terms of Likelihood Ratios.

The likelihood ratio for a diagnostic test result can be calculated if the predictive characteristics \(sensitivity and specificity\) of that test are known. Likelihood ratios are known for common diagnostic tests performed by humans \(e.g., correctly identifying viral pneumonia from chest CT scans\). This means that for example, your ML segmentation algorithm may be measuring the volume of a specific anomaly in the lung very accurately, but this measurement, while important to quantify the degree of lung involvement by some disease state, may be not specific at all for predicting whether that state is due to a viral pneumonia \(e.g., presence of such anomalies could mean viral pneumonia, bacterial pneumonia or non-infectious causes like hemorrhage or edema\). Thus, your algorithm with high Dice scores may end up being not very useful to solve a clinical task if the goal is a specific diagnosis.



The skills that you have learned through watching videos and doing exercises will help you in the final project where you will build your own AI system using a U-net implementation, and apply it to DICOM datasets.

In this lesson, we have covered the following:

* A quick refresher on how convolutional neural networks operate and a took a closer look at the different types of convolutions that underlie the operation of these networks.
* Ways to approach segmentation and classification problems for 3D medical imaging
* We did an exercise where we trained our own segmentation network on a medical imaging dataset
* Technical methods for evaluating performance of CNNs for 3D medical image analysis, and talked about the clinical aspect of evaluating performance.

Before we are ready to implement the full-scale AI solution in the final project, there is one final set of concepts that I want you to get familiar with - how to integrate such algorithms into real-world systems, and what these real-world systems look like. This would be the topic of our next lesson.

# Further Resources {#further-resources}

## More problems {#more-problems}

As mentioned in my closing remarks, machine learning problems in 3D medical imaging do not boil down to only classification and segmentation. The two problems we’ve looked at here help you understand the principles, but there is so much more you can do. Here are some pointers for some amazing things people do with deep neural networks in 3D medical imaging:

* Using deep learning to increase the resolution of low-res scans:
  [Chaudhari AS, Fang Z, Kogan F, et al. Super-resolution musculoskeletal MRI using deep learning. Magn Reson Med. 2018;80\(5\):2139–2154. doi:10.1002/mrm.27178](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6107420/)
* GANs for synthetic MRI:
  [Frid-Adar, M., Diamant, I., Klang, E., Amitai, M., Goldberger, J., & Greenspan, H. \(2018\). GAN-based synthetic medical image augmentation for increased CNN performance in liver lesion classification. Neurocomputing, 321, 321–331. doi: 10.1016/j.neucom.2018.09.013](https://arxiv.org/pdf/1803.01229.pdf)
* A survey of deep learning methods for medical image registration:
  [Haskins, G., Kruger, U. & Yan, P. Deep learning in medical image registration: a survey. Machine Vision and Applications 31, 8 \(2020\). https://doi.org/10.1007/s00138-020-01060-x](https://arxiv.org/abs/1903.02026)
* Overview of opportunities for deep learning on MRIs:
  [Lundervold, A. S., & Lundervold, A. \(2019\). An overview of deep learning in medical imaging focusing on MRI. Zeitschrift Für Medizinische Physik, 29\(2\), 102–127. doi: 10.1016/j.zemedi.2018.11.002](https://www.sciencedirect.com/science/article/pii/S0939388918301181)

## Tools and libraries {#tools-and-libraries}

We tried to minimize the dependency on external libraries and focus on understanding some key concepts. At the same time, there are many tools that the community has developed, which will help you get moving faster with the tasks typical for medical imaging ML workflows.

A few tools/repos worthy of attention are:

* Fast.ai - python library for medical image analysis, with focus on ML:
  [https://dev.fast.ai/medical.imaging](https://dev.fast.ai/medical.imaging)
* MedPy - a library for medical image processing with lots of various higher-order processing methods:
  [https://pypi.org/project/MedPy/](https://pypi.org/project/MedPy/)
* Deepmedic, a library for 3D CNNs for medical image segmentation:
  [https://github.com/deepmedic/deepmedic](https://github.com/deepmedic/deepmedic)
* Work by the German Cancer Research Institute:
  * [https://github.com/MIC-DKFZ/trixi](https://github.com/MIC-DKFZ/trixi)
    - a boilerplate for machine learning experiment
  * [https://github.com/MIC-DKFZ/batchgenerators](https://github.com/MIC-DKFZ/batchgenerators)
    - tooling for data augmentation
* A publication about a project dedicated to large-scale medical imaging ML model evaluation which includes a comprehensive overview of annotation tools and related problems \(including inter-observer variability\):
  [https://link.springer.com/chapter/10.1007%2F978-3-319-49644-3\_4](https://link.springer.com/chapter/10.1007%2F978-3-319-49644-3_4)

## Books {#books}

Some resources readily available online for free will help you grasp the basic concepts of computer vision and overall machine learning.

* [https://d2l.ai/](https://d2l.ai/)
  - deep learning with a special section on computer vision by Alexander Smola et al. Alexander has a strong history of publications on machine learning algorithms and statistical analysis and is presently serving as a director for machine learning at Amazon Web Services in Palo Alto, CA
* [http://www.mbmlbook.com/](http://www.mbmlbook.com/)
  - a book on general concepts of machine learning by Christopher Bishop et al. Christopher has a distinguished career as a machine learning scientist and presently is in charge of Microsoft Research lab in Cambridge, UK, where I had the honor to work on
  [project InnerEye](https://www.microsoft.com/en-us/research/project/medical-image-analysis/)
  for several years.

## More notable papers {#more-notable-papers}

* If you’re curious about segmentation space specifically, you may appreciate a foray into non-ML-based methods for segmentation. A couple of papers that can provide an introduction into that space are:
  * [Boykov, Y., & Jolly, M.-P. \(2000\). Interactive Organ Segmentation Using Graph Cuts. Medical Image Computing and Computer-Assisted Intervention – MICCAI 2000 Lecture Notes in Computer Science, 276–286. doi: 10.1007/978-3-540-40899-4\_28](https://cs.uwaterloo.ca/~yboykov/Papers/miccai00.pdf)
  * [Probabilistic Graphical Models for Medical Image Segmentation](https://www.researchgate.net/publication/280664591_Probabilistic_Graphical_Models_for_Medical_Image_Segmentation)
* This GitHub repo provides an excellent overview of CNN-based seg methods for general image domain:
  [https://github.com/mrgloom/awesome-semantic-segmentation](https://github.com/mrgloom/awesome-semantic-segmentation)





