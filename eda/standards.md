## Key Points {#key-points}

[![](https://video.udacity-data.com/topher/2020/April/5e8cdc52_l1-ehr-data-security-analysis-2/l1-ehr-data-security-analysis-2.jpg "Key Industry Regulations")Key Industry Regulations](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/eaf98312-bcfb-473d-abf1-0d78164a561f/concepts/ea8a3015-ff85-43e1-a84d-c1bf378f1d30#)

## Key Industry Regulations {#key-industry-regulations}

Healthcare data security and privacy regulations are ubiquitous around the world. Most countries have regulations regarding data security and privacy around electronic health records with varying levels of complexity. However, for this course, we will focus on U.S. healthcare regulations.

### United States: {#united-states-}

* **HIPAA**
  : The Health Insurance Portability and Accountability Act is the key industry regulation that you should be familiar within the U.S.
* **HITECH**
  : The Health Information Technology for Economic and Clinical Health Act is also important to note and this is really just an update to HIPAA that accounts for technology.

HIPAA was passed in 1996 and then updated in 2009 under the HITECH act to evolve for digital technologies. While it might seem like EHR has been around for a while, it was only a little over a decade ago when the foundational legislation really was put in place that made the healthcare community start moving towards digitizing and putting their records in electronic format. Even now many systems are legacy systems that are built off of transcribed handwritten notes and other unstructured formats that organizations are spending large amounts of time to normalize and aggregate this info.

### **EU**: European Union {#eu-european-union}

* **GDPR**
  : The General Data Protection Regulation is generally considered more stringent than even HIPAA when it comes to protections for patients.

### United Kingdom: {#united-kingdom-}

* **DPA**
  : The Data Protection Act really builds off of and add to GDPR

#### Additional Resources: {#additional-resources-}

* [HIPAA](https://www.hhs.gov/hipaa/index.html)
* [HITECH](https://www.hhs.gov/hipaa/for-professionals/special-topics/hitech-act-enforcement-interim-final-rule/index.html)
* [GDPR](https://gdpr-info.eu/)
* [DPA](https://www.gov.uk/data-protection)

## PHI {#phi}

**PHI**: Protected Health Information

Probably one of the most important terms that you should be familiar with is PHI, which stands for protected health information. PHI is a part of HIPAA that protects the transmission of certain types of personally identifiable information such as name, address, and other info. Certain information in an electronic medical record is considered PHI and must comply with HIPAA standards around data security and privacy. This informs not only how you transmit and store data but also data usage rights and restrictions around building models for other purposes than the original use.

PHI by nature is identifiable information and can be used to easily identify a person. However, please note that**omitting this information does not ensure that a person can not be identified**.

#### Additional Resources {#additional-resources}

[PHI](https://www.hhs.gov/answers/hipaa/what-is-phi/index.html)

[![](https://video.udacity-data.com/topher/2020/April/5e8b8de9_l1-ehr-data-security-analysis/l1-ehr-data-security-analysis.jpg "Covered Entities")Covered Entities](https://classroom.udacity.com/nanodegrees/nd320-beta/parts/2ca838f8-e10d-4038-8426-d47eb4a20a62/modules/1644460b-a828-4443-ad8c-bbcca3151a30/lessons/eaf98312-bcfb-473d-abf1-0d78164a561f/concepts/ea8a3015-ff85-43e1-a84d-c1bf378f1d30#)

## Covered Entities {#covered-entities}

**Covered Entities**: are a group of industry organizations defined by HIPAA to be one of three groups: health insurance plans, providers, or clearinghouses. You can see from the table the types of entities in each category.

These groups transmit protected health information and are subject to HIPAA regulations regarding these transmissions.

Other Covered Entities:

* **Business Associates**: A business associate is a person or entity that performs certain functions or activities that involve the use or disclosure of protected health information on behalf of, or provides services to, a covered entity.

  * Covered entities can disclose to BAs under Privacy Rule
    * Only for a purpose allowed by the covered entity
    * Safeguard data against misuse
    * Comply with other requirements of the covered entity under HIPAA Privacy Rule

* **Business Associates Agreement/Addendum \(BAA\)**
  : this is the contract between a covered entity and BA

#### Additional Resources {#additional-resources}

[Covered Entites](https://www.hhs.gov/hipaa/for-professionals/covered-entities/index.html)[Sample Business Associate Agreement](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html)[Business Associate Guidance](https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html)

