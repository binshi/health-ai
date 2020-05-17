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

* `print(total_rows > total_unique_encounters)`would evaluate to `True`

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

* `print(total_rows == total_unique_encounters)`would evaluate to `True`

Therefore this dataset would be at the encounter level.

[![](https://video.udacity-data.com/topher/2020/April/5e908adf_l3-ehr-data-transformations-and-tensorflow-feature-engineering-6/l3-ehr-data-transformations-and-tensorflow-feature-engineering-6.jpg "Longitudinal Level")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/cfa9ae5d-50fc-45e0-9145-b158a80e6717#)

## Longitudinal Level {#longitudinal-level}

For the longitudinal or patient level, you will see multiple encounters grouped under a patient and you might not even see the encounter id field since this information is collapsed/aggregated under a unique patient id. In this case, the total number of rows should equal the total number of unique patients.

## Encounter Representation Key Points {#encounter-representation-key-points}

[![](https://video.udacity-data.com/topher/2020/April/5e90ab5b_l3-ehr-data-transformations-and-tensorflow-feature-engineering-9/l3-ehr-data-transformations-and-tensorflow-feature-engineering-9.jpg "What is an Encounter?")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/906a7254-1ef6-47c9-8442-a9982addf173#)

[**Encounter**](https://www.hl7.org/fhir/encounter-definitions.html): “An interaction between a patient and healthcare provider\(s\) for the purpose of providing healthcare service\(s\) or assessing the health status of a patient.”

### What is an encounter? {#what-is-an-encounter-}

The definition of an encounter commonly used for EHR records comes from the Health Level Seven International \(HL7\), the organization that sets the international standards for healthcare data. As the definition states, it is essentially an interaction between a patient and a healthcare professional\(s\). It usually refers to doctors visits and hospital stays.

### How do we aggregate line level at encounter level? {#how-do-we-aggregate-line-level-at-encounter-level-}

1. Create a column list for the columns you would use to group. Likely these would be:

   * "encounter\_id"
   * "patient\_id"
   * "principal\_diagnosis\_code"

2. Create column list for the other columns not in the grouping

3. Transform your data into a new dataframe. You can use
   `groupby()`
   and
   `agg()`
   functions for this
4. Then you can do a quick inspection of the result by grabbing one of the patient records and to compare the output of the original dataframe and the newly transformed encounter dataframe.

**Note**: The data used in the walkthrough was created just to show you how this would work. You'll practice this more in later exercises as well.

#### Additional Resources {#additional-resources}

* [Encounter Definitions](https://www.hl7.org/fhir/encounter-definitions.html)

# Longitudinal Representation Key Points {#longitudinal-representation-key-points}

**Longitudinal Representation**: Patient History

### What is a longitudinal data representation? {#what-is-a-longitudinal-data-representation-}

Another way to view it is a patient history representation. It is basically a way to aggregate all of the visits/encounters that a patient may have in the healthcare system. Having all of this information is ideal to best analyze and diagnose correctly, but the reality is that patient records do not always come cleanly organized and aggregated correctly, in real-world datasets. Luckily, we can use the line and encounter levels to help with that!

[![](https://video.udacity-data.com/topher/2020/April/5e90abf8_l3-ehr-data-transformations-and-tensorflow-feature-engineering-7/l3-ehr-data-transformations-and-tensorflow-feature-engineering-7.jpg "Longitudinal Representation Example")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/c33cf4f5-e2f0-44aa-8c12-db17b80cf289#)

Combining patients together into a dataset and finding a way to aggregate and represent the visits across a patient is the key to being able to unlock powerful insights and analysis.

### Challenges and Benefits of Using the Longitudinal Level {#challenges-and-benefits-of-using-the-longitudinal-level}

#### Challenges: {#challenges-}

* Scaling with this type of data requires vast resources
* Data representation and preparation requires more preparation and validation
* Training vs production/deployment data differences can cause major issues with model predictions

**Example**: You train this great Model, and it has fantastic results!

However, the data you used for this model had an implicit assumption that you were unaware of :

_Only the label for the last encounter for a patient history would be included._

You think that you can always transform the data in production. Right?

**Reality**: You have put your model into production and things start going haywire.

You are not getting the same results that you anticipated from your trained model. Even with some cross-validation and other augmentation methods, you still see significant model drift. After some analysis of the production data pipeline, you find out that the production use case has an**Encounter selected at random**from the patient history and information has been duplicated by accident because the information was not leveled correctly.

From this example, you can see how important it is to make sure you, or your data team, have properly aggregated the data.

#### Benefits: {#benefits-}

Even though data preparation and validation are more difficult, there are significant benefits to using a longitudinal representation as it allows you to use a full patient history. This helps to better model changes in state over time. When you can also aggregate other patients longitudinal data together, you can achieve some unbelievable results.

[![](https://video.udacity-data.com/topher/2020/April/5e90ac1f_l3-ehr-data-transformations-and-tensorflow-feature-engineering-8/l3-ehr-data-transformations-and-tensorflow-feature-engineering-8.jpg "Deep Learning Potential Importance")](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/e8ba701a-3efd-4d33-8e73-cbb55ab9a311/concepts/c33cf4f5-e2f0-44aa-8c12-db17b80cf289#)

To further illustrate the importance of having a strong EHR longitudinal data representation, you can look through the links in this section about Google creating a data representation to feed into deep learning models from the Fast Healthcare Interoperability Resources or**FHIR**standards.

In Google's[_Nature_](https://www.nature.com/articles/s41746-018-0029-1)article, they highlighted the aforementioned challenges with building a longitudinal representation. The 216K+ patients in the dataset were extrapolated to 46B data points! Now imagine attempting to scale this even further with more patients or other types of data like genomic data, clinical trial data, etc. This is where resources like cloud computing have become critical, but even these can be strained with this much data.

As most practitioners know, the data preparation stage is one of the most time consuming, but impactful parts of a data science project. The representation and deep learning models outperformed clinically used traditional predictive models in**ALL**cases. It is still early days for the healthcare industry as they have just started to consider using deep learning and still barriers exist in using them due to concerns about interpretability. Hopefully, with more studies like this, critical attention to detail and eye on data security and privacy we can bring this practice to the forefront in healthcare.

#### Additional Links {#additional-links}

* [Google's Nature Paper](https://www.nature.com/articles/s41746-018-0029-1)
* [Google's Patent](https://patents.google.com/patent/US20160042124)
* [FHIR](https://www.hl7.org/fhir/overview.html)
* [FHIR Overview](https://datica.com/blog/what-is-fhir/)



