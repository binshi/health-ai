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



