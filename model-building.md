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

  


