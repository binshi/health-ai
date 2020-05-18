## Building, Evaluating and Interpreting Models for Bias and Uncertainty Overview {#building-evaluating-and-interpreting-models-for-bias-and-uncertainty-overview}

This lesson contains some really fantastic tools for working with EHR data in AI.

**Note**: At the time this material was created some of the TensorFlow features are in beta form and may change as they launch. That being said these are some bleeding-edge resources to help with many AI projects.

* In the first part, you will get hands-on with using Tensorflow
  `DenseFeatures`
  for building a simple regression model.
* Next, you will first review some common evaluation metrics for EHR models and then learn to implement brier scores for model evaluation
* Then, you'll conduct a demographic bias analysis and become familiar with a framework out of the University of Chicago called Aequitas. You will use this Aequitas for group bias and fairness disparity analysis.
* Next, you will implement uncertainty estimation using the Tensorflow Probability API. You will also review some of the underlying concepts for Bayesian probability and types of uncertainty that very important in evaluating model performance.
  * **Note**: TensorFlow Probability is still in beta at the time this course was published. Documentation is still being built out and some of the code may change.
* Finally, you will interpret models with Shapley values. You will first review model interpretability and some model agnostic methods. Then you will use one of those methods, Shapley values.
  * **Note**: This section is included as a bonus and is not included in the project itself, but is highly useful.

There is so much to cover in this section. Head to the next section to get started.

[![](https://video.udacity-data.com/topher/2020/April/5e90dae4_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-1/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-1.jpg "Lesson Overview")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/097b1cd3-89ae-49f1-b1f0-723d438b9cbf#)

### Tensorflow Regression Model with DenseFeatures

[![](https://video.udacity-data.com/topher/2020/April/5e90db09_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-2/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-2.jpg "Build Model with TF \`DenseFeatures\`")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/c3d65858-313d-45b0-9ba1-97b55b19098c#)

## TF DenseFeatures Key Points {#tf-densefeatures-key-points}

In this section, we will focus on just building a simple Tensorflow regression model with TF`DenseFeatures`, but we realize that there are more advanced deep learning architectures. Please feel free to try different approaches to augment your features and architecture with the walkthroughs and exercises in this lesson. There are some additional links below to explore around with some AutoML offerings. These are not the focus of this course but are included in case this is interesting to you.

### Build Model with TF DenseFeatures {#build-model-with-tf-densefeatures}

Tensorflow DenseFeatures, combining features, like those from the TensorFlow Feature Columns API, into a dense representation for the model.

You can only use certain TF Feature Columns with`DenseFeatures`:

* `numeric_column`
* `embedding_column`
* `bucketized_column`
* `indicator_column`

**Note**:

* For the sake of simplicity, we will use the Sequential API for this course, but if you want to customize further, feel free to try to the Functional API. However, you might encounter some issues later with configuring some other parts with the TF Probability outputs.
* As of writing, Tensorflow is experimenting with Sequence Features columns as well that can be combined with the
  `SequenceFeatures`
  function. This will not be covered in the course but wanted to share this for your info since it can be very useful.

#### Additional Resources {#additional-resources}

* [TF DenseFeatures](https://www.tensorflow.org/api_docs/python/tf/keras/layers/DenseFeatures)
* [Functional API](https://www.tensorflow.org/guide/keras/functional)
* [SequenceFeatures](https://www.tensorflow.org/api_docs/python/tf/keras/experimental/SequenceFeatures)
* [GCP AutoML](https://cloud.google.com/automl/)
* [AWS Autopilot](https://aws.amazon.com/sagemaker/autopilot/)
* [AdaNet](https://ai.googleblog.com/2018/10/introducing-adanet-fast-and-flexible.html)
* [AutoKeras](https://autokeras.com/)
* [H20 Driverless AI](https://www.h2o.ai/products/h2o-driverless-ai/)
* [AdaNet Additional](https://arxiv.org/abs/1607.01097)
* [AutoKeras Titanic](https://autokeras.com/examples/titanic/)
* [Evolved Transformer](https://arxiv.org/abs/1901.11117)

### Tensorflow DenseFeatures Layer {#tensorflow-densefeatures-layer}

Tensorflow provides the[DenseFeatures layer](https://www.tensorflow.org/api_docs/python/tf/keras/layers/DenseFeatures)as a way to combine some of the Tensorflow feature columns we learned earlier into a dense input tensor to your model. For this example, we use the[Sequential API](https://www.tensorflow.org/api_docs/python/tf/keras/Sequential)but you could use the[Functional API](https://www.tensorflow.org/guide/keras/functional)as well with some adjustments. Hopefully, the Tensorflow Keras modeling functions should be familiar to you and for the walkthrough, we again used the architecture from the[Tensorflow regression tutorial](https://www.tensorflow.org/tutorials/keras/regression).

One key thing to note is that you can only use some Tensorflow Feature Column types with DenseFeatures so it is worth checking. At the time of writing this course, here are the features that you can use.

* [Numeric Feature](https://www.tensorflow.org/api_docs/python/tf/feature_column/numeric_column)
* [Embedding Feature](https://www.tensorflow.org/api_docs/python/tf/feature_column/embedding_column)
* [Bucketized Feature](https://www.tensorflow.org/api_docs/python/tf/feature_column/bucketized_column)
* [One hot encoded Feature](https://www.tensorflow.org/api_docs/python/tf/feature_column/indicator_column)

Below I will walk through the simple steps to use the DenseFeatures layer.

* First, combine all of your Tensorflow feature columns into a list that can be passed to the DenseFeatures layer. Please note that this is not the column name but the Tensorflow feature column metadata that you will get when you create a Tensorflow feature.
  ```
  feature_columns = tf_categorical_feature_list + tf_numerical_feature_list
  dense_feature_layer = tf.keras.layers.DenseFeatures(feature_columns)
  ```
* Next, we simply add this 'dense\_feature\_layer' as the first layer to the model and this will handle the combining of feature inputs to the model.

  ```
    model = tf.keras.Sequential(**dense_feature_layer**,tf.keras.layers.Dense(
  64, activation='relu')
   ... additional layers
  ```

## Common EHR Metrics Key Points {#common-ehr-metrics-key-points}

For this course, we will assume some exposure to common evaluation metrics used for classification and regression. These terms should hopefully be familiar and we will not cover them fully, but here is a quick review.

### Common Classification Metrics {#common-classification-metrics}

* [**ROC**](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc): receiver operating characteristic curve or ROC curve that shows a graph of the performance of a classification model. It is the True Positive Rate Vs. False Positive Rate across different thresholds.
* [**AUC**](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc): Area under the ROC Curve measures the entire two-dimensional area underneath the entire ROC curve.
* [**F1**](https://en.wikipedia.org/wiki/F1_score): Harmonic mean between precision and recall
* [**Precision**](https://en.wikipedia.org/wiki/Precision_and_recall): the fraction of relevant instances among the retrieved instances
* [**Recall**](https://en.wikipedia.org/wiki/Precision_and_recall): the fraction of the total amount of relevant instances that were actually retrieved.

### Common Regression Metrics {#common-regression-metrics}

* [**RMSE**](https://en.wikipedia.org/wiki/Root-mean-square_deviation): Root Mean Square Error- a measure of the differences between values predicted by a model.
* [**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error): Mean Absolute Percentage Error is a measure of quality for regression models loss.
* [**MAE**](https://en.wikipedia.org/wiki/Mean_absolute_error): Mean Absolute Error is a measure of errors between paired observations.

[![](https://video.udacity-data.com/topher/2020/April/5e90db6e_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-3/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-3.jpg "Precision Recall Tradeoff")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/933a24af-3859-4a7f-b79a-1d227798758c#)

## Precision-Recall Tradeoff {#precision-recall-tradeoff}

As a quick reminder, there is often some level of precision that you must align with and some capture rate or recall that you are trying to improve. This balance between the two is a necessary tradeoff and a key component that you must communicate with non-technical stakeholders who might have unrealistic views of the impact or capabilities of AI.

## Brier Scores {#brier-scores}

One metric that you might not be familiar with is a Brier score and it is often used in weather forecasting for estimating the probability certainty of a forecast. It can be a useful metric for comparing the performance of algorithms based on the degree of confidence in a given prediction. This can be helpful because the confidence and measurement of uncertainty can yield vastly different interpretations.

For the actual definition, it is essentially the mean squared error of a given probability forecast and I walk through the formula step by step below. However, please note that you are not expected to know this formula or use it in this course because we are focusing on the predictions for a regression model. It is introduced as a relevant evaluation metric for if we had a binary classification problem with the associated prediction probabilities.

### Brier Score Breakdown Part 1 {#brier-score-breakdown-part-1}

Basically you take the probability forecast on a 0 to 1 scale which is\_f of t\_and subtract that from\_O of t\_which is the actual value which is a binary 0 and 1 value. You then square the difference of this value.

[![](https://video.udacity-data.com/topher/2020/April/5e90db8c_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-4/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-4.jpg "Brier Score Breakdown Part 2")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/933a24af-3859-4a7f-b79a-1d227798758c#)

1. Take the summation of these squared differences from t=1 to N, the total number of predictions.

[![](https://video.udacity-data.com/topher/2020/April/5e90dbe8_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-5/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-5.jpg "Brier Score Breakdown Part 3")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/933a24af-3859-4a7f-b79a-1d227798758c#)2. Then divide by N the total number of predictions

[![](https://video.udacity-data.com/topher/2020/April/5e90dc18_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-6/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-6.jpg "Brier Score Breakdown Part 4")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/933a24af-3859-4a7f-b79a-1d227798758c#)

The result is the Brier Score which ranges from 0 and 1.

* Lower is better
* 0 is the best score
* 1 is the worst score.

#### Additional Resources {#additional-resources}

* [A Note on the Evaluation of Novel Biomarkers: Do Not Rely on Integrated Discrimination Improvement and Net Reclassification Index](https://pubmed.ncbi.nlm.nih.gov/23553436/)
* [Brier Scores](https://en.wikipedia.org/wiki/Brier_score)
* [Brier Score Limitations](https://diagnprognres.biomedcentral.com/articles/10.1186/s41512-017-0020-3)

# Demographic Bias Analysis Points {#demographic-bias-analysis-key-points}

## Why is bias important to consider in your models? {#why-is-bias-important-to-consider-in-your-models-}

It's important to note that bias within models can restrict or limit patient access to key medical benefits from government aid programs.

Programs using AI algorithms to help automate approvals of key government benefits are becoming more commonplace. However, it is just as important to consider how bias can unintentionally occur.

[![](https://video.udacity-data.com/topher/2020/April/5e90dc4b_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-8/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-8.jpg "Why is bias in models important to consider?")Why is bias in models important to consider?](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/4c00ff99-adf2-47f7-8d2d-e77b2c5ccfe4#)

Another reason that you want to consider bias in models is that in order to create better treatments for patients we need to find better ways to select and recruit patients that represent the wider population that a drug/treatment would be targeted for.

In many cases, there may be systemic biases for key groups and while this cannot always be prevented, bringing awareness of limitations and biases can give a more accurate picture of a treatment's effectiveness across different demographics.

In the example above, we can see that the purple parallelogram is representative of the population, while the blue circles and triangles are not and both are over-represented in the trial.

### Unintended Bias {#unintended-bias}

**Unintended biases**: a bias that is not intentional and often is not even apparent to the creator of a model

Unintended biases represent the unconscious or unintentional biases that come with the AI models that we are building. Becoming more aware of these biases and how they impact different groups is key.

**Note**: We usually associate bias with a negative connotation, but biases can be a source of valuable prior information. The problem can be when we are not aware of our biases and do not account for those that have a significant impact on the populations these models serve.

## Aequitas {#aequitas}

* Developed by University of Chicago Data Science for Social Good
* Addresses concerns about unintended bias unfairly affecting certain groups
* Definitions and metrics for unintended bias in predictive models



