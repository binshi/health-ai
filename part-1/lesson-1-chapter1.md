EHR Code sets allow for the comparison of data across various EHR systems. You first learned about diagnosis codes. In particular, ICD10-CM. You gained knowledge of what this code set is, why it's important, how to read them. You also applied that knowledge by grouping data using EHR codes as well as build an embedding with visualizations. Hopefully, ICD10- CM codes will be easy to work with from now on.

Then you reduced cardinality of a dataset using procedure codes after learning more about ICD10-PCS, CPT and HCPCS. You should be able to break down medication codes too! You should also be able to apply your knowledge to deal with the challenges of working with medication code sets.

Finally, you put all of this code knowledge to work by grouping and categorizing these code sets with CCS. I know that there was a lot of information in this lesson, but now it's time to move on to learning some magic to transform EHR Data! Into what? You'll have to head to the next lesson to find out!

[![](https://video.udacity-data.com/topher/2020/April/5e908920_l2-ehr-code-sets-15/l2-ehr-code-sets-15.jpg "EHR Code Sets Overview")EHR Code Sets Overview](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/36105657-a626-43d7-bed7-2c6fe2fa5f2d#)

## Key Terms {#key-terms}

| Key Terms | Definition |
| :--- | :--- |
| Medical Encounter | An interaction between a patient and healthcare provider\(s\) for the purpose of providing healthcare service\(s\) or assessing the health status of a patient. |
| ICD10: | International Classification of Diseases 10 |
| WHO: | World Health Organization |
| ICD10-CM | International Classification of Diseases 10 - Clinical Modification |
| Primary Diagnosis Code | The code that takes up the most resources to treat. |
| Principal Diagnosis Code | The diagnosis that is found after hospitalization to be the one that is chiefly responsible. |
| Secondary Diagnosis Codes: | The other diagnosis codes listed on an encounter. |
| Medical Procedure | A course of action to achieve an intended result for a patient in healthcare. |
| Procedure Codes | The categorization of these medical codes during an encounter. |
| NDC | National Drug Code |
| Crosswalk | A connection between two different code sets or versions of drugs in the same code set. |
| RXNorm | Groups medications together. |
| CCS | Clinical Classifications Software used to map diagnosis or procedure codes from ICD code sets. |
| MS-DRG | Medicare Severity-Diagnosis Related Group |
| SNOMED CT | Systematized Nomenclature of Medicine—Clinical Terms |

## Working with EHR Code Sets {#working-with-ehr-code-sets}

You are about 20% of the way through and you have gained some very important skills in dealing with EHR data. Now you'll build even more on those skills as we begin to look at Electronic Health Record Code sets. But before we get started, let's review what will be covered in this lesson.

This lesson will delve into Code Sets in EHR and how to work with them, including:

* Diagnosis Codes
  * IDC10-CM
* Procedure Codes
  * ICD10 - PCS
  * CPT
  * HCPCS
* Medication Codes
  * NDC Codes
  * RXNorm
* Using these codes to group and categorize your datasets
  * Using CCS

This is very important because of how many codes there are in the medical field. If you don't understand the codes, you will likely not get all of and/or the right data you need for creating your models. So get ready for a lot of different codes! You'll be a master at figuring them out and using them to group your data by the time you're done.For the purposes of this course, when we refer to a code set we are referencing an EHR data field that is linked to an accepted code standard such as ICD10 or CPT codes, which you will learn more about shortly. Examples of important code sets are diagnosis codes and procedure codes. We will go over these in more detail later.

### Why do we need code sets? {#why-do-we-need-code-sets-}

There are many different providers and EHR systems around the world. There needs to be a standard way to encode common diagnoses, medications, procedures, and lab test results across all these providers and systems. We will focus on some of the most common code sets that allow for some of the most high-value analysis.

[![](https://video.udacity-data.com/topher/2020/April/5e8e1ab1_l2-ehr-code-sets/l2-ehr-code-sets.jpg "Context of a Medical Encounter")Context of a Medical Encounter](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/f071590a-6eff-4b14-aa4d-c9c66a773363#)

## Context of a Medical Encounter {#context-of-a-medical-encounter}

**Medical Encounter**: An interaction between a patient and healthcare provider\(s\) for the purpose of providing healthcare service\(s\) or assessing the health status of a patient.

This could also be online in today's world. During each encounter, one or more codes are generated about your encounter.

1. It is important to note that medical records often go through a process from a written note or some other unstructured input to a structured medical code that is standardized but can be very specific. There is a whole industry of medical coders that take this unstructured information and “code” it to medical code standards and into EHR records.

2. Another piece of information that is relevant to industry codes is whether the encounter occurred in an inpatient or outpatient setting.

   * Inpatient usually refers to hospitalization.
   * Outpatient refers to visits and encounters like visits with your primary care physician or specialists like a cardiologist but do not require hospitalization.
   * Outpatient can also refer to ambulatory care.

It is important to work with subject-matter experts who know the domain and processes at a much deeper level. There are a lot of intricacies and details in healthcare that require you to work deeply with SMEs to understand the context better.

However, you should feel empowered to bring the technology and AI perspective to the table and re-think legacy processes!

## Importance of Using Codes to Group/Categorize Your Data {#importance-of-using-codes-to-group-categorize-your-data}

As you will see in a bit there are literally thousands of medical codes in use and each time have a**Medical Encounter**, you have several of these codes added to that encounter. If you do not properly group/categorize your data, you may end up with missing or incorrect data to feed into your model. As an example let's say you knew one of the diagnosis codes for Sepsis and wanted to build a model around predicting which patients are at the greatest risk for Sepsis. Sepsis- sepsis during labor \(O75.3\) If you used only that code to build your dataset for training you are likely missing out on thousands of other records that also deal with Sepsis, but have a different code.

Here is a link to the Sepsis codes to see some others:[IDC10-CM Sepsis Codes](https://www.icd10data.com/ICD10CM/Codes/A00-B99/A30-A49/A41-/A41)

This would lead your model to be inaccurate. This concept will become clearer as dig deeper.

## Diagnosis Codes Key Points {#diagnosis-codes-key-points}

In Healthcare, the different diagnosis code sets are extremely important and can sometimes be tricky to deal with. But don't worry, when you are done with this section, you'll be a able to crack the code like a pro!

### What is a diagnosis? {#what-is-a-diagnosis-}

Let’s imagine that you see a doctor and you have some symptoms such as coughing and a stomach ache and then your doctor magically comes up with a diagnosis. As we will learn later this diagnosis is a key piece of information that connects so much of the encounter experience together.

### ICD10 {#icd10}

**ICD10**: International Classification of Diseases 10

* Also known as ICD10
* Standard developed by
  **WHO**
  : World Health Organization and in 10th revision

### ICD10-CM {#icd10-cm}

**ICD10-CM**: International Classification of Diseases 10 - Clinical Modification

Diagnosis code standard used in the U.S.

* Maintained by U.S. CDC
* Contains a wide variety of diseases and conditions
* Used for Medical claims, disease epidemic, and mortality tracking

[![](https://video.udacity-data.com/topher/2020/April/5e8e20d2_l2-ehr-code-sets-1/l2-ehr-code-sets-1.jpg "ICD9 to 10")ICD9 to 10](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/66c28cbc-955d-4e50-9a78-6d06193e3d36#)

### ICD9 to 10 {#icd9-to-10}

The U.S. had been using ICD9 since 1979 until it adopted ICD10 in October of 2014. The US lagged a bit behind the rest of the world who had adopted ICD10 much earlier. The reason for the changes from ICD9 to ICD10 is that the code set was not robust enough to meet future healthcare needs.

Important Changes:

* 14,025 codes =
  &gt;
   69,823 codes
* Up to 7 characters
* Created a much higher level of detail
  * Included things like laterality or side of injury

You can learn more by looking at the[CDC's Rationale](https://www.cdc.gov/nchs/icd/icd10cm_pcs_background.htm)

[![](https://video.udacity-data.com/topher/2020/April/5e8e2259_l2-ehr-code-sets-2/l2-ehr-code-sets-2.jpg "ICD10- CM Code Structure")ICD10- CM Code Structure](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/66c28cbc-955d-4e50-9a78-6d06193e3d36#)

## ICD10- CM Code Structure {#icd10-cm-code-structure}

### _\[ XXX \]_.XXX.X {#-xxx-xxx-x}

The first part is the category of the diagnosis and it is the first three characters and could be an S code like the injury category. There are 21 different categories and these range from the disease of the respiratory system to injury, poisoning, and certain other consequences of external causes.

### XXX._\[ XXX \]_.X {#xxx-xxx-x}

The Second is the etiology, anatomic site, and manifestation part which can be up to 3 more characters and is essentially the cause for a condition or disease or the location of the condition.

### XXX.XXX._\[ X \]_ {#xxx-xxx-x-}

Finally, the third part is the extension which is the last character and can be tricky b/c it can often be null or has an X placeholder. It is often used with injury-related codes referring to the episode of care.

#### Additional Resources {#additional-resources}

[Code Structure](https://library.ahima.org/doc?oid=106177#.Xm70u5NKhQI)[Coding Guidelines](https://www.cms.gov/Medicare/Coding/ICD10/Downloads/2019-ICD10-Coding-Guidelines-.pdf)

## Diagnosis Codes Key Points {#diagnosis-codes-key-points}

First, it is important to note that it can take several encounters for a patient to receive a diagnosis, and therefore a diagnosis code. There may be an initial appointment, some follow-up testing, and then another appointment before a diagnosis is determined. This is very common. This means that there may not be diagnosis codes for every encounter.

Encounter 1: G30.9, M06.9,~~**G30.9**~~

Second, a diagnosis code should never be repeated in the same encounter.

* Encounter 1:
  **G30.9**
  , M06.9
* Encounter 2:
  **G30.9**
  , M06.9, A34.6

Third, it is expected that a diagnosis would carry across different encounters as the patient is being treated for that diagnosis.

#### Additional Resources {#additional-resources}

In the video, we showed you the CDC lookup tool for ICD10-CM codes. The link is below. Go ahead and look up some codes you might be familiar with!

* [CDC ICD10-CM Lookup Tool](https://icd10cmtool.cdc.gov/?fy=FY2019)
* [Coronavirus ICD Code Announcement](https://www.cdc.gov/nchs/data/icd/Announcement-New-ICD-code-for-coronavirus-2-20-2020.pdf)

[![](https://video.udacity-data.com/topher/2020/April/5e8e3c2a_l2-ehr-code-sets-4/l2-ehr-code-sets-4.jpg)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/3277cb18-1de1-49c3-b42e-3287aae3fde0#)

### Diagnosis Code Prioritization {#diagnosis-code-prioritization}

At a high level, it is important to distinguish what code is taking up the most resources or is the most critical and there are few terms that you should become familiar with.

* **Primary Diagnosis Code**
  : The code that takes up the most resources to treat.
* **Principal Diagnosis Code**
  : The diagnosis that is found after hospitalization to be the one that is chiefly responsible.

This**can be**an important distinction since the admitting diagnosis code can widely differ from the final, Principal Diagnosis. For the most part, these terms interchangeably but it's good to be aware of the differences and the need to dig into the details when necessary.

* **Secondary Diagnosis Codes**
  : The other diagnosis codes listed on an encounter.

For example, if a patient were to have a knee replacement surgery but had type 2 diabetes as a prior condition, the secondary diagnosis code of type 2 diabetes would be included in the medical record.

**Note**: Secondary diagnoses codes can include many additional codes

#### Additional Resources {#additional-resources}

* [ICD10 -CM Coding Guidelines](https://www.cms.gov/Medicare/Coding/ICD10/Downloads/2019-ICD10-Coding-Guidelines-.pdf)
* [Primary, Principal, and Secondary Diagnosis Codes](https://www.hcpro.com/HIM-324035-5707/QA-Primary-principal-and-secondary-diagnoses.html)

**Procedure Codes**: the categorization of the medical codes during an encounter. It's important to note if a procedure is inpatient or outpatient.

[![](https://video.udacity-data.com/topher/2020/April/5e8f6b65_l2-ehr-code-sets-3/l2-ehr-code-sets-3.jpg "Key Procedure Code Sets")Key Procedure Code Sets](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/9e9ef750-5450-48f6-8f34-1870cd460225#)

### Key Procedure Code Sets {#key-procedure-code-sets}

The graphic above shows some additional information about 3 of the important code sets. Here are the key points about each of them.

* **ICD10 PCS**
  : Procedure Coding Systems
  * Only for
    **Inpatient**
  * 72,000+ codes as of 2019
  * Focus on medical and surgical
* **CPT**
  : Current Procedural Terminology
  * **Outpatient**
    focused but can apply to physician visits in ambulatory settings
  * 10,000+ codes as of 2019
  * Focus on professional services by physician
* **HCPCS**
  : Healthcare Common Procedure Coding System
  * Inpatient and outpatient
  * Has 3 levels
    * L1: CPT Codes
    * L2: Non-physician services
      * DME: Durable Medical Equipment
      * Ambulatory Services
      * Dental
    * L3: Medicare/Medicaid related

We will focus on ICD-10 PCS and CPT codes, but it's good to be aware of HCPCS codes, too!

[![](https://video.udacity-data.com/topher/2020/April/5e8f8994_l2-ehr-code-sets-6/l2-ehr-code-sets-6.jpg "ICD10 PCS Selections")ICD10 PCS Selections](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/9e9ef750-5450-48f6-8f34-1870cd460225#)

### ICD10-PCS Code Structure {#icd10-pcs-code-structure}

* 7 alphanumeric characters \[A-Z, 0-9\]
* 1st character is the Section
  * Reference Section codes categories in the table above
* Subsequent characters relate to the
  _Section_
  and give:
  * Body System
  * Body Part
  * Approach
  * Device used for a procedure

#### Additional Resources {#additional-resources}

* [PCS Procedure Code System](https://www.cms.gov/Medicare/Coding/ICD10/Downloads/2014-pcs-procedure-coding-system.pdf)

[![](https://video.udacity-data.com/topher/2020/April/5e8f8953_l2-ehr-code-sets-4/l2-ehr-code-sets-4.jpg "Coronary Heart Code Example: 027004Z")Coronary Heart Code Example: 027004Z](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/9e9ef750-5450-48f6-8f34-1870cd460225#)

## PCS Code Example {#pcs-code-example}

For this example we used 027004Z. This is a Heart Surgery code that relates to:

* Dilation of Coronary Artery
* One Site with Drug-eluting Intraluminal Device
* Open Approach

You can see the breakdown in the image above. In the table, you can see that this code starts with**0**, which is the**Medical**category. If you look over the link from the last section,[PCS Procedure Code System](https://www.cms.gov/Medicare/Coding/ICD10/Downloads/2014-pcs-procedure-coding-system.pdf), you can further see where the rest of the code breaks down.

* 0 = Medical
* 2 = Heart and Great Vessels
* 7 = Dilation
* 0 = Coronary Artery, One Site
* 0 = Open Approach
* 4 = Drug-eluting Intraluminal Device
* Z = No Qualifier

#### Additional Resources {#additional-resources}

* [PCS Overview](https://www.tacomacc.edu/userfiles/servers/server_6/file/him/him240/pcsoverview/pcsoverview4)
* [Medicare Coding](https://www.cms.gov/Medicare/Coding/ICD10/Downloads/2016-PCS-Slides.pdf)

[![](https://video.udacity-data.com/topher/2020/April/5e8f8960_l2-ehr-code-sets-5/l2-ehr-code-sets-5.jpg "CPT Category 1 Billable Codes")CPT Category 1 Billable Codes](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/9e9ef750-5450-48f6-8f34-1870cd460225#)

## CPT Codes {#cpt-codes}

CPT codes have the following structure:

* Up to 5 numbers
* 3 Categories
  * Category 1: Billable codes
    * 10 sections largely split along biological systems which are broken out in the image above
  * Category 2:
    * Five digits that end in F
    * Non-billable
    * Performance measure focused on physicals and patient history
  * Category 3:
    * Services and procedures using emerging technology

Generally, you'll focus on the first two categories.

As a reminder, we will not be covering HCPCS Codes. However, we have added some additional links regarding HCPCS codes below.

#### Additional Links {#additional-links}

* [Understanding CPT Codes](https://www.aapc.com/resources/medical-coding/cpt.aspx)
* [CMS.gov Database Download for CPT Codes](https://www.cms.gov/medicare-coverage-database/license/cpt-license.aspx?from=~/overview-and-quick-search.aspx&npage=/medicare-coverage-database/downloads/downloadable-databases.aspx)
* [CMS.gov HCPCS Codes Information](https://www.cms.gov/Medicare/Coding/MedHCPCSGenInfo)
* [HCPCS Wikipedia](https://en.wikipedia.org/wiki/Healthcare_Common_Procedure_Coding_System)

## Medication Code Key Points {#medication-code-key-points}

**NDC**: National Drug Code

In this section, we discussed the NDC Codes. These codes have been in place since 1972, and are maintained by the FDA.

### NDC Code Structure {#ndc-code-structure}

* 10- to 11-digit code
  * multiple configurations
* 3 Parts
  * Labeler: Drug manufacturer
  * Product code: the actual drug details
  * Package code: form and size of medication

[![](https://video.udacity-data.com/topher/2020/April/5e8f8a68_l2-ehr-code-sets-7/l2-ehr-code-sets-7.jpg)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/5d48e35a-a785-4642-bad7-631cc878ce9f#)

In the image above you, can see the first part of the NDC code for_Tecentriq_.

The first part of the code**50242**is the**Labeler**, which maps to the manufacturer of the drug \(which in this case is**Genentech, Inc**\).

[![](https://video.udacity-data.com/topher/2020/April/5e8f8a73_l2-ehr-code-sets-8/l2-ehr-code-sets-8.jpg "NDC Product Code")NDC Product Code](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/5d48e35a-a785-4642-bad7-631cc878ce9f#)

In the example above, we are looking at the middle section of the code**917**.

**917**is the**Product Code**. In this case, it takes the unpronounceable, Non-Proprietary Name,**ATEZOLIZUMAB 1200mg/20ml**and maps it to the Proprietary Name,**Tecentriq**. It also indicates that the drug dosage form is**Injection, Solution**and the route of administration is**Intravenous**.

It is important to note that you may need to use the Non-Proprietary Name in order to group a drug by all of its generics.

[![](https://video.udacity-data.com/topher/2020/April/5e8f8a7e_l2-ehr-code-sets-9/l2-ehr-code-sets-9.jpg "NDC Packaging Code")NDC Packaging Code](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/5d48e35a-a785-4642-bad7-631cc878ce9f#)

Finally, the last two digits**01**.

This is the**packaging code**, and in this instance indicates that the package is a 14ml single vial in 1 carton for single use.

[![](https://video.udacity-data.com/topher/2020/April/5e8f8aa3_l2-ehr-code-sets-10/l2-ehr-code-sets-10.jpg "HCPCS Crosswalk")HCPCS Crosswalk](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/5d48e35a-a785-4642-bad7-631cc878ce9f#)

### Crosswalk {#crosswalk}

**Crosswalk**: A connection between two different code sets or versions of drugs in the same code set.

A common term you will hear for medical code sets is a crosswalk.

For NDC codes we can connect them to HCPCS codes and depending on the data source you are looking at there may be a mapping from these codes.

For the previous example, there is a crosswalk between the Tecentriq NDC code and the HCPCS code J9022. Something to note is that the HCPCS code starts with a J and the “J” codes tend to be drugs that are injected but by definition, they are drugs that are not taken orally. You can see that the HCPCS code maps some info from the NDC code.

[![](https://video.udacity-data.com/topher/2020/April/5e8f8a8e_l2-ehr-code-sets-11/l2-ehr-code-sets-11.jpg "NDC Code Challenges")NDC Code Challenges](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/5d48e35a-a785-4642-bad7-631cc878ce9f#)

### Challenge with NDC Codes {#challenge-with-ndc-codes}

One of the major challenges with NDC codes is to normalize or group codes by common types of drugs. For example, let’s take acetaminophen. You would think this would be relatively straightforward to group whenever someone has the NDC code for this drug. However, as you can see from this table of only a sample of the results from[NDC List](http://ndclist.com/)that there many drug codes that could contain acetaminophen with other drugs too. This can make drugs very difficult to deal with.

#### Additional Resources: {#additional-resources-}

[NDC List](http://ndclist.com/)

[![](https://video.udacity-data.com/topher/2020/April/5e8f8ab0_l2-ehr-code-sets-12/l2-ehr-code-sets-12.jpg "RXNorm Example")RXNorm Example](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/5d48e35a-a785-4642-bad7-631cc878ce9f#)

### RXNorm {#rxnorm}

To address the problem just mentioned, the NIH developed a normalized naming system called**RXNorm**, which does what its name implies and groups medication together. This is important because providers, pharmacies, payers all send EHR records with this data but might use different names and it becomes difficult to communicate between different systems.

To illustrate this issue, take a look at a drug Naproxen and the just a few examples of different names of naproxen that is the same thing,

While there is a crosswalk between NDC codes and RXNorm, there are still some issues. Depending on the system you are dealing with, it could use one or the other code set.

## Grouping/Categorizing Key Points {#grouping-categorizing-key-points}

**CCS**- Clinical Classifications Software

As mentioned earlier, there is a tremendous challenge of taking the 77K+ ICD10 PCS codes and categorizing them into meaningful categories at scale. This is where a government-industry partnership called the Healthcare Cost and Utilization Project \(HCUP\) created a categorization system called clinical classifications software or CCS. It can be used to map diagnosis or procedure codes from ICD code sets. It has single or multi-level options for mapping these codes.

### Single Level Categories {#single-level-categories}

* Mutually exclusive categories
* 285 categories for diagnoses
* 231 categories for procedures

As an example using single-level codes:

* **Operations on the cardiovascular system **are codes **43-63**
* **Heart valve procedures **is code **43**
  .

Below you can see how that changes using multi-level codes.

Single level categories can be used for ranking of codes and help with risk adjustment scoring

[![](https://video.udacity-data.com/topher/2020/April/5e90882a_l2-ehr-code-sets-13/l2-ehr-code-sets-13.jpg "CCS Multi-level Categories")CCS Multi-level Categories](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/2e1be139-4d7d-4fce-ac3a-9f3b93aadb30#)

### Multi-Level Categories {#multi-level-categories}

CCS Multi-level categories are helpful for more detailed analysis and grouping at a more granular level. there are 4 levels for diagnosis codes and 3 levels for procedure codes.

In the above example,**7**is a broad category, but then you can see that it branches off into**7.1**and**7.2**into more detail.**7.1**.

Then**7.1.X.X**further break down hypertension.

**7.1.2.1**= Hypertensive heart and/or renal disease \(_typo in image above_\).

[![](https://video.udacity-data.com/topher/2020/April/5e90884f_l2-ehr-code-sets-14/l2-ehr-code-sets-14.jpg "CCS ICD10 - PCS Category Mapping File ")CCS ICD10 - PCS Category Mapping File[CCS website](https://www.hcup-us.ahrq.gov/toolssoftware/ccs10/ccs10.jsp)](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/76f4c48c-a664-4750-8753-5811c854d02a/concepts/2e1be139-4d7d-4fce-ac3a-9f3b93aadb30#)

### CCS ICD10-PCS Category Mapping File {#ccs-icd10-pcs-category-mapping-file}

Below we have included a link to the CCS ICD10-PCS Category Mapping File which makes figuring out and working with these category groupings much easier._You download the mapping file for taking ICD10 PCS procedure codes and map them to grouping/categorization with CCS._

The mapping file breaks down like this \(image above\):

* ICD10 codes and their descriptions on the left
* CCS category descriptions, Multi CCS LVL 1, and Multi Lvl 1 Label on the right.

It would be hard to have a medical expert categorize these at and they can change over time. Thankfully CCS maps these for us. These mappings can be helpful in reducing dimensionality.

### Other Categorization Systems {#other-categorization-systems}

* **MS- DRG**
  * Medicare Severity-Diagnosis Related Group
  * Group payment based on the principal diagnosis
  * Up to 25 secondary dx
  * Up to 25 procedures during a visit/encounter
* **SNOMED CT**
  * Systematized Nomenclature of Medicine—Clinical Terms
  * License to use
  * Helpful for making the EHR records interoperable

We will not be using these two in this course and there are many other systems as well, but knowing that they exist is good.

#### Additional Resources {#additional-resources}

* [CCS website](https://www.hcup-us.ahrq.gov/toolssoftware/ccs10/ccs10.jsp)
* [MS-DRG](https://www.cms.gov/Medicare/Medicare-Fee-for-Service-Payment/AcuteInpatientPPS/MS-DRG-Classifications-and-Software)
* [SNOMED CT](http://www.snomed.org/snomed-ct/five-step-briefing)



