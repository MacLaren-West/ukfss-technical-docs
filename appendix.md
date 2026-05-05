# Appendix

[← Back to overview](README.md)

---

## Data Model & Field Mappings

### Sample Mapping

| Legacy Name                | API Field                    | Required |
| -------------------------- | ---------------------------- | -------- |
| *(New Field)*              | fsId                         | Yes      |
| SampleNumber               | fsReference                  | Yes      |
| LocalAuthorityCode         | authorityCode                | Yes      |
| LocalAuthorityOfficeCode   | authorityOfficeCode          | Yes      |
| LocalAuthoritySampleNumber | authorityReference           | Yes      |
| CategoryCode               | detailCategoryCode1          | Optional |
| SubcategoryCode            | detailCategoryCode2          | Optional |
| Subcategory2Code           | detailCategoryCode3          | Optional |
| Subcategory3Code           | detailCategoryCode4          | Optional |
| LaboratoryComments         | laboratoryComments           | Yes      |
| LIMSSampleNumber           | laboratoryReference          | Yes      |
| IsSatisfactory             | laboratoryIsSatisfactory     | Yes      |
| StartDate                  | laboratoryAnalysisStartDate  | Yes      |
| FailCode                   | laboratorySampleFailureCode  | Yes      |
| RecordedDate               | laboratoryAnalysisReportDate | Yes      |

### Result Mapping

| Legacy Name | API Field | Required |
|------------|----------|----------|
| *(New Field)* | laboratoryResultId | Yes |
| ResultTimeStamp | resultTimestamp | Yes |
| Determination | determinationCode | Yes |
| TestSubstance | testSubstance | Yes |
| TestUnits | testUnits | Yes |
| ResultType | resultTypeCode | Yes |
| Output | formattedOutput | Yes |
| TextResult | textResult | Conditional |
| Result | numericResult | Conditional |
| Qualifier | numericResultQualifier | Conditional |
| LODLOQ | lodloq | Optional |
| FailCode | outcomeCode | Yes |
| ComponentDescription | componentDescription | Optional |

### Outcome Mapping

| Legacy Name | API Field | Required |
|------------|----------|----------|
| Code | outcomeCode | Yes |

---

## Constraints & Validation

- All dates are UTC
- `fsId` is immutable
- Payloads must strictly match the defined schema
- Additional JSON properties are rejected
- Each result must include either `textResult` or `numericResult`

---

## Client Field Mapping

This is a list of all fields available for import from the Client (UKFSS Desktop) to the laboratory.

| Legacy Name                         | Api Name                         |
| :---------------------------------- | :------------------------------- |
| RECORD\_TYPE                        | fsRecordTypeCode                 |
| ANALYSIS\_TYPE                      | fsAnalysisTypeCode               |
| CODE                                | fsReference                      |
| SAMPLE\_STATUS\_\_CODE              | fsStatusCode                     |
| ADDITIONAL\_INFO                    | fsAdditionalInformation          |
| SAMPLE\_TIMESTAMP                   | authorityDateTimeSampleTaken     |
| SAMPLE\_DATE                        | authorityDateSampleTaken         |
| SAMPLE\_TIME                        | authorityTimeSampleTaken         |
| LOCAL\_AUTHORITY\_\_CODE            | authorityCode                    |
| LOCAL\_AUTH\_OFFICE\_\_CODE         | authorityOfficeCode              |
| LA\_SAMPLENO                        | authorityReference               |
| SAMPLE\_OFFICER\_\_CODE             | authoritySamplingOfficerCode     |
| SAMPLE\_OFFICER\_NAME               | authoritySamplingOfficerName     |
| SAMPLE\_OFFICER\_EMAIL\_ADDRESS     | authoritySamplingOfficerEmail    |
| LABORATORY\_\_CODE                  | laboratoryCode                   |
| ANALYSIS\_TEXT                      | laboratoryAnalysisRequiredDetail |
| BUSINESS\_ID                        | premisesBusinessId               |
| PREMISES\_NAME                      | premisesTradingName              |
| BUSINESS\_ADDR\_1                   | premisesAddressLine1             |
| BUSINESS\_ADDR\_2                   | premisesAddressLine2             |
| BUSINESS\_ADDR\_3                   | premisesAddressLine3             |
| BUSINESS\_ADDR\_4                   | premisesAddressLine4             |
| BUSINESS\_POSTCODE                  | premisesPostcode                 |
| RISK\_CATEGORY                      | premisesFoodHygieneRiskRating    |
| RISK\_CATEGORY\_FOOD\_STAND\_\_CODE | premisesFoodStandardsRiskRating  |
| PREMISES\_TYPE\_\_CODE              | premisesTypeCode                 |
| REASON\_\_CODE                      | reasonCode                       |
| SAMPLE\_TYPE\_\_CODE                | reasonTypeCode                   |
| FLAG\_FOLLOW\_UP                    | reasonIsFollowupSample           |
| INDEX\_NUMBER                       | reasonFollowupSampleReference    |
| FLAG\_FOOD\_POISONING               | reasonIsFoodborneIllness         |
| DETAILS                             | reasonFoodborneIllnessDetail     |
| SURVEY\_BODY\_\_CODE                | surveyBodyCode                   |
| FLAG\_SURVEY                        | surveyIsSurvey                   |
| SURVEY\_NUMBER                      | surveyReference                  |
| CATEGORY\_\_CODE                    | detailCategoryCode1              |
| SUB\_CATEGORY\_\_CODE               | detailCategoryCode2              |
| SUB\_CATEGORY\_2\_\_CODE            | detailCategoryCode3              |
| SUB\_CATEGORY\_3\_\_CODE            | detailCategoryCode4              |
| FOOD\_DESCRIPTION                   | detailDescription                |
| REGISTRATION\_NUMBER                | feedRegistrationNumber           |
| DATE\_OF\_MANUFACTURE               | feedDateOfManufacture            |
| ANIMAL\_SPECIES\_\_CODE             | feedAnimalSpeciesCode            |
| LABEL\_REGISTRATION\_NUMBER         | feedLabelRegistrationNumber      |
| LABEL\_BUSINESS\_TYPE\_\_CODE       | feedLabelBusinessTypeCode        |
| LABEL\_BUSINESS\_ADDRESS            | feedLabelBusinessAddress         |
| BRAND\_NAME                         | detailBrandName                  |
| PRODUCT\_\_CODE                     | detailNatureOfProductCode        |
| MANUFACTURER                        | detailManufacturer               |
| DISTRIBUTER                         | detailDistributor                |
| IMPORTER                            | detailImporter                   |
| COUNTRY                             | detailCountryOfOrigin            |
| PACKAGING\_\_CODE                   | packagingProvidedCode            |
| MATERIAL\_\_CODE                    | packagingMaterialCode            |
| OTHER\_MATERIAL                     | packagingOtherMaterialDetail     |
| QUANTITY                            | packagingPackQuantity            |
| PACK\_SIZE                          | packagingPackUnitsCode           |
| BATCH\_NUMBER                       | packagingBatchNumber             |
| HEALTH\_MARK                        | packagingHealthMark              |
| DURABILITY\_\_CODE                  | durabilityCode                   |
| DURABILITY\_DAY                     | durabilityDay                    |
| DURABILITY\_MONTH                   | durabilityMonth                  |
| DURABILITY\_YEAR                    | durabilityYear                   |
| CONDITION\_\_CODE                   | conditionCode                    |
| OTHER\_CONDITION                    | conditionOtherDetail             |
| TEMPERATURE                         | conditionTemperatureWhenTaken    |
| FLAG\_COP\_CONDITION                | codeOfPracticeCompliant          |
| NOT\_COP                            | codeOfPracticeNotCompliantDetail |
| FLAG\_COP\_LAB                      | codeOfPracticeTransportCompliant |
| LABELLING\_DETAILS                  | packagingLabellingDetail         |
| SAMPLE\_COMMENTS                    | authorityComments                |
| ANALYSIS\_COST                      | authorityAnalysisCost            |
| PURCHASE\_COST                      | authorityPurchaseCost            |

---

[← Reference Endpoints](reference-endpoints.md)
