## Data Splitting Key Points {#data-splitting-key-points}

This may seem like a trivial topic, however, it is actually something that is often done incorrectly and can lead to significant downstream issues later.

Imagine that you spent significant time working on a model only to find out that your results are invalid because the data was not prepared correctly. This sometimes happens due to a rush to process data without enough testing and validation of the data and steps before modeling.

### Challenges: {#challenges-}

**Data Leakage**: Inadvertently sharing data between test and training datasets.

Data leakage is a massive problem as your model will perform fantastically during training and fail miserably in production.

An example of where this can occur is when you have a longitudinal dataset and you use different patient encounters across different splits. You may inadvertently leak in information about the patient into your training data that you will be testing on. Essentially giving your model some of the answers. So preventing data leakage is very important to ensure your mode can generalize in production.

#### Additional Resources {#additional-resources}

* [Kaggle Preventing Data Leakage](https://www.kaggle.com/alexisbcook/data-leakage)

[![](https://video.udacity-data.com/topher/2020/April/5e90acc5_l3-ehr-data-transformations-and-tensorflow-feature-engineering-10/l3-ehr-data-transformations-and-tensorflow-feature-engineering-10.jpg "Representative Splitting")Representative Splitting](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/0054fbd5-5779-4a32-b10d-5fb066203aee#)

### Challenges continued {#challenges-continued}

**Representative Splitting**: Having accurate labels and demographics in your data splits that reflect the real world. A major challenge for most machine learning problems is a generalization and building a dataset that is representative. Common errors that can occur include:

* Non-representative distribution of your label across the splits
* Non-representative demographics

**Example**: Only female patients in training and male in testing

This would introduce some unintended biases and issues in your model for procedures that are gender-specific.

### Testing and Validating Dataset Splitting {#testing-and-validating-dataset-splitting}

It is important to have some ways to assess whether you have split your data right.

Here are a few ways to do this.

1. Assess to make sure that a single patient's data is not in more than one partition to avoid possible data leakage.
2. Check that the total number of unique patients
   **across the splits**
   is equal to the total number of unique patients in the
   **original dataset**
   . This ensures no patient information lost in the splitting and that the counts are correct.
3. Check that the total number of rows in
   **original dataset**
   should be equal to the sum of rows across
   **all three dataset partitions**
   .

`len(original_df) == len(train_df) + len(val_df) + len(test_df)`should evaluate to`True`.

