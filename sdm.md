# Source Data Model

* * *
## PERSON

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
person_id | Yes | Yes| Integer | A unique identifier for each person; this is created by each contributing site. | <p>This is not a value found in the EHR.</p> PERSON_ID must be unique for all patients within a single data set.</p><p>A mapping from the person_id to a real patient ID or MRN from the source EHR must be kept at the local site. This mapping is not shared with the data coordinating center. It is used only by the site for re-identification for study recruitment or for data quality review.</p>
gender_concept_id | Yes |Yes|  Integer |  A foreign key that refers to a standard concept identifier in the Vocabulary for the gender of the person. | Please include valid concept ids (consistent with OMOP CDMv5.1). Predefined value set (valid concept_ids found in CONCEPT table select \* from concept where ((domain_id='Gender' and concept_class_id='Gender')or (domain_id='Observation' and vocabulary_id='PCORNet' and concept_class_id in ('Gender','Undefined'))) and concept_code not in ('Sex-F','Sex-M') and invalid_reason is null: <ul><li>Ambiguous: concept_id = 44814664 </li> <li>Female: concept_id = 8532</li> <li>Male: concept_id = 8507</li> <li>No Information: concept_id = 44814650 (Vocabulary_id='PCORNet')</li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul>
gender_source_concept_id | Yes | Yes|  Integer | A foreign key to the gender concept that refers to the code used in the source.|  <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
year_of_birth | Yes |Yes|  Integer |  The year of birth of the person. | <p>For data sources with date of birth, the year is extracted. For data sources where the year of birth is not available, the approximate year of birth is derived based on any age group categorization available.</p> Please keep all accurate/real dates (No date shifting)
month_of_birth | No |Provide When Available|  Integer | The month of birth of the person. | <p>For data sources that provide the precise date of birth, the month is extracted and stored in this field.</p> Please keep all accurate/real dates (No date shifting)
day_of_birth | No |Provide When Available|  Integer | The day of the month of birth of the person. | <p>For data sources that provide the precise date of birth, the day is extracted and stored in this field.</p> Please keep all accurate/real dates (No date shifting)
birth_date | No |Provide When Available|  Date |  The birth date | Full date. Please keep all accurate/real dates (No date shifting).
birth_datetime | No |Provide When Available|  Datetime |  The birth date and time | <p>Do not include timezone.</p> Please keep all accurate/real dates (No date shifting). If there is no time associated with the date assert midnight.
race_concept_id | Yes |Yes|  Integer |  A foreign key that refers to a standard concept identifier in the Vocabulary for the race of the person. | Details of categorical definitions: <ul><li>**-American Indian or Alaska Native**: A person having origins in any of the original peoples of North and South America (including Central America), and who maintains tribal affiliation or community attachment.</li> <li>**-Asian**: A person having origins in any of the original peoples of the Far East, Southeast Asia, or the Indian subcontinent including, for example, Cambodia, China, India, Japan, Korea, Malaysia, Pakistan, the Philippine Islands, Thailand, and Vietnam.</li> <li>**-Black or African American**: A person having origins in any of the black racial groups of Africa.</li> <li>**-Native Hawaiian or Other Pacific Islander**: A person having origins in any of the original peoples of Hawaii, Guam, Samoa, or other Pacific Islands.</li> <li>**-White**: A person having origins in any of the original peoples of Europe, the Middle East, or North Africa.</li></ul> <p>For patients with multiple races (i.e. biracial), race is considered a single concept, meaning there is only one race slot. If there are multiple races in the source system, concatenate all races into one race_source_value (see below) and use concept_id code as 'Multiple Race.'</p> Predefined values (valid concept_ids found in CONCEPT table where ((domain_id='Race' and vocabulary_id = 'Race') or (vocabulary_id='PCORNet' and concept_class_id='Undefined') or concept_id in (44814659,44814660)) and invalid_reason is null: <ul><li>American Indian/Alaska Native: concept_id = 8657</li> <li>Asian: concept_id = 8515</li> <li>Black or African American: concept_id = 8516</li> <li>Native Hawaiian or Other Pacific Islander: concept_id = 8557</li> <li>White: concept_id = 8527</li> <li>Multiple Race: concept_id = 44814659 (vocabulary_id='PCORNet')</li> <li>Refuse to answer: concept_id = 44814660 (vocabulary_id='PCORNet')</li> <li>No Information: concept_id = 44814650 vocabulary_id='PCORNet')</li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul>
race_source_concept_id| Yes |Yes|  Integer| A foreign key to the race concept that refers to the code used in the source.| <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
ethnicity_concept_id | Yes | Yes|  Integer | A foreign key that refers to the standard concept identifier in the Vocabulary for the ethnicity of the person. | <p>A person with Hispanic ethnicity is defined as "A person of Cuban, Mexican, Puerto Rican, South or Central American, or other Spanish culture or origin, regardless of race."</p> Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id ='Ethnicity' or (vocabulary_id=PCORNet and concept_class_id='Undefined) where noted): <ul><li>Hispanic: concept_id = 38003563</li> <li>Not Hispanic: concept_id = 38003564</li> <li>No Information: concept_id = 44814650 (vocabulary_id='PCORNet')</li> <li>Unknown: concept_id = 44814653 (vocabulary_id='PCORNet')</li> <li>Other: concept_id = 44814649 (vocabulary_id='PCORNet')</li></ul>
ethnicity_source_concept_id | Yes | Yes|Integer | A foreign key to the ethnicity concept that refers to the code used in the source.| <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
location_id | No |Provide When Available| Integer |  A foreign key to the place of residency (ZIP code) for the person in the location table, where the detailed address information is stored.
provider_id | No |Provide When Available| Integer |  Foreign key to the primary care provider the person is seeing in the provider table.| <p>Sites will use site-specific logic to determine the best primary care provider and document how that decision was made (e.g., billing provider).</p>
care_site_id | Yes |Yes| Integer |  A foreign key to the site of primary care in the care_site table, where the details of the care site are stored | For patients who receive care at multiple care sites, use site-specific logic to select a care site that best represents where the patient obtains the majority of their recent care. If a specific site within the institution cannot be identified, use a care_site_id representing the institution as a whole.
pn_gestational_age | No |Provide When Available| Integer |  The post-menstrual age in weeks of the person at birth, if known | Use granularity of age in weeks as is recorded in local EHR.
person_source_value | No |Provide When Available|  Varchar |  An encrypted key derived from the person identifier in the source data. | <p>Insert a unique pseudo-identifier (random number, encrypted identifier) into the field. Do not insert the actual MRN or PAT_ID from your site. A mapping from the pseudo-identifier for person_source_value in this field to a real patient ID or MRN from the source EHR must be kept at the local site. This mapping is not shared with the data coordinating center. It is used only by the site for re-identification for study recruitment or for data quality review.</p>
gender_source_value | Yes |Yes|  Varchar |  The source code for the gender of the person as it appears in the source data. | The person's gender is mapped to a standard gender concept in the Vocabulary; the original value is stored here for reference. See gender_concept_id
race_source_value | Yes |Yes|  Varchar |  The source code for the race of the person as it appears in the source data. | <p>The person race is mapped to a standard race concept in the Vocabulary and the original value is stored here for reference.</p> For patients with multiple races (i.e. biracial), race is considered a single concept, meaning there is only one race slot. If there are multiple races in the source system, concatenate all races into one source value, and use the concept_id for Multiple Race.
ethnicity_source_value | Yes |Yes|  Varchar |  The source code for the ethnicity of the person as it appears in the source data. | The person ethnicity is mapped to a standard ethnicity concept in the Vocabulary and the original code is, stored here for reference.
language_concept_id|Yes| Yes| Integer | A foreign key that refers to the standard concept identifier in the Vocabulary for the language of the person.| <p>Please map your source codes to acceptable language values in appendix 2. **If there is not a mapping for the source code in the network language mapping, use concept_id = 44814649 (Other PCORNet Vocabulary)**</p>|
language_source_concept_id| Yes| Yes | Integer | A foreign key to the language concept that refers to the code used in the source.|<p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
language_source_value| Yes| Yes| Varchar | The source code for the language of the person as it appears in the source data | The person language is mapped to a standard language concept in the Vocabulary and the original code is stored here for reference.

## 2 DEATH

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
death_cause_id | Yes | Yes | Integer | A unique identifier for each death cause occurrence | This is not a value found in the EHR. Sites may choose to use a sequential value for this field
person_id | Yes | Yes |   Integer | A foreign key identifier to the deceased person. The demographic details of that person are stored in the person table.| See PERSON.person_id (primary key)
death_date | Yes |Yes|   Date | The date the person was deceased. | <p>If the precise date including day or month is not known or not allowed, December is used as the default month, and the last day of the month the default day. If no date available, use date recorded as deceased.</p> When the date of death is not present in the source data, use the date the source record was created.
death_datetime | Yes |Yes|   Datetime | The date the person was deceased. |<p>If the precise date including day or month is not known or not allowed, December is used as the default month, and the last day of the month the default day. If no date available, use date recorded as deceased.</p> <p>When the date of death is not present in the source data, use the date the source record was created. If there is no time associated with the date assert '23:59:59'.</p>
death_type_concept_id | Yes | Yes|  Integer | A foreign key referring to the predefined concept identifier in the Vocabulary reflecting how the death was represented in the source data. | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where domain_id ='Death Type')</p> <p>select \* from concept where concept_class_id ='Death Type' yields 9 valid concept_ids. If none are correct, use concept_id = 0</p> <p>Note: Most current ETLs are extracting data from EHR. The common concept_id to insert here is <ul><li>38003569 ("EHR record patient status "Deceased")</li></ul>. Please assert <ul><li>No information: concept_id =  44814650</li></ul> where there is no information in the source</p> **Note**: These terms only describe the source from which the death was reported. It does not describe our certainty/source of the date of death, which may have been created by one of the heuristics described in death_date.
cause_concept_id | No |Provide When Available|   Integer | A foreign referring to a standard concept identifier in the Vocabulary for conditions. | 
cause_source_value | No |Provide When Available|   Varchar | The source code for the cause of death as it appears in the source. This code is mapped to a standard concept in the Vocabulary and the original code is stored here for reference.
cause_source_concept_id | No |Provide When Available| Integer | A foreign key to the vocabulary concept that refers to the code used in the source.| This links to the concept id of the vocabulary of the cause of death concept id as stored in the source. For example, if the cause of death is "Acute myeloid leukemia, without mention of having achieved remission" which has an icd9 code of 205.00 the cause source concept id is 44826430 which is the icd9 code concept that corresponds to the diagnosis 205.00.  <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
death_impute_concept_id| Yes | Yes| Varchar | A foreign key referring to a standard concept identifier in the vocabulary for death imputation. | p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where concept_class_id ='Death Imput Type')</p> <p>select \* from concept where (concept_class_id ='Death Imput Type' or (vocabulary_id='PCORNet' and concept_class_id='Undefined')) and invalid_reason is null yields 8 valid concept_ids. If none are correct, use concept_id = 0</p> <ul> <li>Both month and day imputed: 2000000034</li><li>Day imputed: 2000000035</li><li>Month imputed: 2000000036</li><li>Full Date imputed: 2000000038</li><li>Not imputed:2000000037 </li> <li>No Information: concept_id = 44814650 (Vocabulary_id='PCORNet')</li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul></p>

## 3 LOCATION

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
location_id | Yes | Yes | Integer | A unique identifier for each geographic location. | This is not a value found in the EHR. Sites may choose to use a sequential value for this field
city | Yes |Provide When Available |Varchar | The city field as it appears in the source data.
state | No |Provide When Available|  Varchar | The state field as it appears in the source data.
zip | No|Provide When Available|  Varchar | The zip code. For US addresses, valid zip codes can be 3, 5 or 9 digits long, depending on the source data. | While optional, this is the most important field in this table to support location-based queries.
location_source_value | No |Provide When Available|  Varchar | The verbatim information that is used to uniquely identify the location as it appears in the source data. | <p>If location source values are deemed sensitive by your organization, insert a pseudo-identifier (random number, encrypted identifier) into the field. Sites electing to obfuscate location_source_values will keep the mapping between the value in this field and the original clear text location source value. This value is only used for site-level re-identification for study recruitment and for data quality review.</p> Sites may consider using the location_id field value in this table as the pseudo-identifier as long as a local mapping from location_id to the real site identifier is maintained.
address_1 | No | NO| Varchar | |Do not transmit
address_2 | No | NO| Varchar | | Do not transmit
county | No |NO| Varchar | |Do not transmit

## 4 CARE_SITE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
care_site_id | Yes | Yes | Integer | A unique identifier for each defined location of care within an organization. Here, an organization is defined as a collection of one or more care sites that share a single EHR database. | <p>This is not a value found in the EHR.</p> Sites may choose to use a sequential value for this field
care_site_name | No |Provide When Available|  Varchar | The description of the care site | 
place_of_service_concept_id | No |Provide When Available|   Integer | A foreign key that refers to a place of service concept identifier in the Vocabulary | <p>Please include valid concept ids (consistent with OMOP CDMv5.1). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id = 'CMS Place of Service' and invalid_reason is null)</p> <p>select \* from concept where vocabulary_id = 'CMS Place of Service' and invalid_reason is null yields 49 valid concept_ids.</p> Please use the following value set: <ul><li><b>Urgent Care Facility = 	8782</b></li><li>Rural Health Clinic	 = 8761</li><li>Outpatient (Examples: Hospital	Dialysis, HOD, Day Hospital, Day Medicine) = 8756	</li><li>Office	=8940	</li><li>Inpatient Psychiatric Facility	=8971	</li><li>Inpatient Hospital	=8717	</li><li>Independent Clinic	=8716	</li><li>Emergency Room - Hospital = 8870	</li><li>Other Place of Service	=8844	</li><li>Other Inpatient Care	=8892	</li><li>Unknown: concept_id = 44814653 </li> <li>Other: concept_id =  44814649 </li> <li>No information: concept_id =  44814650</li></ul>
location_id | No |Provide When Available|   Integer | A foreign key to the geographic location of the administrative offices of the organization in the location table, where the detailed address information is stored.
care_site_source_value | Yes  | Yes|  Varchar | The identifier for the organization in the source data, stored here for reference. | <p>If care site source values are deemed sensitive by your organization, insert a pseudo-identifier (random number, encrypted identifier) into the field. Sites electing to obfuscate care site_source_values will keep the mapping between the value in this field and the original clear text location source value. This value is only used for site-level re-identification for study recruitment and for data quality review.</p> <p>For EPIC EHRs, map care_site_id to Clarity Department.</p> Sites may consider using the care_site_id field value in this table as the pseudo-identifier as long as a local mapping from care_site_id to the real site identifier is maintained.
place_of_service_source_value | No |Provide When Available|  Varchar | The source code for the place of service as it appears in the source data, stored here for reference.
specialty_concept_id|No|Provide When Available| Integer|The specialty of the department linked to a standard specialty concept as it appears in the Vocabulary | <p>Care sites could have one or more specialties or a Care site could have no specialty information.</p><p>**Valid specialty concept ids are found in the appendix.**</p><p>**Please use the following rules:**</p><ul><li><p> If care site specialty information is unavailable, please follow the convention on reporting values that are unknown,null or unavailable. </p></li><li><p> If a care site has a single specialty associated with it, sites should link the specialty to the **valid specialty concepts as assigned in the appendix.**. If the specialty does not correspond to a value in this listing, please use the NUCC Listing (vocabulary_id='NUCC') provided in the vocabulary as a reference. </p></li><li><p> If there are multiple specialties associated with a particular care site and sites are not able to assign a specialty value on the visit occurrence level, sites should use the specialty concept id=38004477 "Pediatric Medicine". </p></li><li><p> If there are multiple specialties associated with a particular care site and this information is attainable, sites should document the strategy used to obtain this information and the strategy used to link the correct care site/specialty pair for each visit occurrence. Sites should also link the specialty to the **valid specialty concepts as assigned in the appendix.**</p> If the specialty does not correspond to a value in this listing, please use the NUCC Listing (vocabulary_id='NUCC') provided in the vocabulary as a reference. </li><li>If the speciality does not correspond to a value in the NUCC Listing and no value in the ABMS Listing, please use the Specialty listing (vocabulary_id=' Medicare Specialty') as a reference</li></ul>|
specialty_source_value| No |Provide When Available|  Varchar | The source code for the specialty as it appears in the source data, stored here for reference.

## 5 PROVIDER

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
provider_id | Yes |Yes|  Integer | A unique identifier for each provider. Each site must maintain a map from this value to the identifier used for the provider in the source data. | This is not a value found in the EHR. <p>A mapping from the provider_id to a real provider from the source EHR must be kept at the local site. This mapping is not shared with the data coordinating center. It is used only by the site for re-identification for study recruitment or for data quality review.</p> Sites should document who they have included as a provider.
provider_name | No | NO| Varchar | A description of the provider | DO NOT TRANSMIT
gender_concept_id | No | Provide When Available|Integer | The gender of the provider | A foreign key to the concept that refers to the code used in the source.|Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table select \* from concept where domain_id='Gender'): <ul><li>Ambiguous: concept_id = 44814664 </li> <li>Female: concept_id = 8532</li> <li>Male: concept_id = 8507</li> <li>No Information: concept_id = 44814650 (Vocabulary_id='PCORNet')</li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul>
specialty_concept_id | No | Provide When Available| Integer | A foreign key to a standard provider's specialty concept identifier in the Vocabulary. | <p>Please map the source data to the mapped provider specialty concept associated with the American Medical Board of Specialties as seen in **Appendix A1**. Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id in ('Medicare Specialty', 'ABMS','NUCC','custom'))</p> <p>select \* from concept where vocabulary_id in (' Medicare Specialty', 'ABMS','NUCC','custom') and invalid_reason is null yields 2200 valid concept_ids.</p> <p>If none are correct, use concept_id = 0</p> For providers with more than one specialty, use site-specific logic to select one specialty and document the logic used. For example, sites may decide to always assert the \*\*first\*\* specialty listed in their data source. As a first guide please use the ABMS and custom vocabulary specialty listing listing to map your specialtity values. If the specialty does not correspond to a value in these listings, please use the NUCC Listing (vocabulary_id='NUCC') provided in the vocabulary as a reference and the Specialty (vocabulary_id='Medicare Specialty') if no correspond value exists in the NUCC Listing.
care_site_id | Yes |Yes|  Integer | A foreign key to the main care site where the provider is practicing. | See CARE_SITE.care_site_id (primary key)
year_of_birth | No |Provide When Available| Integer | The year of birth of the provider||
NPI | No | Site Preference|Varchar | The National Provider Identifier (NPI) of the provider. |
DEA | No |Site Preference|  Varchar | The Drug Enforcement Administration (DEA) number of the provider. |
provider_source_value | Yes |Yes|  Varchar | The identifier used for the provider in the source data, stored here for reference. | <p>Insert a pseudo-identifier (random number, encrypted identifier) into the field. Do not insert the actual PROVIDER_ID from your site. A mapping from the pseudo-identifier for provider_source_value in this field to a real provider ID from the source EHR must be kept at the local site. This mapping is not shared with the data coordinating center. It is used only by the site for re-identification for study recruitment or for data quality review.</p> Sites may consider using the provider_id field value in this table as the pseudo-identifier as long as a local mapping from provider_id to the real site identifier is maintained.
specialty_source_value | No |Provide When Available|  Varchar | The source code for the provider specialty as it appears in the source data, stored here for reference. | Optional. May be obfuscated if deemed sensitive by local site.
specialty_source_concept_id | No |Provide When Available| Integer | A foreign key to a concept that refers to the code used in the source.| If providing this information, sites should document how they determine the specialty associated with the provider. **Valid specialty concept ids for are found in the appendix.** If the specialty does not correspond to a value in this listing, please use the NUCC Listing (vocabulary_id='NUCC') provided in the vocabulary as a reference.  <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
gender_source_value | No |Provide When Available| Varchar | The source value for the provider gender.
gender_source_concept_id | No |Provide When Available| Integer | The gender of the provider as represented in the source that maps to a concept in the vocabulary| <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>

## 6 VISIT_OCCURRENCE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
visit_occurrence_id | Yes |Yes|  Integer | A unique identifier for each person's visits or encounter at a healthcare provider. |<p>This is not a value found in the EHR.</p> VISIT_OCCURRENCE_ID must be unique for all patients within a single data set.</p><p>A mapping from the visit occurrence id to a real patient encounter from the source EHR must be kept at the local site. This mapping is not shared with the data coordinating center. It is used only by the site for re-identification for study recruitment or for data quality review.  Do not use institutional encounter ID.</p>
person_id | Yes|Yes|  Integer | A foreign key identifier to the person for whom the visit is recorded. The demographic details of that person are stored in the person table.
visit_start_date | Yes|Yes|  Date | The start date of the visit. | No date shifting. Full date.
visit_end_date | No |Provide When Available| Date | The end date of the visit. | <p>No date shifting. Full date.</p> <p>If this is a one-day visit the end date should match the start date.</p> If the encounter is on-going at the time of ETL, this should be null.
visit_start_datetime |Yes |Yes|  Datetime | The start date of the visit. | No date shifting.  Full date and time. **If there is no time associated with the date assert midnight for the start time**
visit_end_datetime | No |Provide When Available| Datetime | The end date of the visit. | <p>No date shifting.</p> <p>If this is a one-day visit the end date should match the start date.</p> If the encounter is on-going at the time of ETL, this should be null.  Full date and time. **If there is no time associated with the date assert 11:59:59 pm for the end time**
provider_id | No |Provide When Available|  Integer | A foreign key to the provider in the provider table who was associated with the visit. | <p>Use attending or billing provider for this field if available, even if multiple providers were involved in the visit. Otherwise, make site-specific decision on which provider to associate with visits and document.</p> **NOTE: this is NOT required in OMOP CDM v4, but appears in OMOP CDMv5.**
care_site_id | No |Provide When Available| Integer | A foreign key to the care site in the care site table that was visited. | See CARE_SITE.care_site_id (primary key)
visit_concept_id | Yes |Yes| Integer | A foreign key that refers to a place of service concept identifier in the vocabulary. | <p>**This field was previously called place_of_service_concept_id**</p><p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where (domain_id='Visit' and ~ 'Enc|Vis|Und')  or (vocabulary_id='PCORNet' and concept_class_id='Encounter Type' and not concept_code ~ '-ED\|-IP\|-AV') or (vocabulary_id='PCORNet' and concept_class_id='Undefined' and not concept_code ~ '-ED\|-IP\|-AV') and invalid_reason is null.</p> <p>select \* from concept where (domain_id='Visit' and ~ 'Enc|Vis|Und') or (vocabulary_id='PCORNet' and concept_class_id='Encounter Type' and not concept_code ~ '-ED\|-IP\|-AV') or (vocabulary_id='PCORNet' and concept_class_id='Undefined' and not concept_code ~ '-ED\|-IP\|-AV') and invalid_reason is null yields 11 valid concept_ids.</p> If none are correct, use concept_id = 0 <ul><li>Inpatient Hospital Stay: concept_id = 9201</li> <li> In person Ambulatory Visit with Physician: concept_id = 9202</li> <li> In person Ambulatory Visit with Non-Physician: concept_id = 2000000469</li> <li>Emergency Department: concept_id = 9203</li> <li>Long Term Care Visit = 42898160 </li><li>Other ambulatory Visit (Non in-person) = 44814711</li> <li>Non-Acute Institutional Stay: concept_id = 44814710 )</li><li>Emergency Department Admit to Inpatient Hospital Stay (If sites are unable to split the encounter) = 2000000048</li> <li>Observation Stay= 2000000088</li> <li>Administrative Visit= 2000000104</li> <li>Unknown: concept_id = 44814653 </li> <li>Other: concept_id =  44814649 </li> <li>No information: concept_id =  44814650</li></ul> 
visit_type_concept_id | Yes |Yes| Integer | A foreign key to the predefined concept identifier in the standard vocabulary reflecting the type of source data from which the visit record is derived.| <p> select \* from concept where concept_class_id='Visit Type' yields 3 valid concept_ids.</p> If none are correct, user concept_id=0. The majority of visits should be type 'Visit derived from EHR record' which is concept_id=44818518
visit_source_value | No |Provide When Available| Varchar | The source code used to reflect the type or source of the visit in the source data. Valid entries include office visits, hospital admissions, etc. These source codes can also be type-of service codes and activity type codes.
visit_source_concept_id | No |Provide When Available| Integer | A foreign key to a concept that refers to the code used in the source. | If a site is using HCPS or CPT for their visit source value, the standard concept id that maps to the particular vocabulary can be used here.  <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
preceding_visit_occurrence_id| No | NO | Integer | A foreign key to the VISIT_OCCURRENCE table record of the visit immediately preceding this visit.| Do not transmit
admitted_from_concept_id| No|Provide When Available|Integer|A foreign key to the predefined concept in the Place of Service Vocabulary reflecting the admitting source for a visit.| <p>Please use the following valid concept id set for Admitting source:</p><ul><li>Adult Foster Home=44814670</li><li>Assisted Living Facility=44814671</li><li>Ambulatory Visit=44814672</li><li>Emergency Department=8870=</li><li>Home Health=44814674</li><li>Home / Self Care=44814675</li><li>Hospice=8546</li><li>Other Acute Inpatient Hospital=38004279</li><li>Nursing Home (Includes ICF)=44814678</li><li>Rehabilitation Facility=44814679</li><li>Residential Facility=44814680</li><li>Skilled Nursing Facility=8863</li><li>No information=44814650</li><li>Unknown=44814653</li><li>Other=44814649</li></ul> This should be populated for inpatient encounters in the source but may vary for emergency department (ED) visits and outpatient encounters (AV,OA).|
discharge_to_concept_id|No|Provide When Available|Integer | A foreign key to the predefined concept in the Place of Service Vocabulary reflecting the discharge disposition (destination) for a visit.|<p>Please use the following valid concept id set for Discharge Destination:</p><ul><li>Adult Foster Home=38004205</li>	<li>Assisted Living Facility=38004301</li><li>Against Medical Advice=4021968</li><li>Absent without leave=44814693</li>	<li>Expired=4216643</li><li>Home Health=38004195</li><li>Home / Self Care=8536</li><li>Hospice=8546</li><li>Other Acute Inpatient Hospital=38004279</li><li>Nursing Home (Includes ICF)=8676</li><li>Rehabilitation Facility=8920</li>	<li>Residential Facility=44814701</li><li>Still In Hospital=8717</li><li>Skilled Nursing Facility=8863</li><li>No information=44814650</li><li>Unknown=44814653</li><li>Other=44814649</li></ul> This should be populated for inpatient encounters in the source but may vary for emergency department (ED) visits and outpatient encounters (AV,OA).
admitted_from_source_value|No|Provide When Available|Varchar | The source code for the admitting source as it appears in the source data. |This should be populated for inpatient encounters in the source but may vary for emergency department (ED) visits and outpatient encounters (AV,OA).
discharge_to_source_value|No|Provide When Available|Varchar | The source code for the discharge disposition as it appears in the source data. |This should be populated for inpatient encounters in the source but may vary for emergency department (ED) visits and outpatient encounters (AV,OA).

## 7 CONDITION_OCCURRENCE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
condition_occurrence_id | Yes |Yes| Integer | A unique identifier for each condition occurrence event. | This is not a value found in the EHR. Sites may choose to use a sequential value for this field
person_id | Yes |Yes| Integer | A foreign key identifier to the person who is experiencing the condition. The demographic details of that person are stored in the person table.
condition_concept_id | Yes |Yes| Integer | A foreign key that refers to a standard condition concept identifier in the Vocabulary. | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id ='SNOMED')</p> <p>select \* from concept where vocabulary_id ='SNOMED'  yields ~440,000 valid concept_ids.</p> If none are correct, use concept_id = 0
condition_start_date | Yes |Yes| Date| The date when the instance of the condition is recorded. | No date shifting.  
condition_end_date | No |Provide When Available| Date| The date when the instance of the condition is considered to have ended | <p>No date shifting.</p> If this information is not available, set to NULL. 
condition_start_datetime | Yes |Yes| Datetime | The date and time when the instance of the condition is recorded. | No date shifting.  Full date and time. **If there is no time associated with the date assert midnight for the start time**
condition_end_datetime | No |Provide When Available| Datetime | The date and time when the instance of the condition is considered to have ended | <p>No date shifting.</p> If this information is not available, set to NULL.  Full date and time.  **If there is no time associated with the date assert 11:59:59 pm for the end time**
condition_type_concept_id | Yes |Yes| Integer | A foreign key to the predefined concept identifier in the Vocabulary reflecting the source data from which the condition was recorded, the level of standardization, and the type of occurrence. For example, conditions may be defined as primary or secondary diagnoses, problem lists and person statuses. | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where concept_class_id ='Condition Type' and vocabulary_id='custom')</p> <p>select \* from concept where concept_class_id ='Condition Type' and vocabulary_id='custom' yields 21 valid concept_ids.</p> <p>If none are correct, use concept_id = 0</p>**For the primary diagnosis for the inpatient, outpatient or emergency setting (may be identified as Dx#1 in a source system), Please use concepts the following concepts:** <ul><li>Outpatient header - 1st position - Order Origin=2000000095</li><li>Outpatient header - 1st position - Billing Origin=2000000096</li><li>Outpatient header - 1st position - Claim Origin=2000000097</li><li>Inpatient header - primary - Order Origin=2000000092</li><li>Inpatient header - primary - Billing Origin =2000000093</li><li>Inpatient header - primary - Claim Origin= 2000000094</li><li>Emergency Header - 1st Position - Order Origin=2000001280</li> <li>Emergency Header - 1st Position - Claim Origin=2000001281</li> <li>Emergency Header - 1st Position - Billing Origin=2000001282</li></ul>**All other diagnosis that is not the primary (or Dx#1) in the inpatient, outpatient or emergency setting should correspond to the following concept ids:**<ul><li>Inpatient header - 2nd position - Order Origin=2000000098</li><li>Inpatient header - 2nd position - Billing Origin = 2000000099</li><li>Inpatient header - 2nd position - Claim Origin = 2000000100</li><li>Outpatient header - 2nd position - Order Origin=2000000101</li><li>Outpatient header - 2nd position - Billing Origin =2000000102</li><li>Outpatient header - 2nd position - Claim Origin =2000000103</li><li>Emergency Header - 2nd Position - Order Origin=2000001283</li> <li>Emergency Header - 2nd Position - Claim Origin=2000001284</li> <li>Emergency Header - 2nd Position - Billing Origin=2000001285</li></ul>**For diagnosis from the problem list, please use the following concept ids:**<ul><li>EHR problem list entry - Order Origin = 2000000089 </li><li>EHR problem list entry - Billing Origin =2000000090</li><li>EHR problem list entry - Claim Origin =2000000091</li></ul> 
stop_reason | No |Provide When Available| Varchar | The reason, if available, that the condition was no longer recorded, as indicated in the source data. | <p>Valid values include discharged, resolved, etc. Note that a stop_reason does not necessarily imply that the condition is no longer occurring, and therefore does not mandate that the end date be assigned.</p>
provider_id | No |Provide When Available| Integer | A foreign key to the provider in the provider table who was responsible for determining (diagnosing) the condition. | **This field was previously called associated_provider_id**<p>Any valid provider_id allowed (see definition of providers in PROVIDER table)</p> Make a best-guess and document method used. Or leave blank
visit_occurrence_id | No | Provide When Available|Integer | A foreign key to the visit in the visit table during which the condition was determined (diagnosed).
condition_source_value | Yes |Yes| Varchar | The source code for the condition as it appears in the source data. This code is mapped to a standard condition concept in the Vocabulary and the original code is, stored here for reference. | Condition source codes are typically ICD-9-CM or ICD-10-CM diagnosis codes from medical claims or discharge status/visit diagnosis codes from EHRs. Use source_to_concept maps to translation from source codes to OMOP concept_ids. **Please include the diagnosis name and source code when populating this field, by using the pipe delimiter "\|" when concatenating values.** Example: Diagnosis Name "\|" IMO Code "\|" Diagnosis Code
condition_source_concept_id | No |Provide When Available| Integer | A foreign key to a condition concept that refers to the code used in the source| As a standard convention this code must correspond to the ICD9/ICD10 concept mapping of the source value only. For example, if the condition is "Acute myeloid leukemia, without mention of having achieved remission" which has an icd9 code of 205.00 the condition source concept id is 44826430 which is the icd9 code concept that corresponds to the diagnosis 205.00. <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
condition_status_concept_id |No | Optional| Integer|A foreign key to the predefined concept in the standard vocabulary reflecting the condition status. | <p> We are only reporting final diagnosis, please use the following concept id:</p><ul> <li> Final Diagnosis=4230359 </li></ul>
condition_status_source_value| No| Optional | Varchar|  The source code for the condition status as it appears in the source data.
poa_concept_id|No|Optional|Integer| A foreign key to value in the source for that determines if the diagnosis is present on admission| <p> Please use the following: <ul><li>Yes=4188539 </li><li>No=4188540</li><li>No Information: concept_id = 44814650 </li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul> If none are correct, use concept_id = 0. </p>

## 8 PROCEDURE_OCCURRENCE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | PEDSnet Conventions
 --- | --- | --- | --- | ---| ---
procedure_occurrence_id | Yes |Yes| Integer | A system-generated unique identifier for each procedure occurrence | This is not a value found in the EHR. Sites may choose to use a sequential value for this field
person_id | Yes |Yes| Integer | A foreign key identifier to the person who is subjected to the procedure. The demographic details of that person are stored in the person table.
procedure_concept_id | Yes |Yes| Integer | A foreign key that refers to a standard procedure concept identifier in the Vocabulary. | <p>Valid Procedure Concepts belong to the "Procedure" domain. Procedure Concepts are based on a variety of vocabularies: SNOMED-CT (vocabulary_id ='SNOMED'), ICD-9-Procedures (vocabulary_id ='ICD9Proc'),ICD-10-Procedures (vocabulary_id ='ICD10PCS' **NOT YET AVAILABLE**), CPT-4 (vocabulary_id ='CPT4' ), and HCPCS (vocabulary_id ='HCPCS')</p> <p>Procedures are expected to be carried out within one day. If they stretch over a number of days, such as artificial respiration, usually only the initiation is reported as a procedure (CPT-4 "Intubation, endotracheal, emergency procedure").</p> Procedures could involve the administration of a drug, in which case the procedure is recorded in the procedure table and simultaneously the administered drug in the drug table.
modifier_concept_id | No |Provide When Available| Integer | A foreign key to a standard concept identifier for a modifier to the procedure (e.g. bilateral) |  <p>Valid Modifier Concepts belong to the "Modifier" concept class. select /* from concept where concept_class_id like '%Modifier%'. </p>
quantity | No |Provide When Available|Float |The quantity of procedures ordered or administered.
procedure_date | Yes | Yes|Date | The date on which the procedure was performed. 
procedure_datetime | Yes | Yes|Datetime | The date and time on which the procedure was performed. If there is no time associated with the date assert midnight. | **This field is a custom PEDSnet field**
procedure_type_concept_id | Yes |Yes| Integer | A foreign key to the predefined concept identifier in the Vocabulary reflecting the type of source data from which the procedure record is derived. (OMOP vocabulary_id = 'Procedure Type') | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id = 'Procedure Type')</p> <p>select \* from concept where vocabulary_id ='Procedure Type' yields 93 valid concept_ids.</p><p>For procedures coming from billing records please map to the following concepts:<ul><li>Primary Procedure: 44786630</li><li>Secondary Procedure: 44786631</li></ul><p>If you are unable to distinguish between primary and secondary procedures. Please map to the following: <ul><li>Secondary Procedure: 44786631</li></ul></p><p>For procedures coming from physician orders and all other types, please map to the following:<ul><li> EHR order list entry: 38000275 </li></ul></p>
provider_id | No | Provide When Available|Integer | A foreign key to the provider in the provider table who was responsible for carrying out the procedure. | <p>Any valid provider_id allowed (see definition of providers in PROVIDER table)</p> Document how selection was made.
visit_occurrence_id | No |Provide When Available| Integer | A foreign key to the visit in the visit table during which the procedure was carried out. | See VISIT.visit_occurrence_id (primary key)
procedure_source_value | Yes |Yes| Varchar | The source code for the procedure as it appears in the source data. This code is mapped to a standard procedure concept in the Vocabulary and the original code is stored here for reference. | Procedure_source_value codes are typically ICD-9, ICD-10 Proc, CPT-4, HCPCS, or OPCS-4 codes. All of these codes are acceptable source values.Please also include the procedure name. See Note 1.
procedure_source_concept_id | No |Provide When Available| Integer | A foreign key to a procedure concept that refers to the code used in the source.| For example, if the procedure is "Anesthesia for procedures on eye; lens surgery" in the source which has a concept code in the vocabulary that is 2100658. The procedure source concept id will be 2100658.  <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
modifier_source_value | No |Provide When Available| Varchar | The source code for the modifier as it appears in the source data.

## 9 OBSERVATION

Concept Name | Observation concept ID | Vocab ID | Value as concept ID | Concept description | Vocab ID| PCORNet Mapping
 --- | --- | --- | --- | --- | ---| ---
Discharge status | 44813951 | SNOMED | 4161979 | Discharged alive
Discharge status| 44813951 | SNOMED | 4216643 | Expired
Discharge status | 44813951 | SNOMED | 44814650 | No information | PCORNet
Discharge status | 44813951 | SNOMED | 44814653 | Unknown | PCORNet
Discharge status | 44813951 | SNOMED | 44814649 | Other | PCORNet
Tobacco |4005823| |4005823 |Tobacco User | | 01 = Current user
Tobacco |4005823| |45765920 |  Never used Tobacco| |02 = Never
Tobacco |4005823| |45765917|  Ex-tobacco user| |03 = Quit/Former Smoker
Tobacco |4005823| |4030580 | Non-smoker's second hand smoke syndrome| |04 = Passive or environmental exposure
Tobacco |4005823| |2000000040 ||| 06 = Not asked
Tobacco |4005823| |44814650 |No information | PCORNet | NI
Tobacco |4005823| |44814653| Unknown| PCORNet | OT
Tobacco |4005823| |44814649| Other| PCORNet| UN
Tobacco Type |4219336 |Multiple Response allowed |4298794 |Smoker | | 01 = Smoked tobacco only
Tobacco Type |4219336 |Multiple Response allowed |4224317 |Pipe smoking tobacco | | 01 = Smoked tobacco only
Tobacco Type |4219336 |Multiple Response allowed |4282779 |Cigarette smoking tobacco | | 01 = Smoked tobacco only
Tobacco Type |4219336 | Multiple Response allowed|4132133 |Cigar smoking tobacco | | 01 = Smoked tobacco only
Tobacco Type |4219336 |Multiple Response allowed |4218197 |Snuff tobacco | | 02 = Non-smoked tobacco only
Tobacco Type |4219336 | Multiple Response allowed|4219234 |Chewing tobacco | | 02 = Non-smoked tobacco only
Tobacco Type |4219336 | |45765920 |Never used tobacco | | 04 = None
Tobacco Type |4219336 | |45765917 |Ex tobacco user | | 04 = None
Tobacco Type |4219336 | |4030580 | Non-smoker's second hand smoke syndrome| |04 = Passive or environmental exposure/None
Tobacco Type |4219336 | | 44814650 |No information | PCORNet| NI
Tobacco Type |4219336 | |44814653| Unknown| PCORNet | OT
Tobacco Type |4219336 | |44814649| Other| PCORNet| UN
Smoking |4275495 | |42709996 |Smokes tobacco daily| | 01 = Current everyday smoker
Smoking |4275495 | |2000000039|Occasional tobacco smoker - SNOMED International Code|custom | 02 = current some day smoker
Smoking |4275495 | |4310250|Ex-smoker| | 03 = Former smoker
Smoking |4275495 | |4144272|Never smoked tobacco| | 04 = Never smoker
Smoking |4275495 | |4298794|Smoker| | 05 = Smoker, current status unknown
Smoking |4275495 | |4141786|Tobacco smoking consumption(status) unknown| | 06 = Unknown if ever smoked
Smoking |4275495 |**USE AS DEFAULT FOR CATEGORY** |4044778|Chain smoker | | 07 = Heavy tobacco smoker
Smoking |4275495 | |4209006|Heavy smoker (over 20 per day)| | 07 = Heavy tobacco smoker
Smoking |4275495 |**USE ONLY IF QUANTITY OF CIGARETTES IS KNOWN** |4209585|Moderate smoker (20 or less per day)| | 08 = Light tobacco smoker
Smoking |4275495 | | 44814650 |No information | PCORNet| NI
Smoking |4275495 | |44814653| Unknown| PCORNet | OT
Smoking |4275495 | |44814649| Other| PCORNet| UN
Delivery Mode|40760190| SNOMED|4192676 |Born by cesarean section|SNOMED|
Delivery Mode|40760190| SNOMED|4212794|Born by elective cesarean section|SNOMED|
Delivery Mode|40760190| SNOMED|4250010|Born by emergency cesarean section|SNOMED|
Delivery Mode|40760190| SNOMED|4216797|Born by normal vaginal delivery|SNOMED|
Delivery Mode|40760190| SNOMED|4217586|Born by forceps delivery|SNOMED|
Delivery Mode|40760190| SNOMED|4236293|Born by ventouse delivery|SNOMED|
Delivery Mode|40760190| SNOMED|4250009|Born by breech delivery|SNOMED|


## 10 OBSERVATION PERIOD

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
Observation_period_id | Yes |Yes| Integer | A system-generate unique identifier for each observation period | This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
person_id | Yes |Yes| Integer | A foreign key identifier to the person who is experiencing the condition. The demographic details of that person are stored in the person table.
Observation_period_start_date | Yes |Yes| Date | The start date of the observation period for which data are available from the data source | <p>Use the earliest clinical fact date available for this patient.</p> No date shifting.  
Observation_period_end_date | Yes | Yes|Date | The end date of the observation period for which data are available from the source. | <p>Use the latest clinical fact date available for this patient. If there exists one or more records in the DEATH table for this patient, use the latest date recorded in that table.</p> 
Observation_period_start_time | Yes | Yes|Datetime | The start date of the observation period for which data are available from the data source | <p>Use the earliest clinical fact time available for this patient.</p> No date shifting.  Full date and time. **If there is no time associated with the date assert midnight for the start time**
Observation_period_end_time | Yes |Yes| Datetime | The end date of the observation period for which data are available from the source. | <p>Use the latest clinical fact time available for this patient. If there exists one or more records in the DEATH table for this patient, use the latest date recorded in that table.</p> For patients who are still in the hospital or ED or other facility at the time of data extraction, leave this field NULL.  Full date and time.  **If there is no time associated with the date assert 11:59:59 pm for the end time**
period_type_concept_id|Yes|Yes|Integer|A unique identifier for each observation period.

## 11 DRUG EXPOSURE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
 drug_exposure_id | Yes |Yes| Integer | A system-generated unique identifier for each drug exposure | This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
person_id | Yes |Yes|Integer | A foreign key identifier to the person who is experiencing the condition. The demographic details of that person are stored in the person table.
drug_concept_id| Yes |Yes| Integer | A foreign key that refers to a standard drug concept identifier in the Vocabulary. | Valid drug concept IDs are mapped to RxNorm using the source to concept map table to transform source codes (GPI, NDC etc to the RxNorm target).
drug_exposure_start_date| Yes |Yes| Date |The start date of the utilization of the drug. The start date of the prescription, the date the prescription was filled, the date a drug was dispensed or the date on which a drug administration procedure was recorded are acceptable. | If the start date of the drug is null in the source system, use the ordering date as the start date. No date shifting. 
drug_exposure_end_date| No |Provide When Available|Date | The end date of the utilization of the drug | No date shifting.
drug_exposure_order_date| No | Provider When available| Date | The order date of the drug | No date shifting.
drug_exposure_start_datetime| Yes |Yes| Datetime |The start date and time of the utilization of the drug. The start date of the prescription, the date the prescription was filled, the date a drug was dispensed or the date on which a drug administration procedure was recorded are acceptable. | No date shifting. Full date and time. **If there is no time associated with the date assert midnight for the start time**|
drug_exposure_end_datetime| No |Provide When Available|Datetime | The end date and time of the utilization of the drug | No date shifting. Full date and time.  **If there is no time associated with the date assert 11:59:59 pm for the end time**|
drug_exposure_order_datetime| No | Provider When available| Datetime | The order date and time of the drug |If the start datetime of the drug is null in the source system, use the ordering datetime as the start datetime. No date shifting.Full date and time. **If there is no time associated with the date assert midnight for the start time**
drug_type_concept_id| Yes | Yes|Integer | A foreign key to a standard concept identifier of the type of drug exposure in the Vocabulary as represented in the source data | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where concept_class_id ='Drug Type')</p> <p>select \* from concept where domain_id ='Drug Type' yields 13 valid concept_ids.</p> <p>If none are correct, use concept_id = 0.</p> For the drug types listed above, use the following concept_ids: <ul><li>Prescription dispensed in pharmacy (dispensed meds pharma information): concept_id = 38000175</li> <li>Inpatient Medication Order: 581373</li><li>Inpatient administration (MAR entries): concept_id = 38000180</li> <li>Prescription written: concept_id = 38000177</li></ul>
stop_reason| No | Provide When Available|Varchar | The reason, if available, where the medication was stopped, as indicated in the source data. | <p>Valid values include therapy completed, changed, removed, side effects, etc. Note that a stop_reason does not necessarily imply that the medication is no longer being used at all, and therefore does not mandate that the end date be assigned.</p>
refills| No | Provide When Available|Integer | The number of refills after the initial prescription| Extract numbers as much as possible , full value should be a part of the xml sig field.|
quantity| No |Provide When Available| Integer | The quantity of the drugs as recorded in the original prescription or dispensing record| Extract numbers as much as possible , full value should be a part of the xml sig field.|
days_supply| No |Provide When Available| Integer | The number of days of supply the medication as recorded in the original prescription or dispensing record||
sig| No | Provide When Available|CLOB (XML Structure) | The directions on the drug prescription as recorded in the original prescription (and printed on the container) or the dispensing record| |
route_concept_id| No |Provide When Available| Integer | A foreign key that refers to a standard administration route concept identifier in the Vocabulary. | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where domain_id='Route')</p> <p>select * from concept where domain_id='Route' and invalid_reason is null yields 70 valid concept_ids.</p> <p><ul><li>Within the set of 70 valid concept ids, duplicates may exist. If this is the case, use the standard concept (standard_concept='S') first for mapping and then the non-standard concept for all other cases</li></ul> If none are correct, use concept_id = 0. </p>
effective_drug_dose| No |Provide When Available| Float | Numerical value of drug dose for this drug_exposure record| |
eff_drug_dose_source_value| No |Provide When Available| Varchar | The drug dose for this drug_exposure record as it appears in the source| |
dose_unit_concept_id| No |Provide When Available| Integer | A foreign key to a predefined concept in the Standard Vocabularies reflecting the unit the effective drug_dose value is expressed|<p> Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id = UCUM)</p> select * from concept where vocabulary_id = 'UCUM' yields 971 valid concept_ids.|
lot_number| No |Site preference| Varchar | An identifier to determine where the product originated||
provider_id| No |Provide When Available|  Integer | A foreign key to the provider in the provider table who initiated (prescribed) the drug exposure |<p>Any valid provider_id allowed (see definition of providers in PROVIDER table)</p> Document how selection was made.
visit_occurrence_id| No |Provide When Available|  Integer | A foreign key to the visit in the visit table during which the drug exposure initiated. | See VISIT.visit_occurrence_id (primary key)
drug_source_value| No|Provide When Available|  Varchar | The source drug value as it appears in the source data. The source is mapped to a standard RxNorm concept and the original code is stored here for reference.| Please be sure to include your source code and the drug name in this field. This will be useful in the event that there is no RxNorm mapping for your local medication code. Please use the pipe delimiter "\|" when concatenating values. 
drug_source_concept_id| No |Provide When Available|  Integer | A foreign key to a drug concept that refers to the code used in the source | In this case, if you are transforming drugs from GPI or NDC to RXNorm. The concept id that corresponds to the GPI or NDC value for the drug belongs here. <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p>
route_source_value| No|Provide When Available|  Varchar |The information about the route of administration as detailed in the source ||
dose_unit_source_value| No|Provide When Available|  Varchar | The information about the dose unit as detailed in the source ||
frequency| No | Optional | Varchar | The frequency information as available from the source ||
dispense_as_written_concept_id|No|Optional|Integer| A foreign key to value in the source for that determines if the medication is to be dispensed as written| <p> Please use the following: <ul><li>Yes=4188539 </li><li>No=4188540</li><li>No Information: concept_id = 44814650 vocabulary_id='PCORNet')</li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul> If none are correct, use concept_id = 0. </p>

## 12 MEASUREMENT

Domain id | Measurement concept ID | Vocab ID | Value as concept ID | Concept description | Vocab ID
 --- | --- | --- | --- | --- | ---
Vital | 3013762 | | | Weight
Vital | 3023540 | | | Height
Vital | 21490852| | | Invasive Mean arterial pressure (MAP)
Vital | 21492241| | | Non-Invasive Mean arterial pressure (MAP)
Vital | 3027018 | | | Heart Rate
Vital | 40762499 | | | Oxygen Saturation (SpO2)
Vital | 3024171  | | | Respiration Rate
Vital | 3038553 | | | BMI kg/m<sup>2</sup>
Vital | 3034703 | | | Diastolic Blood Pressure - Sitting
Vital | 3019962 | | | Diastolic Blood Pressure - Standing
Vital | 3013940 | | | Diastolic Blood Pressure - Supine
Vital | 3012888 | | | Diastolic BP Unknown/Other
Vital | 3018586 | | | Systolic Blood Pressure - Sitting
Vital | 3035856 | | | Systolic Blood Pressure - Standing
Vital | 3009395 | | | Systolic Blood Pressure - Supine
Vital | 3004249 | | | Systolic BP Unknown/Other
Vital | 2000000041 | | | Weight for age z score NHANES
Vital | 2000000042 | | | Height for age z score NHANES
Vital | 2000000043 | | | BMI for age z score NHANES
Vital | 2000000044 | | | Weight for age z score WHO
Vital | 2000000045 | | | Height for age z score WHO
Vital | 2000000046 | | | Systolic BP for age/height Z score NCBPEP
Vital | 2000000047 | | | Diastolic BP for age/height Z score NCBPEP
Vital | 3020891 || | Temperature 
Vital | 3001537|| | Head Circumference
Lab | 3020158|| |  FVC
Lab | 3037879|| |  FVC pre (if recorded differently)
Lab | 3001668|| |  FVC post
Lab | 3024653|| | FEV 1
Lab | 3005025|| |  FEV 1 pre (if recorded differently)
Lab | 3023550|| |  FEV 1 post
Lab | 42868460|| |  FEF 25-75
Lab | 42868461|| |  FEF 25-75 pre (if recorded differently)
Lab | 42868462|| |  FEF 25-75 post
Lab | 3023329|| |  Peak Flow (PF)
Lab | 2000000064|| |  Peak Flow post
Vital | 3013762 | | | BIRTH Weight
Vital | 3023540 | | | BIRTH Height
Vital | 3001537|| | BIRTH Head Circumference
Measurement Type | 44818704 | Measurement Type | | Patient reported
Measurement Type | 2000000032| Measurement Type | | Vital sign from device direct feed
Measurement Type | 2000000033| Measurement Type | | Vital sign from healthcare delivery setting
Measurement Type | 44818702| Measurement Type | | Clinical and Laboratory Results


## 13 FACT RELATIONSHIP

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
Domain_concept_id_1|Yes|Yes| Integer |	The concept representing the domain of fact one, from which the corresponding table can be inferred.| Predefined value set: <ul><li>Visit domain (ED->Inpatient linking) = 8</li><li>Measurement domain (blood pressure linking) = 21</li><li>Observation domain (tobacco linking) = 27</li><li> Drug Domain (Inpatient Medication Orders) = 13</li></ul>
Fact_id_1|	Yes |Yes| Integer |The unique identifier in the table corresponding to the domain of fact one.| 
Domain_concept_id_2|Yes |Yes| Integer |	The concept representing the domain of fact two, from which the corresponding table can be inferred.| Predefined value set: <ul><li>Visit domain (ED->Inpatient linking) = 8</li><li>Measurement domain (blood pressure linking) = 21</li><li>Observation domain (tobacco linking) = 27</li><li> Drug Domain (Inpatient Medication Orders) = 13</li></ul>
Fact_id_2 |	Yes |Yes| Integer |	The unique identifier in the table corresponding to the domain of fact two.
Relationship_concept_id	|Yes |Yes| Integer |A foreign key to a standard concept identifier of relationship in the Standardized Vocabularies.| Predefined value set: <ul><li>Occurs before (ED Visit) = 44818881</li><li>Occurs after (Inpatient Visit) = 44818783</li><li>Associated with finding (blood pressures) = 44818792</li><li>Occurrence of (Inpatient Medication Orders=44818848 </li><li>Subsumes (Inpatient Medication Orders=44818723 </li><li>No matching concept (tobacco) = 0</li></ul>

## 14 VISIT_PAYER

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
visit_payer_id | Yes |Yes|  Integer |A system-generated unique identifier for each visit payer relationship. | This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
visit_occurrence_id | Yes |Yes| Integer | A foreign key to the visit in the visit table where the payer was billed for the visit.
plan_name | Yes |Yes|  Varchar| The untransformed payer/plan name from the source data
plan_type | No |Provide When Available|  Varchar |  A standardized interpretation of the plan structure | Please only map your plan type to the following categories: <ul> <li>HMO</li> <li>PPO</li> <li>POS</li> <li>Fee for service</li><li> Other/Unknown </li></ul> If the categories are unclear, please work with your billing department or local experts to determine how to map plans to these values.
plan_class | Yes |Yes|  Varchar | A list of the "payment sources" most often used in demographic analyses| Please map your plan type to the following categories: <ul> <li>Private/Commercial</li> <li>Medicaid/sCHIP</li> <li>Medicare</li> <li>Other public</li> <li>Self-pay</li> <li>Other/Unknown</li></ul> Please work with your billing department or local experts to determine how to map plans to these values.
visit_payer_type_concept_id| No| Optional| Integer| A foreign key to a concept that refers to the status of the payer in the source.| This is the concept id that maps to the source value in the standard vocabulary. <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p> Use the following concept_ids: <ul><li>Payer is primary" concept_id = 31968</li> <li>Payer is secondary: concept_id = 31969</li></ul> <p>If you are unable to distinguish between primary and secondary payers. Please map to the following: <ul><li>Payer is secondary: concept_id = 31969</li></ul></p>

## 15 MEASUREMENT_ORGANISM

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
meas_organism_id | Yes |Yes|  Integer |A system-generated unique identifier for each organism culture relationship. | This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
measurement_id | Yes |Yes| Integer | A foreign key to the lab result in the measurement table where the organism was observed.
person_id|	Yes |Yes|	Integer|	A foreign key identifier to the person who the measurement is being documented for. The demographic details of that person are stored in the person table.|	
visit_occurrence_id| No |Provide When Available| Integer | A foreign key to the visit where the culture lab was ordered|
organism_concept_id| Yes |Yes|  Integer| A foreign key to a standard concept identifier for the organism in the Vocabulary.| <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id = SNOMED and concept_class_id= Organism and standard_concept=S)</p> <p>select \* from concept where vocabulary_id ='SNOMED' and concept_class_id='Organism' and standard_concept='S' yields 33039 valid concept_ids.</p>
organism_source_value | Yes |Yes|  Varchar | The organism value as it appears in the source.
positivity_datetime| No| Optional | Datetime| The estimated date and time of initial growth as reported in the source.|

## 16 ADT_OCCURRENCE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
 adt_occurrence_id| 	Yes	| Yes| 	Integer| 	A unique identifier for each ADT event.| This is not a value found in the EHR. Sites may choose to use a sequential value for this field
person_id| 	Yes| 	Yes	| Integer| 	A  foreign key identifier to the person for whom the visit is recorded.	
visit_occurrence_id| 	Yes| 	Yes| 	Integer	| A foreign key identifier to the visit containing this event.	
adt_date| 	Yes| 	Yes| 	Date	| The date of the adt event
adt_datetime	| Yes| 	Yes	| Datetime	| The datetime of the adt event|	No date shifting. Full date and time. If there is no time associated with the date assert midnight for the start time.
care_site_id| 	No| 	Provide when available| 	Integer| 	A foreign key to the care site in which this adt event occurred.	
service_concept_id| 	Yes	| Yes| 	Integer	| A foreign key that refers to a adt event service concept identifier in the vocabulary.  This concept describes the type of service associated with this adt event.	|<p>select \* from concept where vocabulary_id ='custom' and concept_class_id='Service Type' and standard_concept='S' yields 14 valid concept_ids.</p> **Only the NICU,CICU and PICU services are included.**<p>The value set available includes:<ul> <li>	CICU (cardiac care) = 2000000079</li><li> NICU (neonatal care) = 2000000080</li><li> PICU (all other ICU) = 2000000078 </li><li>	Critical care = 2000000067</li><li>	Intermediate care = 2000000068 </li><li>	Acute care = 2000000069 </li><li>	Observation care = 2000000070 </li><li>	Surgical site (includes OR, ASC) = 2000000071  </li><li>	Procedural service = 2000000072  </li><li> Behavioral health =  2000000073  </li><li> Rehabilitative service (includes PT, OT, ST) = 2000000074 </li><li>	Specialty service = 2000000075</li><li> Radiology = 2000000076 </li><li> Hospital Outpatient = 2000000077 </li>li>Unknown: concept_id = 44814653 </li> <li>Other: concept_id =  44814649 </li> <li>No information: concept_id =  44814650</li></ul></p>
adt_type_concept_id|No| Provide when available| Integer| A foreign key that refers to an adt event type concept identifier in the vocabulary. This concept describes the type of the adt event. | <p>select \* from concept where vocabulary_id ='custom' and concept_class_id='ADT Event Type' yields 5 valid concept_ids.</p> <p> The value set includes: <ul><li>Admission = 2000000083 </li><li>Discharge = 2000000084 </li>Transfer in = 2000000085 <li>Transfer out = 2000000086</li><li>Census = 2000000087</li></ul></p>
prior_adt_occurrence_id| No | Provide when available | Integer| Foreign key into the adt_occurrence table pointing to the ADT record immediately preceding this record in the event stream for the visit.  Must be populated for all but the first ADT even within a visit.
next_adt_occurrence_id | No | Provide when available | Integer| Foreign key into the adt_occurrence table pointing to the ADT record immediately following this record in the event stream for the visit.  Must be populated for all but the last ADT even within a visit.
service_source_value| 	No| 	Provide when available	| Varchar	| The source data used to derive the service type for this event.  It will typically be a department code from the ADT event.| 	
adt_type_source_value| No | Provide when available| Varchar| The source data used to identify the adt event type |

## 17 Immunization

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
immunization_id | Yes |Yes|  Integer | A system-generated unique identifier for each immunization record | This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
person_id | Yes |Yes|  Integer | A foreign key identifier to the person who the immunization record is being documented for. The demographic details of that person are stored in the person table.
immunization_concept_id | Yes |Yes|  Integer | A foreign key to the standard immunization concept identifier in the Vocabulary. |  <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id='CVX')</p> <p>select \* from concept where vocabulary_id='CVX' and invalid_reason is null yields 188 valid concept_ids.</p> <p>If none are correct, use concept_id = 0.</p> 
immunization_source_concept_id| No |Provide When Available|  Integer | A foreign key to an immunization concept that refers to the code used in the source |   <p>**If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0**</p> 
immunization_date|  Yes |Yes|  Date | The date of the immunization.|This should be the date the immunization was administered. No date shifting.
immunization_datetime|  Yes |Yes|  Datetime | The time of the immunization. |This should be the date the immunization was administered. No date shifting. Full date and time. If there is no time associated with the date assert midnight.
immunization_source_value| Yes |Yes|  Varchar |  The immunization name as it appears in the source data. This code is mapped to a standard concept in the Standardized Vocabularies and the original code is, stored here for reference.| This is the name of the value as it appears in the source system. Please use the pipe delimiter "\|" when concatenating values. 
provider_id | No | Provide When Available| Integer | A foreign key to the provider in the provider table who was responsible for the immunization.
imm_route_concept_id| No |Provide When Available| Integer | A foreign key that refers to a standard immunization administration route concept identifier in the Vocabulary. | <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where domain_id='Route')</p> <p>select * from concept where domain_id='Route' and invalid_reason is null yields 70 valid concept_ids.</p> <p><ul><li>Within the set of 70 valid concept ids, duplicates may exist. If this is the case, use the standard concept (standard_concept='S') first for mapping and then the non-standard concept for all other cases</li></ul> If none are correct, use concept_id = 0. </p>
immunization_dose| No |Provide When Available| Float | Numerical value of immunization dose for this immunization record| |
imm_dose_unit_concept_id| No |Provide When Available| Integer | A foreign key to a predefined concept in the Standard Vocabularies reflecting the unit the immunization_dose value is expressed| <p> Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id = UCUM)</p> select * from concept where vocabulary_id = 'UCUM' yields 971 valid concept_ids.|
imm_dose_unit_source_value| No|Provide When Available|  Varchar | The information about the immunization dose unit as detailed in the source ||
imm_route_source_value| No|Provide When Available|  Varchar |The information about the route of immunization as detailed in the source ||
visit_occurrence_id|No|Optional| Integer | A foreign key that refers to the visit associated with the immunization record.
procedure_occurrence_id|No|Optional| Integer | A foreign key that refers to the procedure associated with the immunization record.
imm_recorded_date|No| Provide when available| Date| The date the immunization was recorded.| This date is applicable for immunizations that have been reported by the patient and not administered at the visit. No date shifting.
imm_recorded_datetime|No| Provide when available|Datetime |The time the immunization was recorded.| This date and time is applicable for immunizations that have been reported by the patient and not administered at the visit. No date shifting.
imm_manufacturer|No| Provide when available| Varchar| The information about the immunization manufacturer||
imm_lot_num|No| Provide when available|Varchar|The information about the immunization lot number||
imm_exp_date|No| Provide when available|Date| The date of the immunization expiration.| No date shifting.|
imm_exp_datetime|No| Provide when available|Datetime| The date and time of the immunization expiration.| No date shifting.|
immunization_type_concept_id| 	Yes	| Yes| Integer| A foreign key that refers to source of immunization record.| <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where vocabulary_id ='custom' and concept_class_id='Immunization Type') <p> The value set includes: <ul><li>Internal administration(OD) = 2000001288 </li><li>External feed (EF) =2000001289  </li>Immunization Information Systems (IS) = 2000001290  <li>Patient Reported (PR) = 2000001291 </li><li>No Information: concept_id = 44814650 </li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul></p>
imm_body_site_concept_id| 	Yes	| Yes| Integer| A foreign key that refers to the body site where the immunization was administered in the vocabulary.| <p>Please include valid concept ids (consistent with OMOP CDMv5). Predefined value set (valid concept_ids found in CONCEPT table where domain_id ='Spec Anatomic Site')</p><p> select * from concept where domain_id ='Spec Anatomic Site' yields 38257 valid concept_ids. Flavors of null are also applicable: <ul><li>No Information: concept_id = 44814650 </li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul></p><p> If none are correct, use concept_id = 0</p>
imm_body_site_source_value|No| Provide when available| Varchar| The body site where the immunization was administered in the source system. |

## 18 DEVICE_EXPOSURE

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
device_exposure_id|	Yes|	Yes|	Integer|	A system-generated unique identifier for each Device Exposure.|This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
person_id|	Yes|	Yes|	Integer|	A foreign key identifier to the Person who is subjected to the Device. The demographic details of that Person are stored in the PERSON table.|
device_concept_id|	Yes|	Yes|	Integer|	A foreign key that refers to a Standard Concept identifier in the Standardized Vocabularies belonging to the 'Device' domain.| **Please use concept_id = 0.**
device_exposure_start_date|	Yes	|	Yes|	Date|	The date the Device or supply was applied or used.|No date shifting. Full date.
device_exposure_start_datetime|	Yes| Yes	| Datetime |	The date and time the Device or supply was applied or used.|No date shifting. Full date and time. If there is no time associated with the date assert midnight for the start time
device_exposure_end_date|	No|	No|	Date	|The date use of the Device or supply was ceased.|No date shifting. Full date.
device_exposure_end_datetime|	No|	No |	Datetime|	The date and time use of the Device or supply was ceased.| No date shifting. Full date.If there is no time associated with the date assert 11:59:59 pm for the end time
device_type_concept_id|	Yes| Yes|	Integer|	A foreign key to the predefined Concept identifier in the Standardized Vocabularies reflecting the type of Device Exposure recorded.|<p>select * from concept where concept_class_id='Device Type' yields 4 valid concept ids.</p><p> All of our observations are coming from electronic health records so set this field to concept_id = 44818707 (observation recorded from EHR Detail). </p>
unique_device_id|	No	|	Provide when available|	Varchar|	A UDI or equivalent identifying the instance of the Device used in the Person.|
quantity|	No|	No| Integer |	The number of individual Devices used in the exposure.|
provider_id	|No |Provide when Available|	Integer |	A foreign key to the provider in the PROVIDER table who initiated or administered the Device.|
visit_occurrence_id	|No	|Provide when available| Integer |	A foreign key to the visit in the VISIT_OCCURRENCE table during which the Device was used.|
device_source_value	|No| Yes|	Varchar |	The source code for the Device as it appears in the source data. This code is mapped to a Standard Device Concept in the Standardized Vocabularies and the original code is stored here for reference. | Please include the device name and model number when populating this field, by using the pipe delimiter "\|" when concatenating values. **Example: Device Name "\|" Model Number**
device_source_concept_id	|Yes| Yes|	Integer |	A foreign key to a Device Concept that refers to the code used in the source.| If there is not a mapping for the source code in the standard vocabulary, use concept_id = 0


## 19 LOCATION_HISTORY

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
location_history_id | Yes |Yes| Integer | A system-generated unique identifier for each Location History.|This is not a value found in the EHR. Sites may choose to use a sequential value for this field.
location_id | Yes |Yes| Integer | A foreign key to the location table.|
relationship_type_concept_id | Yes |Yes | Varchar | The type of relationship between location and entity.| At this time OMOP/OHDSI has not released a valid value set for this field.  **Use concept_id = 0.**
domain_id | Yes |Yes| Varchar | The domain of the entity that is related to the location. Either PERSON, PROVIDER, or CARE_SITE.| Only patient address histories are present in this table. Due to this **use domain_id = 'Person'** for all records.
entity_id | Yes |Yes| Integer | The unique identifier for the entity. References either person_id, provider_id, or care_site_id, depending on domain_id.|Only patient address histories are present in this table. Due to this, please populate this field with the corresponding person_id.
location_preferred_concept_id| Yes | Yes| Integer | A foreign key that indicates if the location is the preferred location.| <p> Please use the following: <ul><li>Yes=4188539 </li><li>No=4188540</li><li>No Information: concept_id = 44814650 </li> <li>Unknown: concept_id = 44814653</li> <li>Other: concept_id = 44814649</li></ul> If none are correct, use concept_id = 0. </p>
start_date | Yes| Yes| Date | The date the relationship started.| No date shifting.
start_datetime | Yes| Yes| Datetime | The date the relationship started.| No date shifting.
end_date | No |No| Date | The date the relationship ended.| This field should be NULL for the current location of the entity. No date shifting.
end_datetime | No |No| Datetime | The date the relationship ended.|This field should be NULL for the current location of the entity. No date shifting. 

## 20 HASH_TOKEN

Field |NOT Null Constraint |Network Requirement |Data Type | Description | Conventions
 --- | --- | --- | --- | ---| ---
person_id| Yes |Yes| Integer | A foreign key identifier to the Person who is subjected to the Device. The demographic details of that Person are stored in the PERSON table.
token_01|No|Provide When Available|Varchar|Encrypted keyed hash generated from PII using token strategy 01 in Datavant DeID.
token_02|No|Provide When Available|Varchar|Encrypted keyed hash generated from PII using token strategy 02 in Datavant DeID
token_03|No|Provide When Available|Varchar|Encrypted keyed hash generated from PII using token strategy 03 in Datavant DeID
token_04|No|Provide When Available|Varchar|Encrypted keyed hash generated from PII using token strategy 04 in Datavant DeID
token_05|No|Provide When Available|Varchar| Encrypted keyed hash generated from PII using token strategy 05 in Datavant DeID
token_16|No|Provide When Available|Varchar|Encrypted keyed hash generated from PII using token strategy 16 in Datavant DeID

* * *

## ***APPENDIX***

**Source Data Model is supported by OMOP-supported Vocabulary, which contains all of the additional concept_id codes needed**

### A1. ABMS Specialty Category to OMOP V5 Specialty Mapping
http://www.abms.org/member-boards/specialty-subspecialty-certificates/

ABMS Specialty Category | OMOP Supported Concept for Provider ID | OMOP Concept_name | Concept_class_id | Vocabulary id| Domain_id
--- | --- | --- | --- | --- | ---
Addiction Psychiatry |38004498 | Addiction Medicine | Physician Specialty| Medicare Specialty| Provider   
Adolescent Medicine |45756747 |Adolescent Medicine|Physician Specialty | ABMS | Provider
Adult Congenital Heart Disease |45756748  |Adult Congenital Heart Disease|Physician Specialty |  ABMS | Provider
Advanced Heart Failure and Transplant Cardiology |45756749 |Advanced Heart Failure and Transplant Cardiology|  Physician Specialty | ABMS | Provider
Aerospace Medicine |45756750 |Aerospace Medicine| Physician Specialty | ABMS | Provider
Allergy and Immunology | 38004448 | Allergy/Immunology                | Physician Specialty | Medicare Specialty | Provider    
Anesthesiology |38004450 | Anesthesiology   | Physician Specialty | Medicare Specialty | Provider    
Anesthesiology Critical Care Medicine |45756751 |Anesthesiology Critical Care Medicine|Physician Specialty | Medicare Specialty| Provider
Blood Banking/Transfusion Medicine |45756752|Blood Banking/Transfusion Medicine|Physician Specialty |  ABMS| Provider
Brain Injury Medicine |45756753 |Brain Injury Medicine|Physician Specialty |  ABMS| Provider
Cardiology |38004451 | Cardiology                       | Physician Specialty | Medicare Specialty | Provider 
Cardiovascular Disease |45756754 |Cardiovascular Disease|Physician Specialty |  ABMS| Provider
Child Abuse Pediatrics |45756755 |Child Abuse Pediatrics |Physician Specialty |  ABMS| Provider
Child and Adolescent Psychiatry |45756756|Child and Adolescent Psychiatry|Physician Specialty |  ABMS| Provider
Clinical Biochemical Genetics |45756757|Clinical Biochemical Genetics|Physician Specialty |  ABMS| Provider
Clinical Cardiac Electrophysiology |45756758|Clinical Cardiac Electrophysiology |Physician Specialty |  ABMS| Provider
Clinical Cytogenetics |45756759 |Clinical Cytogenetics|Physician Specialty |  ABMS| Provider
Clinical Genetics (MD) |45756760|Clinical Genetics (MD)|Physician Specialty |  ABMS| Provider
Clinical Informatics |45756761|Clinical Informatics|Physician Specialty |  ABMS| Provider
Clinical Molecular Genetics |45756762|Clinical Molecular Genetics|Physician Specialty |  ABMS| Provider
Clinical Neurophysiology |45756763|Clinical Neurophysiology |Physician Specialty |  ABMS| Provider
Colon and Rectal Surgery | 38004471 | Colorectal Surgery              | Physician Specialty | Medicare Specialty | Provider   
Complex General Surgical Oncology |45756764|Complex General Surgical Oncology| Physician Specialty | ABMS| Provider
Congenital Cardiac Surgery |45756765|Congenital Cardiac Surgery |Physician Specialty |  ABMS| Provider
Critical Care Medicine | 38004500 | Critical care (intensivist)      | Physician Specialty | Medicare Specialty | Provider    
Cytopathology |45756766|Cytopathology |Physician Specialty |  ABMS| Provider
Dermatology  |38004452 | Dermatology                        | Physician Specialty | Medicare Specialty | Provider    
Dermatopathology |45756767|Dermatopathology|Physician Specialty |  ABMS| Provider
Developmental-Behavioral Pediatrics |45756768|Developmental-Behavioral Pediatrics|Physician Specialty |  ABMS| Provider
Diagnostic Radiology |45756769|Diagnostic Radiology|Physician Specialty |  ABMS| Provider
Emergency Medical Services |45756770|Emergency Medical Services|Physician Specialty |  ABMS| Provider
Emergency Medicine | 38004510 | Emergency Medicine         | Physician Specialty | Medicare Specialty | Provider   
Endocrinology, Diabetes and Metabolism |45756771  |Endocrinology, Diabetes and Metabolism|Physician Specialty |  ABMS| Provider
Epilepsy |45756772   |Epilepsy|Physician Specialty |  ABMS| Provider
General Family Medicine | 38004453 | Family Practice                           | Physician Specialty | Medicare Specialty | Provider   
Female Pelvic Medicine and Reconstructive Surgery|45756773 |Female Pelvic Medicine and Reconstructive Surgery| Physician Specialty | ABMS| Provider
Forensic Psychiatry |45756775|Forensic Psychiatry|Physician Specialty |  ABMS| Provider
Gastroenterology |38004455 | Gastroenterology                    | Physician Specialty | Medicare Specialty | Provider  
General Pediatrics (Primary Care)* |2000000063 |General Pediatrics        | Specialty | custom | Provider   
Geriatric Medicine | 38004478 | Geriatric Medicine                   | Physician Specialty | Medicare Specialty | Provider   
Geriatric Psychiatry |45756776|Geriatric Psychiatry|Physician Specialty |  ABMS| Provider
Gynecologic Oncology |38004513 | Gynecology/Oncology              | Physician Specialty | Medicare Specialty | Provider    
Hematology | 38004501 | Hematology                           | Physician Specialty | Medicare Specialty | Provider   
Hospice and Pallative Medicine |45756777 |Hospice and Pallative Medicine|Physician Specialty |  ABMS| Provider
Infectious Disease | 38004484 | Infectious Disease                | Physician Specialty | Medicare Specialty | Provider   
General Internal Medicine | 38004456 | Internal Medicine| Physician Specialty | Medicare Specialty | Provider   
Internal Medicine - Critical Care Medicine |45756778|Internal Medicine - Critical Care Medicine|Physician Specialty |  ABMS| Provider
Interventional Cardiology |45756779 |Interventional Cardiology|Physician Specialty |  ABMS| Provider
Interventional Radiology and Diagnostic Radiology |38004511 | Interventional Radiology | Physician Specialty | Medicare Specialty | Provider   
Maternal and Fetal Medicine |45756780|Maternal and Fetal Medicine |Physician Specialty |  ABMS| Provider
Medical Biochemical Genetics |45756781|Medical Biochemical Genetics|Physician Specialty |  ABMS| Provider
Medical Genetics and Genomics |45756782 |Medical Genetics and Genomics|Physician Specialty |  ABMS| Provider
Medical Oncology |38004507 | Medical Oncology | Physician Specialty | Medicare Specialty | Provider   
Medical Physics |45756783|Medical Physics|Physician Specialty |  ABMS| Provider
Medical Toxicology|45756784|Medical Toxicology|Physician Specialty |  ABMS| Provider
Molecular Genetic Pathology |45756785|Molecular Genetic Pathology|Physician Specialty |  ABMS| Provider
Neonatal-Perinatal Medicine |45756786|Neonatal-Perinatal Medicine|Physician Specialty |  ABMS| Provider
Nephrology | 38004479 | Nephrology                   | Physician Specialty | Medicare Specialty | Provider   
Neurodevelopmental Disabilities |45756787|Neurodevelopmental Disabilities|Physician Specialty |  ABMS| Provider
Neurological Surgery | 38004459 | Neurosurgery                    | Physician Specialty | Medicare Specialty | Provider   
General Neurology | 38004458 | Neurology                                     | Physician Specialty | Medicare Specialty | Provider   
Neurology with Special Qualification in Child Neurology |45756788|Neurology with Special Qualification in Child Neurology|Physician Specialty |  ABMS| Provider
Neuromuscular Medicine |45756789|Neuromuscular Medicine|Physician Specialty |  ABMS| Provider
Neuropathology |45756790 |Neuropathology|Physician Specialty |  ABMS| Provider
Neuroradiology |45756791|Neuroradiology|Physician Specialty |  ABMS| Provider
Neurotology |45756792|Neurotology|Physician Specialty |  ABMS| Provider
Nuclear Medicine |38004476 | Nuclear Medicine                  | Physician Specialty | Medicare Specialty | Provider   
Nuclear Radiology |45756793 |Nuclear Radiology|Physician Specialty |  ABMS| Provider
Obstetrics and Gynecology | 38004461 | Obstetrics/Gynecology              | Physician Specialty | Medicare Specialty | Provider   
Occupational Medicine |38004492 | Occupational Therapy              | Physician Specialty | Medicare Specialty| Provider   
Ophthalmology |  38004463 | Ophthalmology                       | Physician Specialty | Medicare Specialty | Provider     
Orthopaedic Sports Medicine |45756794|Orthopaedic Sports Medicine|Physician Specialty |  ABMS| Provider
Orthopedics/Orthopaedic Surgery |38004465 |Orthopedics/Orthopedic Surgery                | Physician Specialty | Medcicare Specialty | Provider   
Otolaryngology | 38004449 | Otolaryngology                           | Physician Specialty | Medicare Specialty | Provider     
Pain Medicine | 38004494 | Pain Management                           | Physician Specialty | Medicare Specialty  | Provider    
Pathology |38004466 | Pathology                                  | Physician Specialty | Medicare Specialty   | Provider   
Pathology - Anatomic |45756795|Pathology - Anatomic| Physician Specialty | ABMS| Provider
Pathology - Chemical |45756796|Pathology - Chemical | Physician Specialty | ABMS| Provider
Pathology - Clinical |45756797|Pathology - Clinical| Physician Specialty | ABMS| Provider
Pathology - Forensic |45756798|Pathology - Forensic| Physician Specialty | ABMS| Provider
Pathology - Hematology |45756799|Pathology - Hematology| Physician Specialty | ABMS| Provider
Pathology - Medical Microbiology |45756800 |Pathology - Medical Microbiology| Physician Specialty | ABMS| Provider
Pathology - Molecular Genetic |45756801|Pathology - Molecular Genetic| Physician Specialty | ABMS| Provider
Pathology - Pediatric |45756802|Pathology - Pediatric | Physician Specialty | ABMS| Provider
Pathology-Anatomic/Pathology-Clinical |45756803|Pathology-Anatomic/Pathology-Clinical | Physician Specialty | ABMS| Provider
Pediatric Medicine** | 38004477 | Pediatric Medicine               | Physician Specialty | Medicare Specialty   | Provider   
Pediatric Anesthesiology |45756804|Pediatric Anesthesiology| Physician Specialty | ABMS| Provider
Pediatric Cardiology |45756805|Pediatric Cardiology | Physician Specialty | ABMS| Provider
Pediatric Critical Care Medicine |45756806|Pediatric Critical Care Medicine| Physician Specialty | ABMS| Provider
Pediatric Dermatology |45756807|Pediatric Dermatology| Physician Specialty | ABMS| Provider
Pediatric Emergency Medicine |45756808|Pediatric Emergency Medicine| Physician Specialty | ABMS| Provider
Pediatric Endocrinology |45756809|Pediatric Endocrinology| Physician Specialty | ABMS| Provider
Pediatric Gastroenterology |45756810 |Pediatric Gastroenterology | Physician Specialty | ABMS| Provider
Pediatric Hematology-Oncology |45756811 |Pediatric Hematology-Oncology| Physician Specialty | ABMS| Provider
Pediatric Infectious Diseases |45756812 |Pediatric Infectious Diseases| Physician Specialty | ABMS| Provider
Pediatric Nephrology |45756813 |Pediatric Nephrology| Physician Specialty | ABMS | Provider   
Pediatric Otolaryngology |45756814|Pediatric Otolaryngology | Physician Specialty | ABMS| Provider
Pediatric Pulmonology |45756815|Pediatric Pulmonology | Physician Specialty | ABMS| Provider
Pediatric Radiology |45756816|Pediatric Radiology| Physician Specialty | ABMS| Provider
Pediatric Rehabilitation Medicine |45756817|Pediatric Rehabilitation Medicine| Physician Specialty | ABMS| Provider
Pediatric Rheumatology |45756818|Pediatric Rheumatology| Physician Specialty | ABMS| Provider
Pediatric Surgery |45756819|Pediatric Surgery| Physician Specialty | ABMS| Provider
Pediatric Transplant Hepatology |45756820|Pediatric Transplant Hepatology | Physician Specialty | ABMS| Provider
Pediatric Urology|45756821|Pediatric Urology| Physician Specialty | ABMS| Provider
Physical Medicine and Rehabilitation |38004468 | Physical Medicine And Rehabilitation | Physician Specialty | Medicare Specialty  | Provider    
Plastic Surgery | 38004467 | Plastic And Reconstructive Surgery  | Physician Specialty | Medicare Specialty | Provider     
Plastic Surgery Within the Head and Neck |45756822|Plastic Surgery Within the Head and Neck| Physician Specialty | ABMS| Provider
Preventative Medicine | 38004503 | Preventive Medicine                | Physician Specialty | Medicare Specialty | Provider     
Psychiatry |38004469 | Psychiatry                             | Physician Specialty | Medicare Specialty   | Provider   
Psychosomatic Medicine |45756823 |Psychosomatic Medicine | Physician Specialty | ABMS| Provider
Public Health and General Preventive Medicine |45756824|Public Health and General Preventive Medicine| Physician Specialty | ABMS| Provider
Pulmonary Disease | 38004472 | Pulmonary Disease         | Physician Specialty | Medicare Specialty   | Provider   
Radiation Oncology |38004509 | Radiation Oncology    | Physician Specialty | Medicare Specialty  | Provider    
Radiology |45756825|Radiology| Physician Specialty | ABMS| Provider
Reproductive Endocrinology/Infertility |45756826|Reproductive Endocrinology/Infertility| Physician Specialty | ABMS| Provider
Rheumatology |38004491 | Rheumatology                  | Physician Specialty | Medicare Specialty   | Provider   
Sleep Medicine |45756827|Sleep Medicine| Physician Specialty | ABMS| Provider
Spinal Cord Injury Medicine| concept id requested |Spinal Cord Injury Medicine | Physician Specialty | ABMS|
Sports Medicine |45756828|Sports Medicine| Physician Specialty | ABMS| Provider
General Surgery | 38004447 | General Surgery            | Physician Specialty | Medicare Specialty   | Provider   
Surgery of the Hand | 38004480 | Hand Surgery                  | Physician Specialty | Medicare Specialty   | Provider   
Surgical Critical Care |45756829|Surgical Critical Care | Physician Specialty | ABMS| Provider
Thoracic Surgery | 38004473 | Thoracic Surgery                  | Physician Specialty | Medicare Specialty  | Provider      
Thoracic and Cardiac Surgery |45756830|Thoracic and Cardiac Surgery| Physician Specialty | ABMS| Provider
Transplant Hepatology |45756831|Transplant Hepatology| Physician Specialty | ABMS| Provider
Undersea and Hyperbaric Medicine |45756832 |Undersea and Hyperbaric Medicine| Physician Specialty | ABMS| Provider
Urology | 38004474 | Urology                                   | Physician Specialty | Medicare Specialty | Provider    
Vascular and Interventional Radiology |45756833|Vascular and Interventional Radiology| Physician Specialty | ABMS| Provider
Vascular Neurology |45756834|Vascular Neurology| Physician Specialty | ABMS| Provider
Vascular Surgery | 38004496 | Vascular Surgery           | Physician Specialty | Medicare Specialty   | Provider   


### A2. Person Language Concept Mapping Values

The below langauge listing is representative of the top 10 spoken languages of each of the 8 contributing sites. This list standard list will be used to map language values for consistency.

Language|concept_id|concept_name|domain_id|concept_class_id|standard_concept
---|---|---|---|---|---|
Amharic|4182354|Amharic language|Observation|Qualifier Value|S
Arabic|4181374|Arabic language| Observation|Qualifier Value|S
Bengali|4052786|Bengali language|Observation|Qualifier Value|S
Burmese|4181727|Burmese language|Observation|Qualifier Value|S
Bosnian|40481563|Bosnian language|Observation|Qualifier Value|S
Cape Verde Creole|44814649 | Other        | Observation | Undefined        |                  | 
Chinese|4182948|Chinese Language|Observation|Qualifier Value|S
Chinese(Cantonese)|4177463|Cantonese Chinese dialect|Observation|Qualifier Value|S
Chinese(Mandarin)| 4181724| Mandarin dialect | Observation|Qualifier Value|S
English|4180186|English Language|Observation|Qualifier Value|S
French|4180190|French Language|Observation|Qualifier Value|S
Haitian/Creole|44802876| Haitian Creole Language|Observation|Qualifier Value|S
Japanese|4181524|Japanese Language|Observation|Qualifier Value|S
Korean|4175771|Korean Language|Observation|Qualifier Value|S
Mandarin| 4181724| Mandarin dialect | Observation|Qualifier Value|S
Nepali|4175908|Nepali language|Observation|Qualifier Value|S
No information| 44814650 | No information|Observation | Undefined|S                                                                                                                            
None|44814650 | No information|Observation | Undefined|S                                   
null|44814650 | No information|Observation | Undefined|S                                   
Other|44814649 | Other        | Observation | Undefined        |                  | 
Other Language|44814649 | Other        | Observation | Undefined        |                  | 
Other/Unknown|44814649 | Other        | Observation | Undefined        |                  | 
Portuguese|4181536 | Portuguese language                                 | Observation | Qualifier Value  | S
Russian|4181539 | Russian language                    | Observation | Qualifier Value  | S  
Sign|40483152 | Sign language                                               | Observation        | Qualifier Value   | S
Sign Language|40483152 | Sign language                                               | Observation        | Qualifier Value   | S
Somali|4182350 | Somali language                    | Observation | Qualifier Value  | S
Spanish|4182511 | Spanish language                    | Observation | Qualifier Value  | S
Unable to Collect| 44814650 | No information|Observation | Undefined|S        
Unknown | 44814653 | Unknown|  Observation | Undefined|S      
Vietnamese|4181526 | Vietnamese language                    | Observation | Qualifier Value  | S



* * *