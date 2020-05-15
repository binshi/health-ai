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



