### De-identifying a Dataset {#de-identifying-a-dataset}

**De-identifying**a dataset refers to the removal of identifying fields like name, address from a dataset. De-Identification is done to reduce privacy risks to individuals and support the secondary use of data for research and such.

This is not something you should be doing on your own. HIPAA has two ways that you can use to de-identify a dataset.

The first method is the**Expert Determination Method**and this is done by a statistician that determines there is a small risk that an individual could be identified.

The second method is called**Safe Harbor**and it refers to the removal of 18 identifiers like name, zip code, etc.

**Limited Latitude**: Very limited scope of work. EHR Data can only be used for the purpose granted.

#### Additional Resources {#additional-resources}

[De-Identification Rationale](https://www.hhs.gov/hipaa/for-professionals/privacy/special-topics/de-identification/index.html#rationale)

## Key Points {#key-points}

[![](https://video.udacity-data.com/topher/2020/April/5e8ce705_l1-ehr-data-security-analysis-5/l1-ehr-data-security-analysis-5.jpg "Where EDA fits into CRISP-DM")Where EDA fits into CRISP-DM](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/eaf98312-bcfb-473d-abf1-0d78164a561f/concepts/83e54a91-ca83-495a-8a8d-364990a1cd9f#)

## Data Schema Analysis {#data-schema-analysis}

**EDA**: Exploratory Data Analysis

EDA is a step in the data science process that is often overlooked for the modeling and evaluation phase that can be easier to quantify and benchmark.

**CRISP-DM**: This stands for “cross-industry standard process for data mining” and is a common framework used for data science projects and includes a series of steps from business understanding to deployment.

### EDA and CRISP-DM {#eda-and-crisp-dm}

As you can see from the image above EDA falls in the Data Understanding phase of CRISP-DM

#### Additional Resources {#additional-resources}

[CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)[What is Exploratory Data Analysis](https://towardsdatascience.com/exploratory-data-analysis-8fc1cb20fd15)[EDA in Python](https://towardsdatascience.com/exploratory-data-analysis-in-python-c9a77dfa39ce)

[![](https://video.udacity-data.com/topher/2020/April/5e8ce72c_l1-ehr-data-security-analysis-4/l1-ehr-data-security-analysis-4.jpg)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/eaf98312-bcfb-473d-abf1-0d78164a561f/concepts/83e54a91-ca83-495a-8a8d-364990a1cd9f#)

### Reasons EDA is important {#reasons-eda-is-important}

* EDA can enable you to discover features or data transformations/aggregations that might have data leakage. This can save a tremendous amount of time and prevent you from building a flawed model.
* EDA can help you better translate and define modeling objectives and corresponding evaluation metrics from a machine learning/data science and business perspective.
* EDA can help inform strategies for handling missing/null/zero valued data. This is a common issue that you will encounter with EHR data that you will have missing values and have to determine imputing strategies accordingly.
* EDA can help to identify subsets of features to utilize for feature engineering and modeling along with appropriate feature transformations based off of type \(e.g. categorical vs numerical features\)

## Key Things to Consider: {#key-things-to-consider-}

* Identify the predictor
* Identify categorical, numerical features
* Work with SMEs and Domain Experts
* Domain knowledge is key to representing data correctly

You can find this dataset in the notebook on this page to inspect the dataset as well as use the link below to get more specific information for the different categories.

[UCI Heart Disease Dataset](https://archive.ics.uci.edu/ml/datasets/Heart+Disease)

[![](https://video.udacity-data.com/topher/2020/April/5e8cea63_l1-ehr-data-security-analysis-6/l1-ehr-data-security-analysis-6.jpg "Key Distributions")Key Distributions](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/eaf98312-bcfb-473d-abf1-0d78164a561f/concepts/4081fe09-1941-4b29-97eb-9e4ac1c8bb61#)

## Value Distributions Review {#value-distributions-review}

* Normal is the well-known bell-curve that most people are familiar with and is also referred to as a Gaussian distribution
* The uniform distribution is where the unique values have almost the same frequency and this is important b/c this might indicate some issue with the data
* Skewed/unbalanced data distributions as the name indicates are where a smaller subset of values or a single value dominates
* Bimodal and Poisson but for the scope of this course we will not cover those

## Missing Values and Outliers {#missing-values-and-outliers}

Missing values are especially common in healthcare where you may have incomplete records or some fields are sparsely populated

### Missing Data Classification {#missing-data-classification}

**MCAR**which stands for Missing Completely at Random. This means that the data is missing due to something unrelated to the data and there is**no systematic reason**for the missing data. In other words, there is an equal probability that data is missing for all cases. This is often due to some instrumentation like a broken instrument or process issue where some of the data is randomly missing.

**MAR**refers to Missing at Random and this is the opposite case where there**is some systematic relationship**between data and the probability of missing data. For example, there might be some missing demographics choices in surveys.

**MNAR**is a Missing Not at Random and this usually means there is a relationship between a value in the dataset and the missing values.

Understanding why data is missing help with choosing the best imputing method to fill or drop the values in your dataset.

#### Additional Resources {#additional-resources}

* [Imputation Methods](https://towardsdatascience.com/6-different-ways-to-compensate-for-missing-values-data-imputation-with-examples-6022d9ca0779)
* [Advanced Imputation Methods](https://www.sciencedirect.com/science/article/pii/S2352914819302783)

## High Cardinality {#high-cardinality}

**Cardinality**: refers to the number of unique values that a feature has and is relevant to EHR datasets because there are code sets such as diagnosis codes in the order of tens of thousands of unique codes. This only applies to**categorical features**and the reason this is a problem is that it can increase dimensionality and makes training models much more difficult and time-consuming.

**How do we define a field with high cardinality?**

* Determine if it is a categorical feature.
* Determine if it has a high number of unique values. This can be a bit subjective but we can probably agree that for a field with 2 unique values would not have high cardinality whereas a field like diagnosis codes might have tens of thousands of unique values would have high cardinality.
* Use the
  `nunique()`
  method to return the number of unique values for the categorical categories above.

#### Additional Resources {#additional-resources}

[Reducing Dimensionality](https://en.wikipedia.org/wiki/Dimensionality_reduction)

