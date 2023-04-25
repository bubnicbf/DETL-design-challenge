# Dynamic ETL Design Challenge
Design and implement a health data transformation and loading approach that automates part of the process through use of scalable, reusable and customizable code, while retaining manual aspects of the process that requires knowledge of complex coding syntax. The design needs to provide the flexibility required for the ETL of heterogeneous data, variations in semantic expertise, and transparency of transformation logic that are essential to implement ETL conventions across clinical research sharing networks. Processing workflows are directed by the ETL specifications guideline, developed by ETL designers with extensive knowledge of the structure and semantics of health data (i.e., “health data domain experts”) and target common data model (OMOP).

## D-ETL
The four key components of the D-ETL approach are:

1. Comprehensive ETL specifications: The ETL specifications provide a master plan for the entire ETL process, outlining the scope of data to be extracted, the target data model, and the format of the input and output data files. These specifications provide a high-level view of the ETL process, helping to ensure that all stakeholders have a common understanding of the requirements.

2. D-ETL rules in plain text format: The D-ETL rules are composed in plain text format, making them human-readable and easily scrutinized, maintained, shared, and reused. This approach ensures that the ETL process is transparent and easy to understand, which is critical for ensuring that it meets the needs of all stakeholders.

3. Efficient ETL rules engine: The ETL rules engine generates full SQL statements from the D-ETL rules, enabling the transformation, conformation, and loading of data into target tables. This engine is designed to be efficient and scalable, able to handle large volumes of data quickly and accurately.

4. Accessible auto-generated SQL statements: The auto-generated SQL statements are accessible by ETL designers, enabling them to execute, test, and debug the rules. This supports an iterative process of validation and debugging, ensuring that the ETL process is accurate and reliable.

Together, these components form a powerful and flexible approach to ETL, capable of handling heterogeneous data, variations in semantic expertise, and the need for transparency of transformation logic. By automating part of the ETL process and retaining manual aspects that require complex coding syntax, the D-ETL approach ensures that the ETL process is accurate, efficient, and scalable, while meeting the needs of all stakeholders.

### Spec Guide

The ETL specifications guidelines document is an essential component of the D-ETL approach, as it provides a comprehensive overview of the data extraction, transformation, and loading process. The document typically includes the following information:

Source and target schemas: The document outlines the structure and format of the source and target schemas, including the data types, fields, and relationships between them. This information is critical for ensuring that data is accurately mapped and transformed from the source to the target schema.

Terminology mappings: The document includes mappings between data elements and values in the source and target schemas. This ensures that data is accurately translated from one schema to the other, even if the terminologies or data elements differ.

Definitions and conventions: The document outlines definitions and conventions for data in the target schema, including how data is categorized and structured, as well as any rules for formatting or validation. This information ensures that the target schema is consistent and standardized, even if data from different sources is being integrated.

The ETL specifications document is typically created by health data domain experts, who have extensive knowledge of the source and target schemas. These experts are responsible for ensuring that the document accurately reflects the requirements of the ETL process, as well as any data quality or semantic issues that may arise. They may work closely with other stakeholders, such as ETL designers, to ensure that the document is comprehensive and easy to understand.

The document is typically created in a standard word processing application, such as Microsoft Word or Google Docs. This ensures that it is easy to edit and update as requirements change, and that it can be shared easily with other stakeholders. The document should be reviewed and updated regularly, to ensure that it remains accurate and up-to-date with any changes in the source or target schemas.

## Extraction

The data extraction step is a critical component of the ETL process, as it involves retrieving data from the source system and preparing it for transformation and loading into the target database. In the D-ETL approach, data is extracted from the source system and stored temporarily in a data storage, typically a file-based system or a database.

To enable efficient and standardized data exchange, the D-ETL approach uses comma-separated values (CSV) text files for data exchange. CSV files are widely used and accepted, and can be easily manipulated and transformed using a variety of tools and applications.

Once the data has been extracted and stored in the temporary data storage, it goes through a data validation process. This process includes input data checks for missing data in required fields and orphan foreign key values. For example, if a foreign key column references a primary key column, but the value in the foreign key column is not present in the primary key column, this is considered an orphan foreign key value.

In addition to input data checks, the data transformation processes may have specific assumptions about the value and structure of input data that require validation. For example, if the transformation process assumes that a particular field contains only numeric values, any non-numeric values will need to be identified and addressed before the data can be transformed.

Data validation is an important step in the ETL process, as it ensures that the data is accurate and consistent before it is loaded into the target database. By identifying and addressing any data quality issues at this stage, the ETL process can ensure that the target database is accurate and reliable, which is critical for downstream analysis and decision-making.

### Validation

Here is a technical outline of the validation process used in the D-ETL approach:

1. Input data validation: The validation process begins with input data validation, which involves checking that all required fields are present and contain valid values. This includes checking for missing data, null values, and invalid data types.

- Various validation checks are performed using built-in R functions, such as is.na() for checking for missing data, is.numeric() for checking for invalid data types, and %in% for checking for foreign key values in a reference table.

- The stop() function is used to raise an error if any validation checks fail. This ensures that the ETL process will be halted if any data quality issues are detected, preventing inaccurate or inconsistent data from being loaded into the target database.

```
# Load data into a data frame
df <- read.csv("input_data.csv")

# Check for missing data
missing_data <- sapply(df, function(x) sum(is.na(x)))
if (sum(missing_data) > 0) {
  stop("Missing data detected in input data")
}

# Check for invalid data types
invalid_data_types <- sapply(df, function(x) !is.numeric(x))
if (sum(invalid_data_types) > 0) {
  stop("Invalid data types detected in input data")
}

# Check for foreign key values
foreign_keys <- df$foreign_key_column
if (any(!foreign_keys %in% reference_table$primary_key_column)) {
  stop("Invalid foreign key values detected in input data")
}

# Check for value ranges
value_range <- df$numeric_column
if (any(value_range < 0 | value_range > 100)) {
  stop("Invalid value ranges detected in input data")
}

# Check for data consistency
data_consistency <- df$related_field == df$related_field2
if (!all(data_consistency)) {
  stop("Inconsistent data detected in input data")
}

```

2. Foreign key validation: Next, the validation process checks the foreign key values in the input data. This involves ensuring that all foreign key values are valid, by checking that they correspond to primary key values in the reference tables.

- Reference table is loaded into a separate data frame. The foreign key column in the input data frame is then compared to the primary key column in the reference table to check for valid foreign key values.

- The !foreign_keys %in% valid_foreign_keys expression returns a boolean vector indicating whether each foreign key value is valid or not. The any() function is used to check if any invalid foreign key values are present. If so, the invalid values are stored in the invalid_foreign_key_values variable and an error message is generated using the stop() function.

```
# Load reference table into a data frame
reference_df <- read.csv("reference_table.csv")

# Check for valid foreign key values
foreign_keys <- df$foreign_key_column
valid_foreign_keys <- reference_df$primary_key_column
invalid_foreign_keys <- !foreign_keys %in% valid_foreign_keys
if (any(invalid_foreign_keys)) {
  invalid_foreign_key_values <- foreign_keys[invalid_foreign_keys]
  stop("Invalid foreign key values detected in input data: ", paste(invalid_foreign_key_values, collapse = ", "))
}

```

3. Data type validation: Once the input data has been validated, the validation process checks the data types of each field. This involves ensuring that each field contains data of the expected type, such as numeric, string, or date values.

- The sapply() function is used to apply the is.numeric() function to each column of the data frame, returning a boolean vector indicating whether each column contains numeric data or not.

- The sum() function is used to count the number of invalid columns, and the names() function is used to retrieve the names of the invalid columns. If any invalid data types are detected, an error message is generated using the stop() function, indicating the names of the columns with invalid data types.

```
# Check for invalid data types
invalid_data_types <- sapply(df, function(x) !is.numeric(x))
if (sum(invalid_data_types) > 0) {
  invalid_columns <- names(df)[invalid_data_types]
  stop("Invalid data types detected in columns: ", paste(invalid_columns, collapse = ", "))
}

```

4. Data format validation: In addition to data type validation, the validation process checks the data format of each field. This involves ensuring that the data in each field conforms to a specific format, such as a particular date format or phone number format.

- The grepl() function is used to search for valid patterns in the date_of_birth, phone_number, and email_address columns.

The regular expressions used in this example check for the following patterns:

Date of birth: YYYY-MM-DD format
Phone number: XXX-XXX-XXXX format
Email address: any_word@any_word.any_extension format
If any invalid patterns are detected, an error message is generated using the stop() function, indicating the specific column and invalid value(s) that were detected.

```
# Load patient demographic data into a data frame
demographics <- read.csv("patient_demographics.csv")

# Check for valid date format
invalid_date_format <- !grepl("\\d{4}-\\d{2}-\\d{2}", demographics$date_of_birth)
if (any(invalid_date_format)) {
  invalid_dates <- demographics$date_of_birth[invalid_date_format]
  stop("Invalid date format detected in date_of_birth column: ", paste(invalid_dates, collapse = ", "))
}

# Check for valid phone number format
invalid_phone_format <- !grepl("\\d{3}-\\d{3}-\\d{4}", demographics$phone_number)
if (any(invalid_phone_format)) {
  invalid_phone_numbers <- demographics$phone_number[invalid_phone_format]
  stop("Invalid phone number format detected in phone_number column: ", paste(invalid_phone_numbers, collapse = ", "))
}

# Check for valid email address format
invalid_email_format <- !grepl("\\w+@\\w+\\.\\w+", demographics$email_address)
if (any(invalid_email_format)) {
  invalid_emails <- demographics$email_address[invalid_email_format]
  stop("Invalid email address format detected in email_address column: ", paste(invalid_emails, collapse = ", "))
}

```

5. Value range validation: The validation process checks the range of values in each field, to ensure that they fall within a specific range or set of values. This is particularly important for numeric fields, where values that are outside of an expected range may indicate data quality issues.

- The invalid_value_ranges variable is created by checking for values in the blood_pressure column that are less than 0 or greater than 200. Similarly, the invalid_temperature_range variable is created by checking for values in the body_temperature column that are less than 95 or greater than 105, and the invalid_heart_rate_range variable is created by checking for values in the heart_rate column that are less than 40 or greater than 120.

If any invalid value ranges are detected, an error message is generated using the stop() function, indicating the specific column and invalid value(s) that were detected.

```
# Load patient observation data into a data frame
observations <- read.csv("patient_observations.csv")

# Check for valid value ranges
invalid_value_ranges <- observations$blood_pressure < 0 | observations$blood_pressure > 200
if (any(invalid_value_ranges)) {
  invalid_values <- observations$blood_pressure[invalid_value_ranges]
  stop("Invalid value range detected in blood_pressure column: ", paste(invalid_values, collapse = ", "))
}

# Check for valid body temperature range
invalid_temperature_range <- observations$body_temperature < 95 | observations$body_temperature > 105
if (any(invalid_temperature_range)) {
  invalid_temperatures <- observations$body_temperature[invalid_temperature_range]
  stop("Invalid value range detected in body_temperature column: ", paste(invalid_temperatures, collapse = ", "))
}

# Check for valid heart rate range
invalid_heart_rate_range <- observations$heart_rate < 40 | observations$heart_rate > 120
if (any(invalid_heart_rate_range)) {
  invalid_heart_rates <- observations$heart_rate[invalid_heart_rate_range]
  stop("Invalid value range detected in heart_rate column: ", paste(invalid_heart_rates, collapse = ", "))
}

```

6. Data consistency validation: The validation process checks the consistency of the data, to ensure that it is internally consistent and matches expectations. This may involve checking that certain fields have consistent values across different records or ensuring that certain relationships between data elements are maintained.

- The gender_pregnancy_consistency variable is created by checking for cases where the gender is "Female" but the pregnancy status is "No". If any inconsistencies are detected, an error message is generated using the stop() function, indicating the rows where the inconsistencies were found.

- The age_consistency variable is created by checking that the calculated age matches the age provided in the data, based on the date of birth and the current date. If any inconsistencies are detected, an error message is generated using the stop() function, indicating the rows where the inconsistencies were found.

```
# Load patient demographics data into a data frame
demographics <- read.csv("patient_demographics.csv")

# Check for data consistency between gender and pregnancy status
gender_pregnancy_consistency <- demographics$gender == "Female" & demographics$is_pregnant == "No"
if (!all(gender_pregnancy_consistency)) {
  inconsistent_rows <- which(!gender_pregnancy_consistency)
  stop("Inconsistent data detected between gender and pregnancy status in rows: ", paste(inconsistent_rows, collapse = ", "))
}

# Check for data consistency between age and date of birth
today <- as.Date("2023-04-25")
age_consistency <- demographics$age == as.numeric(format(today, "%Y")) - as.numeric(format(demographics$date_of_birth, "%Y")))
if (!all(age_consistency)) {
  inconsistent_rows <- which(!age_consistency)
  stop("Inconsistent data detected between age and date of birth in rows: ", paste(inconsistent_rows, collapse = ", "))
}

```

7. Cross-field validation: The validation process checks for cross-field validation, which involves ensuring that the data in one field is consistent with the data in another field. For example, if a record contains a date field and a related status field, the validation process may check that the status field is consistent with the date field.

- The invalid_drug_exposure variable is created by checking that the drug exposure end date is not earlier than the drug exposure start date and that the drug concept ID is not missing. If any inconsistencies are detected, an error message is generated using the stop() function, indicating the rows where the inconsistencies were found.

- The invalid_condition_occurrence variable is created by checking that the condition start date is not later than the condition end date and that the condition concept ID is not missing. If any inconsistencies are detected, an error message is generated using the stop() function, indicating the rows where the inconsistencies were found.

- The invalid_procedure_occurrence variable is created by checking that the procedure date is not later than the current date and that the procedure concept ID is not missing. If any inconsistencies are detected, an error message is generated using the stop() function, indicating the rows where the inconsistencies were found.

```
# Load OMOP data into a data frame
omop_data <- read.csv("omop_data.csv")

# Check for valid relationship between drug exposure and drug concept
invalid_drug_exposure <- omop_data$drug_exposure_end_date < omop_data$drug_exposure_start_date | is.na(omop_data$drug_concept_id)
if (any(invalid_drug_exposure)) {
  invalid_rows <- which(invalid_drug_exposure)
  stop("Invalid relationship detected between drug exposure and drug concept in rows: ", paste(invalid_rows, collapse = ", "))
}

# Check for valid relationship between condition occurrence and condition concept
invalid_condition_occurrence <- omop_data$condition_start_date > omop_data$condition_end_date | is.na(omop_data$condition_concept_id)
if (any(invalid_condition_occurrence)) {
  invalid_rows <- which(invalid_condition_occurrence)
  stop("Invalid relationship detected between condition occurrence and condition concept in rows: ", paste(invalid_rows, collapse = ", "))
}

# Check for valid relationship between procedure occurrence and procedure concept
invalid_procedure_occurrence <- omop_data$procedure_date > Sys.Date() | is.na(omop_data$procedure_concept_id)
if (any(invalid_procedure_occurrence)) {
  invalid_rows <- which(invalid_procedure_occurrence)
  stop("Invalid relationship detected between procedure occurrence and procedure concept in rows: ", paste(invalid_rows, collapse = ", "))
}

```

### Additional validation

8. Pattern validation: The validation process checks for patterns in the data, to ensure that they match a predefined pattern or regular expression. This may involve checking that email addresses, phone numbers, or postal codes are formatted correctly, for example.

In general, pattern validation is a type of data format validation. Both pattern validation and data format validation are techniques used to ensure that data is in the correct format or structure.

Pattern validation involves checking whether a string of characters matches a particular pattern or regular expression. This can be used to ensure that data is in the correct format, such as checking that a phone number is in the format "XXX-XXX-XXXX".

Data format validation is a broader term that encompasses pattern validation as well as other techniques used to validate data formats, such as checking the length of a string, verifying that a numeric value falls within a specified range, or ensuring that a date is in the correct format.

- The invalid_npi_format variable is created by checking that the National Provider Identifier (NPI) is a 10-digit number. If any invalid NPI formats are detected, an error message is generated using the stop() function, indicating the specific values in the npi column that were detected.

- The invalid_taxonomy_format variable is created by checking that the Taxonomy Code is a 10-character string consisting of digits and uppercase letters. If any invalid Taxonomy Code formats are detected, an error message is generated using the stop() function, indicating the specific values in the taxonomy_code column that were detected.

- The invalid_zip_format variable is created by checking that the ZIP Code is in the format "XXXXX" or "XXXXX-XXXX". If any invalid ZIP Code formats are detected, an error message is generated using the stop() function, indicating the specific values in the zip_code column that were detected.

```
# Load provider data into a data frame
providers <- read.csv("provider_data.csv")

# Check for valid National Provider Identifier (NPI) format
invalid_npi_format <- !grepl("^\\d{10}$", providers$npi)
if (any(invalid_npi_format)) {
  invalid_npi <- providers$npi[invalid_npi_format]
  stop("Invalid NPI format detected in NPI column: ", paste(invalid_npi, collapse = ", "))
}

# Check for valid Taxonomy Code format
invalid_taxonomy_format <- !grepl("^[\\dA-Z]{10}$", providers$taxonomy_code)
if (any(invalid_taxonomy_format)) {
  invalid_codes <- providers$taxonomy_code[invalid_taxonomy_format]
  stop("Invalid Taxonomy Code format detected in taxonomy_code column: ", paste(invalid_codes, collapse = ", "))
}

# Check for valid ZIP Code format
invalid_zip_format <- !grepl("^\\d{5}(-\\d{4})?$", providers$zip_code)
if (any(invalid_zip_format)) {
  invalid_zips <- providers$zip_code[invalid_zip_format]
  stop("Invalid ZIP Code format detected in zip_code column: ", paste(invalid_zips, collapse = ", "))
}

```

9. Referential integrity validation: The validation process checks for referential integrity, which involves ensuring that all references between tables are valid. This may involve checking that foreign keys refer to primary keys, or that references between tables are maintained during the transformation process.

Referential integrity validation is a broader term that encompasses foreign key validation as well as other types of validation checks that ensure the consistency and accuracy of relationships between tables in a database.

Foreign key validation is a specific type of referential integrity validation that involves checking that a foreign key value in one table matches a primary key value in another table. This ensures that there are no orphaned records (i.e., records that reference a nonexistent primary key value), and that the relationships between tables are maintained correctly.

Referential integrity validation can involve additional checks beyond foreign key validation, such as checking for cascading updates or deletes, enforcing unique constraints, and checking for circular references or other inconsistencies in relationships between tables.

-  Referential integrity validation checks are then performed to ensure that the relationship between the drug_concept_id field in the drug_exposure table and the drug_concept_id field in the drug_strength table is valid.

- The invalid_drug_exposure variable is created by checking that all values in the drug_concept_id field in the drug_exposure table are also present in the drug_concept_id field in the drug_strength table. If any invalid drug_concept_id values are detected, an error message is generated using the stop() function, indicating the specific values in the drug_concept_id column that were detected.

```
# Load OMOP data into data frames
drug_exposure <- read.csv("drug_exposure.csv")
drug_strength <- read.csv("drug_strength.csv")

# Check for valid relationship between drug exposure and drug strength
invalid_drug_exposure <- !drug_exposure$drug_concept_id %in% drug_strength$drug_concept_id
if (any(invalid_drug_exposure)) {
  invalid_drug_ids <- unique(drug_exposure$drug_concept_id[invalid_drug_exposure])
  stop("Invalid drug_concept_id(s) detected in drug_exposure table: ", paste(invalid_drug_ids, collapse = ", "))
}

```

10. Data completeness validation: The validation process checks for data completeness, which involves ensuring that all required fields are present and contain data. This may involve checking that certain fields are not null, or that all records contain a specific set of fields.

- The missing_values variable is created by checking for missing values in the person_id, condition_concept_id, and condition_start_date columns of the condition_occurrence table. If any missing values are detected, an error message is generated using the stop() function, indicating the rows where the missing values were found.

- The duplicated_rows variable is created by checking for duplicate rows in the condition_occurrence table based on the combination of person_id, condition_concept_id, and condition_start_date. If any duplicate rows are detected, an error message is generated using the stop() function, indicating the rows where the duplicate values were found.

```
# Load OMOP data into data frames
condition_occurrence <- read.csv("condition_occurrence.csv")

# Check for missing values in required fields
missing_values <- apply(condition_occurrence[, c("person_id", "condition_concept_id", "condition_start_date")], 1, function(x) any(is.na(x)))
if (any(missing_values)) {
  missing_rows <- which(missing_values)
  stop("Missing data detected in condition_occurrence table in rows: ", paste(missing_rows, collapse = ", "))
}

# Check for duplicates
duplicated_rows <- duplicated(condition_occurrence[, c("person_id", "condition_concept_id", "condition_start_date")])
if (any(duplicated_rows)) {
  duplicate_indices <- which(duplicated_rows)
  stop("Duplicate data detected in condition_occurrence table in rows: ", paste(duplicate_indices, collapse = ", "))
}

```

11. Business rule validation: The validation process checks for compliance with business rules, which are rules that govern how data should be handled based on business requirements. This may involve checking that certain fields contain values that meet specific business requirements, such as legal or regulatory requirements.

Some examples:
- Date of service validation: This type of validation checks that the date of service on a medical claim falls within a specific date range. For example, claims may be rejected if the date of service is more than one year old.

```
# Load claims data into a data frame
claims <- read.csv("claims_data.csv")

# Check for date of service within a specific range
invalid_dates <- !(claims$date_of_service >= "2022-01-01" & claims$date_of_service <= "2022-12-31")
if (any(invalid_dates)) {
  invalid_rows <- which(invalid_dates)
  stop("Invalid date of service detected in rows: ", paste(invalid_rows, collapse = ", "))
}

```

- Eligibility validation: This type of validation checks that the patient was eligible for the services that were provided. For example, claims may be rejected if the patient's insurance coverage had expired before the date of service.

```
# Load claims data and eligibility data into data frames
claims <- read.csv("claims_data.csv")
eligibility <- read.csv("eligibility_data.csv")

# Check for patient eligibility on date of service
invalid_eligibility <- !(claims$patient_id %in% eligibility$patient_id & 
                         claims$date_of_service >= eligibility$start_date & 
                         claims$date_of_service <= eligibility$end_date)
if (any(invalid_eligibility)) {
  invalid_rows <- which(invalid_eligibility)
  stop("Invalid eligibility detected in rows: ", paste(invalid_rows, collapse = ", "))
}

```

- Service code validation: This type of validation checks that the service codes on a medical claim are valid for the type of service provided. For example, claims may be rejected if a service code for an emergency room visit is used for a routine checkup.

```
# Load claims data into a data frame
claims <- read.csv("claims_data.csv")

# Check for valid service codes
invalid_service_codes <- !(claims$service_code %in% c("A123", "B456", "C789"))
if (any(invalid_service_codes)) {
  invalid_rows <- which(invalid_service_codes)
  stop("Invalid service code detected in rows: ", paste(invalid_rows, collapse = ", "))
}

```
- Duplicate claim validation: This type of validation checks that there are no duplicate claims for the same service provided to the same patient. For example, claims may be rejected if a claim has already been processed for the same service on the same date of service.

```
# Load claims data into a data frame
claims <- read.csv("claims_data.csv")

# Check for duplicate claims
duplicated_claims <- duplicated(claims[, c("patient_id", "date_of_service", "service_code")])
if (any(duplicated_claims)) {
  duplicate_rows <- which(duplicated_claims)
  stop("Duplicate claims detected in rows: ", paste(duplicate_rows, collapse = ", "))
}

```

- Provider network validation: This type of validation checks that the provider who provided the service is part of the network for the patient's insurance plan. For example, claims may be rejected if the provider is not in-network for the patient's insurance plan.

```
# Load claims data and provider data into data frames
claims <- read.csv("claims_data.csv")
providers <- read.csv("provider_data.csv")

# Check for valid provider network
invalid_providers <- !(claims$provider_id %in% providers$provider_id & 
                       claims$insurance_id %in% providers$insurance_id)
if (any(invalid_providers)) {
  invalid_rows <- which(invalid_providers)
  stop("Invalid provider network detected in rows: ", paste(invalid_rows, collapse = ", "))
}

```

- Diagnosis code validation: This type of validation checks that the diagnosis codes on a medical claim are valid for the type of service provided. For example, claims may be rejected if a diagnosis code for a minor illness is used for a major medical procedure.

```
# Load claims data into a data frame
claims <- read.csv("claims_data.csv")

# Check for valid diagnosis codes
invalid_diagnosis_codes <- !(claims$diagnosis_code %in% c("123", "456", "789"))
if (any(invalid_diagnosis_codes)) {
  invalid_rows <- which(invalid_diagnosis_codes)
  stop("Invalid diagnosis code detected in rows: ", paste(invalid_rows, collapse = ", "))
}

```

12. Record duplication validation: The validation process checks for record duplication, which involves ensuring that each record is unique and does not appear more than once in the input data. This may involve checking for duplicate values in certain fields, such as a unique identifier field.

- Record duplication validation checks are then performed to ensure that there are no duplicate records in the procedure_occurrence table based on the combination of person_id, procedure_concept_id, and procedure_date.

- The duplicated_records variable is created by checking for duplicate rows in the procedure_occurrence table based on the combination of person_id, procedure_concept_id, and procedure_date. If any duplicate rows are detected, an error message is generated using the stop() function, indicating the rows where the duplicate values were found.

```
# Load OMOP data into a data frame
procedure_occurrence <- read.csv("procedure_occurrence.csv")

# Check for duplicate records
duplicated_records <- duplicated(procedure_occurrence[, c("person_id", "procedure_concept_id", "procedure_date")])
if (any(duplicated_records)) {
  duplicate_indices <- which(duplicated_records)
  stop("Duplicate data detected in procedure_occurrence table in rows: ", paste(duplicate_indices, collapse = ", "))
}

```