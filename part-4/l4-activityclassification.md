In this lesson, we are going to build an activity classifier using data from an accelerometer from a wrist wearable. Activity classifiers can be useful directly in that people like to keep track of the activities they are doing over the day. But they can also be used in more clinical contexts. For example, if a company is doing a drug trial and wants to know if their drug makes study subjects more or less active, they can look at the activity classifier output and see if subjects are spending more time walking around or if they are mostly idle.

To build this activity classifier, you will learn how to featurize a high-rate time-series signal. How do you take a few seconds of a 200Hz signal -- so that’s 1000s of data points -- and reduce it to a handful of features that traditional machine learning models know how to deal with. We’ll then use a random forest model to train our activity classifier and use leave-one-subject out cross-validation to evaluate its performance. We’ll then talk about our model’s hyperparameters and do hyperparameter optimization. To be successful in any modeling task, however, we need to dive into our data and become familiar with its intricacies, so we’re going to begin with some data exploration.

## Outline {#outline}

Understanding Your Data

* Wrist PPG Dataset
* Data Exploration and Visualization Understanding The Literature
  * Feature Engineering and Extraction Modeling Performance Evaluation Hyperparameter Optimization

## Concepts {#concepts}

We will follow an algorithm development process, as outlined below.

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d10_nd320-c4-l3-algorithm-development-progress/nd320-c4-l3-algorithm-development-progress.png "Algorithm Development Process")Algorithm Development Process](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/889d707f-7014-4216-ac94-ab3c285ad0e0/concepts/b5a57872-8f44-4eaa-add9-2e173356ae37#)

Before jumping into a new domain, we first need to understand the data and the literature. For this lesson, we will be building an algorithm that first featurizes the raw high-rate IMU time series into a manageable number of data points. We can then put these features into a classical machine learning model, optimize the hyperparameters, and evaluate our performance.



The dataset we will be using comes from the[Wrist PPG During Exercise](https://physionet.org/content/wrist/1.0.0/)dataset on Physionet. Physionet is a great resource for biomedical time-series signals. It has many datasets with various signals in various disease conditions and all the data is available under an open license. The Wrist PPG dataset contains data from 8 subjects. The protocol contained four activities -- walking on a treadmill, running on a treadmill, low-intensity cycling on an exercise bike, and high-intensity cycling. While the dataset contains ECG and PPG signals as well, we will only be using the accelerometer signal for this project.

We explore the summary statistics of this dataset and look at the raw data. Like most real datasets, this one is not perfect. It is unbalanced in terms of numbers of subjects as well as the total number of data points.

[Wrist PPG Dataset](https://physionet.org/content/wrist/1.0.0/)

* This is a great blog post by [Casie Kozrykov](https://towardsdatascience.com/@kozyrkov) who taught me statistics at Google! In it, she describes the dangers of overfitting your brain when you explore your data.
* [Your dataset is a giant inkblot test](https://towardsdatascience.com/your-dataset-is-a-giant-inkblot-test-b9bf4c53eec5)
* Check out [this StackOverflow discussion](https://stats.stackexchange.com/questions/352688/is-exploratory-data-analysis-important-when-doing-purely-predictive-modeling)
  on the value of data exploration. From one of the responses:
  > Two weeks spent training a neuralnet can save you 2 hours looking at the input data
* And finally, a [blog post](https://machinelearningmastery.com/understand-problem-get-better-results-using-exploratory-data-analysis/)
  from a machine learning practitioner on the data exploration.

  


