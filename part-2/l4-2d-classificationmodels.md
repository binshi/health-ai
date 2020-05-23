### ML vs. DL {#ml-vs-dl}

The biggest difference between ML and DL is the concept of**feature selection.**Classical machine learning algorithms require predefined features in images. And, it takes up a lot of time and effort to design features. When deep learning came along, it was so groundbreaking because it worked to discover important features, taking this burden off of the algorithm researchers.

#### Ostu's method {#ostu-s-method}

It’s often used for background extraction and classification. It takes the intensity distribution of an image and searches it to find the intensity threshold that minimizes the variance in each of the two classes. Once it discovers that threshold, it considers every pixel on one side of that image to be one class and on the other side to be another class.

#### Convolutional neural network \(CNN\) {#convolutional-neural-network-cnn-}

There are several sets of_convolutional layers_in a CNN model. Each layer is made up of a set of_filters_that are looking for features. Layers that come early in a CNN model look for very simple features such as directional lines and layers that come later look for complex features such as shapes.

Note that the input image size must match the size of the_first_set of convolutional layers.

#### U-Net {#u-net}

U-Net is used for_segmentation_problems and it is more commonly used in 3D medical imaging. It's important to note that a limitation of 2D imaging is the inability to measure the_volume_of structures. 2D medical imaging only measures the area with respect to the angle of the image taken, which limits its utility in segmenting the whole area.

**L4-08 notebook**

First, I used Otsu’s method to extract background pixels from all of my images using a threshold intensity value of 50. This allowed me to look at the_intensity distributions_for breast tissue ONLY in fatty tissues or ONLY in dense tissue. I then used`scipy.stats.mode`to identify the mode \(peak\) of each type of tissue’s intensity distribution.

Here, fatty tissue had a peak at 140 and dense tissue had a peak at 176.

I then looped through all of the fatty/dense images and again using Otsu’s method with an intensity threshold of 50 to extract background. I then calculated_how far_each image’s peak was from the peaks of the fatty and dense tissue distributions. Finally, which difference is_smaller_determines what type of tissue my image most likely is.

