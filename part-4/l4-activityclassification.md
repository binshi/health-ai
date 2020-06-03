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

n the previous exercise we thought about how to distinguish between the types of activities based on the data but because of the limited set of data it’s important to recognize that we’ve also just overfit ourselves to the dataset. When we did the data exploration, we looked at the entire dataset and now our brains might pick up on patterns that are only specific for that dataset. And it almost happened to me and we looked at how we might have distinguished biking from walking but there was 1 subject's walking data that looked much more like biking data rather than walking. It is safer to use literature based features, particularly when on a limited dataset. Other researchers use different datasets so it is very unlikely to be overfit to your dataset.

With the previous exercise, we’ve started to think about the data and how we might build features to separate the signals into activity classes. And while that’s a great exercise, it’s important to recognize that we’ve also just overfit ourselves to the dataset. When we did the data exploration we looked at the entire dataset and now our brains might pick up on patterns that are only specific for that dataset. This almost happened to me, in fact. One of my observations was that the x and z channel overlap for walking and not for biking. However, this isn’t true for S6. There could easily have been a dataset where S6 did not participate, and I would have fully believed that x and z acc channels overlap when people are walking. If I had put this feature in my model, it would have not generalized to S6 and the performance of my model would have significantly degraded.

For small datasets like this, looking at the literature of existing features is a great way to avoid overfitting because the researchers who have come up with these features have looked at different datasets from ours. We are going to use features described in:

* Mehrang S., Pietilä J., Korhonen I. An Activity Recognition Framework Deploying the Random Forest Classifier and A Single Optical Heart Rate Monitoring and Triaxial Accelerometer Wrist-Band. Sensors. 2018;18:613. doi: 10.3390/s18020613.
  [Link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5856093/)
* Liu S, Gao RX, Freedson PS. Computational methods for estimating energy expenditure in human physical activities. Med Sci Sports Exerc. 2012;44:2138–2146. doi: 10.1249/MSS.0b013e31825e825a.
  [Link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3475744/)

We describe some of these features in an IPython notebook and leave some of the implementation to you in the following exercise!

# Summary {#summary}

We’ve just done our first modeling task with wearable data. It took a while to get here. We had to learn what an accelerometer was and how it collected information about movement, and we had to learn about signal processing to build the features that we used in our model. We were able to build a pretty successful three-class classifier using a random forest and by doing proper hyperparameter optimization. But this problem is about as easy as it gets.

If you’re looking for a challenge, go to the original dataset and see that it actually has four classes -- running, walking, high-intensity biking, and low-intensity biking. You could try building a classifier that can distinguish between high-intensity and low-intensity biking. You’ll also find a PPG signal in the dataset. That might help you discriminate between these two classes. You might also find that it’s impossible. Maybe there’s not enough information in these sensors to solve this problem, or maybe the dataset wasn’t collected as rigorously as we need. Ultimately, low-resistance and high-resistance is subjective, and the dataset description even notes:

> ...each participant was free to set the pace of the treadmill and pedal rate on the bike so they were comfortable and also to change these settings or stop the exercise at any time.

More often than not, this is the reality of real-world data science problems.

### Outline {#outline}

Understanding Your Data

* Wrist PPG Dataset
* Data Exploration and Visualization Understanding The Literature
  * Feature Engineering and Extraction Modeling Performance Evaluation Hyperparameter Optimization

## Further Resources {#further-resources}

### Data Exploration {#data-exploration}

* [Wrist PPG Dataset](https://physionet.org/content/wrist/1.0.0/)
* This is a great blog post by [Casie Kozrykov](https://towardsdatascience.com/@kozyrkov) who taught me statistics at Google! In it, she describes the dangers of overfitting your brain when you explore your data. [Your dataset is a giant inkblot test](https://towardsdatascience.com/your-dataset-is-a-giant-inkblot-test-b9bf4c53eec5).
* Check out[this StackOverflow discussion](https://stats.stackexchange.com/questions/352688/is-exploratory-data-analysis-important-when-doing-purely-predictive-modeling)on the value of data exploration. From one of the responses:

  > Two weeks spent training a neuralnet can save you 2 hours looking at the input data

* And finally, a[blog post](https://machinelearningmastery.com/understand-problem-get-better-results-using-exploratory-data-analysis/)from a machine learning practitioner on the data exploration.

### Feature Creation {#feature-creation}

This blog post,[Machine Learning with Signal Processing Techniques](http://ataspinar.com/2018/04/04/machine-learning-with-signal-processing-techniques/), goes through a very similar process as this lesson. It starts by explaining some signal processing techniques \(like we did earlier in the course\). The author uses those techniques to build features in much the same way we just did. And then, he uses those features to build an activity classification model, just as we are about to!

The algorithm we built was inspired by these two papers.

[Mehrang S., Pietilä J., Korhonen I. An Activity Recognition Framework Deploying the Random Forest Classifier and A Single Optical Heart Rate Monitoring and Triaxial Accelerometer Wrist-Band. Sensors. 2018;18:613. doi: 10.3390/s18020613.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5856093/)

[Liu S, Gao RX, Freedson PS. Computational methods for estimating energy expenditure in human physical activities. Med Sci Sports Exerc. 2012;44:2138–2146. doi: 10.1249/MSS.0b013e31825e825a.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3475744/)

### Model Building {#model-building}

Random forests are boosted decision tree models. You need to understand a decision tree before learning what a random forest model is. Start with the[`sklearn`tutorial](https://scikit-learn.org/stable/modules/tree.html)on decision trees. Then check out these videos on youtube for a visual explanation:

[Decision Trees Part 1](https://www.youtube.com/watch?v=7VeUPuFGJHk)[Decision Trees Part 2](https://www.youtube.com/watch?v=wpNl-JwwplA)[Random Forest Part 1](https://www.youtube.com/watch?v=J4Wdy0Wc_xQ)[Random Forest Part 2](https://www.youtube.com/watch?v=nyxTdL_4Q-Q)

See this[list of classification accuracy metrics](https://scikit-learn.org/stable/modules/model_evaluation.html#classification-metrics)that can be computed in`sklearn`.

Follow[this series of blog posts](https://towardsdatascience.com/multi-class-metrics-made-simple-part-i-precision-and-recall-9250280bddc2)for an understanding of how these accuracy metrics work on multiclass problems like ours.

### Hyperparameter Optimization {#hyperparameter-optimization}

Nested cross-validation can be a tricky concept to wrap your head around. Here are three different explanations from three different authors. Maybe one of the following resources will explain it in a way that clicks for you:

* [Nested CV - Weina Jin](https://weina.me/nested-cross-validation/)
* [Nested CV - Elder Research](https://www.elderresearch.com/blog/nested-cross-validation)
* [Nested CV - Stack Exchange: Cross Validated](https://stats.stackexchange.com/questions/65128/nested-cross-validation-for-model-selection)

Our code implementing nested CV was pretty verbose so that you could see all the steps. As with almost everything in ML,`sklearn`can do it for us as well and you can learn more about Nested CV in`sklearn`through the[documentation](https://scikit-learn.org/stable/auto_examples/model_selection/plot_nested_cross_validation_iris.html).

Is overfitting our hyperparameters really a problem in practice?[Yes \(or so says this 2010 paper\)](http://www.jmlr.org/papers/volume11/cawley10a/cawley10a.pdf)

An explanation on the difference between hyperparameters and regular parameters with this[article](https://machinelearningmastery.com/difference-between-a-parameter-and-a-hyperparameter/)from Machine Learning Mastery.

If you want to learn more about Regularization through this[article](https://towardsdatascience.com/regularization-an-important-concept-in-machine-learning-5891628907ea)from Towards Data Science.

## Glossary {#glossary}

* **Hyperparameter**: A parameter of the model that dictates how the model learns. This is not trained during the training process of the model itself.
* **Regularization**: Regularization is a technique to reduce overfitting of a model by discouraging complexity in the model.
* **Nested cross-validation**: A technique to determine model performance when hyperparameters are also optimized.
* **Cross-validation**: A technique for estimating model performance where multiple models are trained and tested each on a separate partition of the entire dataset.
* **Classification accuracy**: The percent of correct classifications made by a model.

# Summary {#summary}

Sampling a sensor hundreds of times per second means that raw sensor data has huge dimensionality. There are 7680 points of accelerometer data at 256 Hz over 10 seconds. Trying to model data points of this size will not be successful. We need to do dimensionality reduction.

By selecting features from the literature, we can be confident that we are not using features that overfit to our dataset and that the activity classifier we build will generalize to other studies and devices.

Check out the notebooks for this lesson and`activity_classifier_utils.py`to see how we implement the features and compute them for our dataset.

# Further Resources {#further-resources}

This blog post goes through a very similar process as this lesson. It starts by explaining some signal processing techniques \(like we did earlier in the course\). The author uses those techniques to build features in much the same way we just did. And then, he uses those features to build an activity classification model, just as we are about to![Machine Learning with Signal Processing Techniques](http://ataspinar.com/2018/04/04/machine-learning-with-signal-processing-techniques/)

## Literature {#literature}

The algorithm we built was inspired by these two papers.

* Mehrang S., Pietilä J., Korhonen I. An Activity Recognition Framework Deploying the Random Forest Classifier and A Single Optical Heart Rate Monitoring and Triaxial Accelerometer Wrist-Band. Sensors. 2018;18:613. doi: 10.3390/s18020613.
  [Link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5856093/)
* Liu S, Gao RX, Freedson PS. Computational methods for estimating energy expenditure in human physical activities. Med Sci Sports Exerc. 2012;44:2138–2146. doi: 10.1249/MSS.0b013e31825e825a.
  [Link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3475744/)

# Activity Classification {#activity-classification}

Now that we've explored the data, examined the literature, chosen our features, and pre-processed all the data. Now it's time to finally build the classifier!

First off, we will do feature extraction to train on 10 second long non-overlapping windows. And we used sklearn to build a random forest classifier to classify our data. Then we defined the hyperparameters with 100 trees where each tree has a maximum depth of 4.

Now we are ready to build and train the model!

But as we just trained on the whole dataset we can't easily evaluate it. But one way to evaluate the performance of a multi-class classifier is to look at a confusion matrix. The confusion matrix shows how many data points were misclassified and what they were misclassified as.

# Summary {#summary}

We've explored the data, examined the literature, chosen our features, and pre-processed all the data. Now it's time to finally build the classifier!

In this lesson, we finally train our features to build a random forest model. We talk about model performance and use**cross-validation**to estimate our accuracy. We end up with a model with an overall**classification accuracy**of 73%, which is the percent of correct classifications made by the model. But don’t fret, we’ll do better in the next video!

### Quiz {#quiz}

[![](https://video.udacity-data.com/topher/2020/March/5e7a3d11_nd320-c4-l3-fruit-classifierpng/nd320-c4-l3-fruit-classifierpng.png "Fruit Classifier")Fruit Classifier](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/f2d5d3bd-ad72-415e-85e6-208fe1237dfe/modules/b337aa97-ba0a-4a57-8ee6-e15ae15fc987/lessons/889d707f-7014-4216-ac94-ab3c285ad0e0/concepts/1279d063-a181-43fb-a842-0d00a7e33bca#)

# Further Resources {#further-resources}

Random forests are boosted decision tree models. You need to understand a decision tree before learning what a random forest model is. Start with the[`sklearn`tutorial](https://scikit-learn.org/stable/modules/tree.html)on decision trees. Then check out these videos on youtube for a visual explanation:

* [Decision Trees Part 1](https://www.youtube.com/watch?v=7VeUPuFGJHk)
* [Decision Trees Part 2](https://www.youtube.com/watch?v=wpNl-JwwplA)
* [Random Forest Part 1](https://www.youtube.com/watch?v=J4Wdy0Wc_xQ)
* [Random Forest Part 2](https://www.youtube.com/watch?v=nyxTdL_4Q-Q)

See this[list of classification accuracy metrics](https://scikit-learn.org/stable/modules/model_evaluation.html#classification-metrics)that can be computed in`sklearn`.

Follow[this series of blog posts](https://towardsdatascience.com/multi-class-metrics-made-simple-part-i-precision-and-recall-9250280bddc2)for an understanding of how these accuracy metrics work on multiclass problems like ours.

## Glossary {#glossary}

* **Cross-validation**: A technique for estimating model performance where multiple models are trained and tested each on a separate partition of the entire dataset.
* **Classification accuracy**: The percent of correct classifications made by a model.

# Hyperparameter Tuning and Regularization {#hyperparameter-tuning-and-regularization}

We ended the last concept with a classification accuracy of 77%. However, there are a few more ways we can turn to improve the performance.

We at first used our best guesses but now we can explore the space and see if we can improve the performance. We found that reducing the maximum tree depth to 2, we have significantly increased our classification accuracy, from 77% to 89%. By reducing the depth to 2, we are**regularizing**our model. Regularization is an important topic in ML and is our best way to avoid overfitting. This is why we see an increase in the cross-validated performance.

But, we used the entire dataset many times to figure out the optimal hyperparameters. In some sense, this is also overfitting. Our 90% classification accuracy is likely too high, and not the generalized performance. In the next concept, we can see what our actual generalized performance might be if we use our dataset to optimize hyperparameters.

# Cross-Validation and Feature Importance {#cross-validation-and-feature-importance}

Using the Nested Cross Validation technique, we'd ideally pick the best hyperparameters on a subset of the data, and then evaluate it on a hold-out set which is similar to train-validation-test set split but we don't have enough data to do so. When you don't have enough data to separate your dataset into 3 parts, we can nest the hyperparameter selection in another layer of cross-validation.

We then walked through how to actually apply this technique on our dataset. Our performance dropped because we are now not overfitting our hyperparameters when we evaluate model performance.

We have just learned that another way to regularize our model and increase performance \(besides reducing the tree depth\) is to reduce the number of features we use. The`RandomForestClassifier `can tell us how important the features are in classifying the data. We found the 10 most important features determined by the`RandomForestClassifier`and trained the model on just those 10 features. The trained model no longer misclassified `bike`as`walk`and this improved our classifier performance by 15%, just by picking the most important features!

# Hyperparameter Tuning in Review {#hyperparameter-tuning-in-review}

Machine learning models use data to fit their internal parameters. However, all models also have parameters that configure how they work and aren’t modified during training, these parameters are called**hyperparameters**. We can train these hyperparameters by creating many different models, each with different hyperparameters and evaluating each model’s performance. Just like we can overfit model parameters, we can also overfit its hyperparameters. To avoid this, we can estimate performance using**nested cross-validation**.

Some hyperparameters affect how easily the model will overfit the data, sometimes at the expense of complexity. In our case, this hyperparameter was the depth of the trees in the forest. When we limited the tree depth to just 2, we saw the cross-validation error decrease substantially.

Another hyperparameter that we can modify is the number of features that we choose to model. Random forest models use some features more than others to classify the data. In`sklearn`we can ask the`RandomForestClassifier`which features were more important than others. By building a new model that only uses the 10 best features, we were able to improve our performance to 93%.

