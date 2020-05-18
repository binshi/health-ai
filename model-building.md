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

[![](https://video.udacity-data.com/topher/2020/April/5e90dc4b_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-8/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-8.jpg "Why is bias in models important to consider?")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/4c00ff99-adf2-47f7-8d2d-e77b2c5ccfe4#)

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

# Uncertainty Estimation Key Points {#uncertainty-estimation-key-points}

### Why use Uncertainty Estimation? {#why-use-uncertainty-estimation-}

A typical classification problem provides predictions with relative weightings across the prediction classes and is different than the confidence in a given prediction. Uncertainty estimation helps us with this.

# Class Probabilities != confidence in a prediction. {#class-probabilities-confidence-in-a-prediction-}

[![](https://video.udacity-data.com/topher/2020/April/5e90dcb7_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-10/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-10.jpg "Uncertainty Weather Example")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/8eb25552-df38-44ee-b290-444c13d9cb7c#)

### Example: {#example-}

Which is better?

1. A weather forecast with a simple Yes or No prediction it will rain with some icons?
2. A forecast that also provides the percent chance of rain or how certain the model is of its prediction?

Most people would prefer the second one and in healthcare this even more important.

[![](https://video.udacity-data.com/topher/2020/April/5e90dcea_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-12/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-12.jpg "Review of Bayesian Probability")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/8eb25552-df38-44ee-b290-444c13d9cb7c#)

## Bayesian Probability Review {#bayesian-probability-review}

### Probabilistic Programming {#probabilistic-programming}

To address uncertainty estimation we can use probabilistic programming, in particular, we can create Bayesian Neural Networks \(BNN\) using TensorFlow Probability. TF probability combines Bayesian probabilistic approaches with deep learning and since it is built on Tensorflow you can train with GPUs too.

Bayesian statistical approaches can be very helpful for situations where you can leverage deep domain knowledge and have small datasets. Healthcare is an industry where you might have small datasets on patient data for a new drug or rare conditions. Also, healthcare is a field where you can leverage deep domain knowledge from medical professionals and medical literature which can help provide positive “bias” in your model with pertinent domain knowledge.

### Review of Bayesian Probability {#review-of-bayesian-probability}

* Prior distribution - P\(A\)
* P\(A \| Evidence\) - Updating the prior with new evidence
* P\(B\) - posterior probability

[![](https://video.udacity-data.com/topher/2020/April/5e90dd19_l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-11/l4-building-evaluating-and-interpreting-models-for-bias-and-uncertainty-11.jpg "Response Differences for Bayesian Probability")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/9f2a59cc-ed42-475d-abe6-fdb731927eff/concepts/8eb25552-df38-44ee-b290-444c13d9cb7c#)

### Example: {#example-}

Let’s say we want to assess whether a drug will pass a phase III clinical trial. The frequentist statistical response would be a yes or no response. However, the bayesian response would give a yes/no and the certainty/confidence in that prediction. This can be helpful when connecting it with metrics like Brier scores that can combine predictions from various models.

If you are only 51% certain that you will pass the clinical trial, how certain would you be?



## Types of Uncertainty {#types-of-uncertainty}

* Aleatoric:
  * Statistical Uncertainty - a natural, random process
  * Known Unknowns

Aleatoric uncertainty is otherwise known as statistical uncertainty and is known unknowns. This type of uncertainty is inherent, and just a part of the stochasticity that naturally exists. An example is rolling a dice, which will have an element of randomness always to it.

* Epistemic:
  * Systematic Uncertainty - lack of measurement, knowledge
  * Unknown Unknowns

Epistemic uncertainty is also known as systemic uncertainty and is unknown unknowns. This type of uncertainty can be improved by adding parameters/features that might measure something in more detail or provide more knowledge.

#### Additional Resources {#additional-resources}

* [Uncertainty](https://en.wikipedia.org/wiki/Uncertainty_quantification)

## Key Points {#key-points}

### Building a Basic Uncertainty Estimation Model with Tensorflow Probability {#building-a-basic-uncertainty-estimation-model-with-tensorflow-probability}

* Using the MPG model from earlier, create uncertainty estimation model with TF Probability.
* In particular, we will focus on building a model that accounts for Aleatoric Uncertainty.

NOTE: Before we go into the walkthrough I want to note that TF Probability is not a v1 yet and documentation and standard patterns are evolving. That being said I wanted to expose you to a tool that might be good to have on your radar and this library can abstract away some of the challenging math behind the scenes.

### Model with Aleatoric Uncertainty {#model-with-aleatoric-uncertainty}

* Known Unknowns
* 2 Main Model Changes

  * Add a second unit to the last dense layer before passing it to Tensorflow Probability layer to model for the predictor y and the
    [heteroscedasticity](https://en.wikipedia.org/wiki/Heteroscedasticity)
    or unequal scattering of data
    ```
    tf.keras.layers.Dense(1 + 1)
    ```

  * DistributionLambda is a special Keras layer that uses a Python lambda to construct a distribution based on the layer inputs and the output of the final layer of the model is passed into the loss function. This model will return a distribution for both mean and standard deviation. The 'loc' argument is the mean and is sampled across the normal distribution as well as the 'scale' argument which is the standard deviation and in this case, is a slightly increasing with a positive slope. Important to note here is that we have prior knowledge that the relationship between the label and data is linear. However, for a more dynamic, flexible approach you can use the[VariationalGaussianProcess Layer](https://www.tensorflow.org/probability/api_docs/python/tfp/distributions/VariationalGaussianProcess). This is beyond the scope of this course but as mentioned can add more flexibility.

    ```
    tfp.layers.DistributionLambda(  
            lambda t:tfp.distributions.Normal(
                loc=t[..., :1],scale=1e-3 + tf.math.softplus(0.1 * t[...,1:])
            )
    ```

* We can use different loss functions such as mean squared error\(MSE\) or negloglik. Note that if we decide to use the standard MSE metric that the scale or standard deviation will be fixed. Below is code from the regression tutorial for using negative log-likelihood loss, which through minimization is a way to maximize the probability of the continuous labels.

  ```
    negloglik = lambda y, rv_y: -rv_y.log_prob(y)
    model.compile(optimizer='adam', loss=negloglik, metrics=[loss_metric])
  ```

* Extracting the mean and standard deviations for each prediction by passing the test dataset to the probability model. Then, we can call mean\(\) or stddev\(\) to extract these tensors.

  ```
  yhat = prob_model(x_tst)
  m = yhat.mean()
  s = yhat.stddev()
  ```

### Model with Epistemic Uncertainty {#model-with-epistemic-uncertainty}

* Unknown Unknowns
* Add [Tensorflow Probability DenseVariational Layer](https://www.tensorflow.org/probability/api_docs/python/tfp/layers/DenseVariational) with prior and posterior functions. Below are examples adapted from the [Tensorflow Probability Regression tutorial notebook](https://github.com/tensorflow/probability/blob/master/tensorflow_probability/examples/jupyter_notebooks/Probabilistic_Layers_Regression.ipynb)
  .
  ```
  def
  posterior_mean_field
  (kernel_size, bias_size=
  0
  , dtype=None)
  :

    n = kernel_size + bias_size
    c = np.log(np.expm1(
  1.
  ))
  
  return
   tf.keras.Sequential([
        tfp.layers.VariableLayer(
  2
  *n, dtype=dtype),
        tfp.layers.DistributionLambda(
  lambda
   t: tfp.distributions.Independent(
            tfp.distributions.Normal(loc=t[..., :n],
                                     scale=
  1e-5
   + tf.nn.softplus(c + t[..., n:])),
            reinterpreted_batch_ndims=
  1
  )),
    ])

  def
  prior_trainable
  (kernel_size, bias_size=
  0
  , dtype=None)
  :

    n = kernel_size + bias_size
  
  return
   tf.keras.Sequential([
        tfp.layers.VariableLayer(n, dtype=dtype),
        tfp.layers.DistributionLambda(
  lambda
   t: tfp.distributions.Independent(
            tfp.distributions.Normal(loc=t, scale=
  1
  ),
            reinterpreted_batch_ndims=
  1
  )),
    ])

  ```
* Here is how the 'posterior\_mean\_field' and 'prior\_trainable' functions are added as arguments to the DenseVariational layer that precedes the DistributionLambda layer we covered earlier.
  ```
  tf.keras.layers.Dense(
  75
  , activation=
  'relu'
  ),
           tfp.layers.DenseVariational(
  1
  +
  1
  , posterior_mean_field, prior_trainable),        
        tfp.layers.DistributionLambda( ....

  ```

### Additional Resources {#additional-resources}

* [Tensorflow Regression with Probabilistic Layers Tutorial](https://blog.tensorflow.org/2019/03/regression-with-probabilistic-layers-in.html)



