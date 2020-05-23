### ML vs. DL {#ml-vs-dl}

The biggest difference between ML and DL is the concept of**feature selection.**Classical machine learning algorithms require predefined features in images. And, it takes up a lot of time and effort to design features. When deep learning came along, it was so groundbreaking because it worked to discover important features, taking this burden off of the algorithm researchers.

#### Ostu's method {#ostu-s-method}

It’s often used for background extraction and classification. It takes the intensity distribution of an image and searches it to find the intensity threshold that minimizes the variance in each of the two classes. Once it discovers that threshold, it considers every pixel on one side of that image to be one class and on the other side to be another class.

#### Convolutional neural network \(CNN\) {#convolutional-neural-network-cnn-}

There are several sets of\_convolutional layers\_in a CNN model. Each layer is made up of a set of\_filters\_that are looking for features. Layers that come early in a CNN model look for very simple features such as directional lines and layers that come later look for complex features such as shapes.

Note that the input image size must match the size of the\_first\_set of convolutional layers.

#### U-Net {#u-net}

U-Net is used for\_segmentation\_problems and it is more commonly used in 3D medical imaging. It's important to note that a limitation of 2D imaging is the inability to measure the\_volume\_of structures. 2D medical imaging only measures the area with respect to the angle of the image taken, which limits its utility in segmenting the whole area.

**L4-08 notebook**

First, I used Otsu’s method to extract background pixels from all of my images using a threshold intensity value of 50. This allowed me to look at the\_intensity distributions\_for breast tissue ONLY in fatty tissues or ONLY in dense tissue. I then used`scipy.stats.mode`to identify the mode \(peak\) of each type of tissue’s intensity distribution.

Here, fatty tissue had a peak at 140 and dense tissue had a peak at 176.

I then looped through all of the fatty/dense images and again using Otsu’s method with an intensity threshold of 50 to extract background. I then calculated\_how far\_each image’s peak was from the peaks of the fatty and dense tissue distributions. Finally, which difference is\_smaller\_determines what type of tissue my image most likely is.

[![](https://video.udacity-data.com/topher/2020/April/5e9b5bdf_l3-split/l3-split.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/5f4b34f1-86c8-4be5-921c-2bb2578918b7/concepts/678c028b-f57a-49b4-9248-17b587234fd4#)

### Split data using`train_test_split`with Sklearn {#split-data-using-train_test_split-with-sklearn}

[![](https://video.udacity-data.com/topher/2020/May/5eb60740_l3-code/l3-code.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/5f4b34f1-86c8-4be5-921c-2bb2578918b7/concepts/678c028b-f57a-49b4-9248-17b587234fd4#)

## Summary {#summary}

You need to split your data into two sets before feeding it into the model.

* A training set: DL algorithm will use this data to learn the features that differentiate between your classes.
* A validation set: the algorithm will
  _never_
  use this set for learning. This is the set to determine if the algorithm is actually learning to discriminate between your classes.

The general rule of thumb is to split your data 80 in the training set and 20 in the validation set. The data should be split to maximize the\_prevalence\_of positive cases \(i.e make sure 80% of your positive cases end up in the training set and 20% in the validation set\).

We want to have a\_balanced\_training set so that the model has an equal number of cases in each class to learn. Even if one class is really rare in the wild. We want to have an\_imbalanced\_validation set to reflect the real-world situation.

For all other variables in your dataset such as age, sex, and race, the distribution should follow the\_same\_distribution as your original\_full\_dataset.

**Note:**an image should NEVER be used for both training and validation.

#### Gold standard {#gold-standard}

The\_gold standard\_for a particular type of data refers to the method that detects disease with the\_highest\_sensitivity and accuracy. Any new method that is developed can be compared to this to determine its performance. The gold standard is different for different diseases.

#### Ground truth {#ground-truth}

Often times, the gold standard is unattainable for an algorithm developer. So, you still need to establish the\_ground truth\_to compare your algorithm.

Ground truths can be created in many different ways. Typical sources of ground truth are

* Biopsy-based labeling.
  **Limitations: **difficult and expensive to obtain.
* NLP extraction.
  **Limitations: **may not be accurate.
* Expert \(radiologist\) labeling.
  **Limitations: **expensive and requires a lot of time to come up with labeling protocols.
* Labeling by another state-of-the-art algorithm.
  **Limitations: **may not be accurate.

#### Silver standard {#silver-standard}

The silver standard involves hiring\_several\_radiologists to each make their own diagnosis of an image. The final diagnosis is then determined by a\_voting\_system across all of the radiologists’ labels for each image. Note, sometimes radiologists’ experience levels are taken into account and votes are weighted by years of experience.



[![](https://video.udacity-data.com/topher/2020/April/5e9b9369_l3-goals/l3-goals.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/5f4b34f1-86c8-4be5-921c-2bb2578918b7/concepts/93f5a8cb-0731-4074-b926-dbd4db994d2d#)

[![](https://video.udacity-data.com/topher/2020/April/5e9b9375_l3-augm/l3-augm.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/5f4b34f1-86c8-4be5-921c-2bb2578918b7/concepts/93f5a8cb-0731-4074-b926-dbd4db994d2d#)

[![](https://video.udacity-data.com/topher/2020/April/5e9b9381_l3-resize/l3-resize.png)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f5541bd6-560d-4ac8-b612-9db9b4420eba/modules/004715e8-0ef7-45d6-94b5-00b792a53bdd/lessons/5f4b34f1-86c8-4be5-921c-2bb2578918b7/concepts/93f5a8cb-0731-4074-b926-dbd4db994d2d#)

## Summary {#summary}

#### Intensity normalization {#intensity-normalization}

Intensity normalization is good practice and should always be done prior to using data for training. Making all of your intensity values fall within a small range that is close to zero helps the weights on our convolutional filters stay under control

There are two types of normalization that you can perform.

* zero-meaning: subtract that mean intensity value from every pixel.
* standardization: subtract the mean from each pixel and divide by the image’s standard deviation.

#### Image augmentation {#image-augmentation}

Image augmentation allows us to create different versions of the original data. Keras provides`ImageDataGenerator`package for image augmentation.

**Note:**not all image augmentation method is appropriate for medical imaging. A vertical flip should never be applied. And validation data should NEVER be augmented.

#### Image resize {#image-resize}

CNNs have an input layer that specifies the size of the image they can process. Keras`flow_from_directory`have a`target_size`parameter to resize image.

