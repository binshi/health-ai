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



