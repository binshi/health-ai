## Transformations and Feature Engineering Overview {#transformations-and-feature-engineering-overview}

This lesson is divided into 3 parts:

1. EHR Dataset Levels

In this part, there are three levels - line, encounter, and longitudinal. By the end of this section, you will be able to identify the level of your dataset as well as conduct tests and transform your data.

1. Dataset Splitting Without Data Leakage

In this part, you will learn about dataset splitting without Data leakage, which can be a major issue in EHR datasets. By the end of part two, you will be able to implement some basic tests to help prevent issues when splitting data.

1. Feature Engineering with Tensorflow

Finally, we will cover Feature Engineering with Tensorflow. In this part, we will cover ETL \(Extract, Transform, Load\) using TensorFlow. This will allow you to scalably process and transform your data for modeling. You will also be able to transform datasets using the TF Feature Column API for both numerical and categorical features. The Feature Column API can be extremely useful for transforming datasets at scale and building some unique feature types.

Let's get started!

[![](https://video.udacity-data.com/topher/2020/April/5e9089dd_l3-ehr-data-transformations-and-tensorflow-feature-engineering/l3-ehr-data-transformations-and-tensorflow-feature-engineering.jpg "Transformations and Feature Engineering Overview")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/3306dae9-fbca-4e06-a00c-49bcdf9e6099#)EHR Dataset Levels Key Points

With EHR datasets, there are three levels.

* Line
* Encounter
* Longitudinal.

These levels are extremely important in healthcare data, and being able to identify and work with data at the correct level will ensure that you start with the correct data type and dataset to feed to your models.

[![](https://video.udacity-data.com/topher/2020/April/5e908a09_l3-ehr-data-transformations-and-tensorflow-feature-engineering-1/l3-ehr-data-transformations-and-tensorflow-feature-engineering-1.jpg "Overview of the Three Levels")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

### Line Level {#line-level}

**Line Level**: A denormalized or disaggregated representation of all the things that might happen in a medical visit or encounter.

Think of a visit to the doctor for bronchitis.

Your line-level data entries could be:

* A diagnosis code of bronchitis
* A medication code for a cough suppressant
* A procedure code for a test for bronchitis and a line could be a diagnosis or medication that was prescribed. Another line could include information on a lab test that the doctor ordered for informing the diagnosis.

### Encounter Level {#encounter-level}

**Encounter Level**: Also known as the visit level, which is the aggregated information from the previously mentioned line level for one encounter. This information can be collapsed into a single row or arrays.

Using the example above, the encounter level for that visit would include the diagnosis code of bronchitis, medication code for a cough suppressant, and the procedure code for a test for bronchitis in one array or list.

### Longitudinal Level {#longitudinal-level}

**Longitudinal Level**: Also known as the patient level view. This level aggregates the patient history and can show how the culmination of visits/encounter lead to some clinical impact.

Continuing with our example above if the patient contracts bronchitis often, over a series of years, we might gain some insights into a possible autoimmune disease or know exactly what to prescribe the patient when they start seeing symptoms.

Now that you have a basic understanding of the different levels we'll explore them a bit more with examples.

[![](https://video.udacity-data.com/topher/2020/April/5e908a2f_l3-ehr-data-transformations-and-tensorflow-feature-engineering-2/l3-ehr-data-transformations-and-tensorflow-feature-engineering-2.jpg "Example Overview of Dataset Levels")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

## EHR Dataset Levels Continued {#ehr-dataset-levels-continued}

As stated above, EHR records are commonly represented at one of the following three levels: line, encounter, and longitudinal levels. Let's review this one more time with the visual above.

Patient A had an Encounter on January 20th of 2019, where they had 3 different codes produced. Patient A also had another encounter on March 20th of 2019 with its own set of codes. All together these encounters and line-level codes add up to the Longitudinal Level of knowledge we have on that particular patient.

The Longitudinal view is an important level for aggregating the patient history and is where you connect information across visits/encounters and rolls up information to determine trends across time.

### Why are EHR levels important? {#why-are-ehr-levels-important-}

Using the wrong EHR dataset level can lead to major errors with building models because data preparation is done with faulty assumptions and lead to serious error. For example, a common cause is the duplication of encounter information when you take a line-level dataset and treat it as an encounter level dataset.

#### Example: {#example-}

A particular encounter might have 50 lines and that might be treated inadvertently as 50 distinct encounters when it is actually one encounter. This has the effect of upsampling certain common values for that encounter in your dataset, but also creates a great deal of noise since those 50 lines might have only slight differences.

Further, selecting the wrong encounter from the patient record can often occur and there might be a case where you only want the earliest or latest visit or state for a patient or time step for your model. This can cause many issues that might not become apparent until the modeling or deployment phases of your project

[![](https://video.udacity-data.com/topher/2020/April/5e908a65_l3-ehr-data-transformations-and-tensorflow-feature-engineering-3/l3-ehr-data-transformations-and-tensorflow-feature-engineering-3.jpg "How do you know which level you are at?")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

## How do you know the dataset level for your data? {#how-do-you-know-the-dataset-level-for-your-data-}

This is actually fairly easy if you collect some key metrics from your dataset and there are different ways to do this but I provided a few simple ways to do below.

1. The total number of rows in the dataset. This is a simple calculation with `len()`
2. The number of unique encounters or visits. You can calculate this by finding the field\(s\) that give the identity of a unique encounter using `nunique()`
   .

### Example {#example}

```
total_rows = len(fake_df)
total_encounters = fake_df['encounter'].nunique()
```

[![](https://video.udacity-data.com/topher/2020/April/5e908a84_l3-ehr-data-transformations-and-tensorflow-feature-engineering-4/l3-ehr-data-transformations-and-tensorflow-feature-engineering-4.jpg "Line Level")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

From here we do some simple calculations to figure out our dataset level.

If the total number of rows is greater than the number of unique encounters, it is at the line level.

Again using our example from above:

```
total_rows = len(fake_df)
total_unique_encounters = len(fake_df['encounter'].nunique())
```

if the output was

* `total_rows = 43464`
* `total_unique_encounters = 3259`

We could find out using

* `print(total_rows > total_unique_encounters) `would evaluate to `True`

Therefore this dataset would be at the line level.

[![](https://video.udacity-data.com/topher/2020/April/5e908aa3_l3-ehr-data-transformations-and-tensorflow-feature-engineering-5/l3-ehr-data-transformations-and-tensorflow-feature-engineering-5.jpg "Encounter Level")Encounter Level](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

If the total number of rows is equal to the number of unique encounters, it is at the encounter level.

Again using our example from above:

```
total_rows = len(fake_df)
total_unique_encounters = len(fake_df['encounter'].nunique())
```

if the output was

* `total_rows = 3464`
* `total_unique_encounters = 3464`

We could find out using

* `print(total_rows == total_unique_encounters) `would evaluate to `True`

Therefore this dataset would be at the encounter level.

[![](https://video.udacity-data.com/topher/2020/April/5e908adf_l3-ehr-data-transformations-and-tensorflow-feature-engineering-6/l3-ehr-data-transformations-and-tensorflow-feature-engineering-6.jpg "Longitudinal Level")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

## Longitudinal Level {#longitudinal-level}

For the longitudinal or patient level, you will see multiple encounters grouped under a patient and you might not even see the encounter id field since this information is collapsed/aggregated under a unique patient id. In this case, the total number of rows should equal the total number of unique patients.

