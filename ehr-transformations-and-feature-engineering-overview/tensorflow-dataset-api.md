## TensorFlow Feature Column API Key Points {#tensorflow-feature-column-api-key-points}

The TensorFlow Feature Column API helps make data preprocessing easier by abstracting away some of the work for things like normalization in numerical features. If you have done this type of work in Scikit Learn or Pyspark, you might appreciate the work this API does for you when it comes to preparing features for modeling. It also has the ability to add less common features like cross features and shared embeddings.

#### Additional Resources {#additional-resources}

* [TensorFlow Feature Column API](https://www.tensorflow.org/api_docs/python/tf/feature_column)
* [Learn more about Cross Features](https://developers.google.com/machine-learning/crash-course/feature-crosses/video-lecture)
* [TensorFlow Cross Features](https://www.tensorflow.org/api_docs/python/tf/feature_column/crossed_column)
* [TensorFlow Shared Emeddings](https://www.tensorflow.org/api_docs/python/tf/feature_column/shared_embeddings)

[![](https://video.udacity-data.com/topher/2020/April/5e90d94a_l3-ehr-data-transformations-and-tensorflow-feature-engineering-11/l3-ehr-data-transformations-and-tensorflow-feature-engineering-11.jpg "Transform/Preprocess Numerical Features")Transform/Preprocess Numerical Features](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/94e8379b-64a9-4198-9975-90d0fd0d3d75#)

## Numerical Features and TensorFlow Feature Columns API {#numerical-features-and-tensorflow-feature-columns-api}

To use the TensorFlow Feature Columns with numerical features we need to do the following:

1. Identify the fields with numerical features.
2. Use the TensorFlow Dataset API to load the dataset.
3. Create your own custom normalizer function like a z-score

   ```
   def
   z_score_normalizer
   (args)
   :
   return
    z_score_normalization
   ```

4. Use the TensorFlow numeric\_column feature and pass in the z\_score\_normalizer function to the normalizer\_fn argument.
   * `tf.feature_column.numerical_column(column_name, normalizer_fn=z_score_normalizer)`
5. Let the TensorFlow Feature Column API do it's magic!

#### Additional Resources {#additional-resources}

-[Normalize Features in TensorFlow](https://towardsdatascience.com/how-to-normalize-features-in-tensorflow-5b7b0e3a4177)

## TensorFlow Feature Column API for Categorical Features Key Points {#tensorflow-feature-column-api-for-categorical-features-key-points}

For building categorical features with the TensorFlow Features Column API, we follow the following steps:

1. Select the categorical feature columns.
2. Create a vocabulary text file with all of the unique values for a given categorical feature and add a placeholder value for out of vocabulary\(OOV\) values in the first row.
   * **Note**: For columns with a small number of features, you probably don't need to do this step and can instead pass an array/list to the [categorical\_column\_with\_vocabulary\_list function](https://www.tensorflow.org/api_docs/python/tf/feature_column/categorical_column_with_vocabulary_list)
   * The creation of a separate vocabulary file method is particularly great for features with high cardinality. It can also allow you to use other tools like SQL to generate these vocab files with more massive datasets and decouple this process if you are already creating these for data profiling purposes.
3. Create your new feature by passing the vocabulary feature to the final derived feature. This can be an embedding or one-hot encoded feature. In this example, we created a one-hot encoding feature with the [indicator column function](https://www.tensorflow.org/api_docs/python/tf/feature_column/indicator_column)

Example:

```
principal_diagnosis_vocab = tf.feature_column.categorical_column_with_vocabulary_file(
            key="PRINCIPAL_DIAGNOSIS_CODE", vocabulary_file = vocab_files_list[0], num_oov_buckets=1)
```

#### Additional Resources {#additional-resources}

* [Tensorflow Feature Column API Categorical Features](https://www.tensorflow.org/api_docs/python/tf/feature_column/categorical_column_with_vocabulary_file)
* [TensorFlow One-hot Encoding](https://www.tensorflow.org/api_docs/python/tf/one_hot)



