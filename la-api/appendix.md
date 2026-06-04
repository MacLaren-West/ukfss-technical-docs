# Appendix

[← Back to overview](README.md)

---

## Full Field Reference

All fields accepted by `POST /api/SampleEntry/Save`. Fields not marked required are optional and may be omitted or passed as `null`.

### Sample Identification

| Field | Type | Notes |
| :---- | :--- | :---- |
| `fsRecordTypeCode` | string | **Required.** `"FOOD"` or `"ANIMAL FEED"` (with space) |
| `fsAnalysisTypeCode` | string | **Required.** `"C"` (chemical) or `"M"` (microbiology) |
| `fsStatusCode` | string | Always assigned and managed by UKFSS — never set this field |
| `fsReference` | string | Omit for new samples — UKFSS reference number, assigned on creation. Include on subsequent calls to update an existing record |
| `fsAdditionalInformation` | string | Free-text additional information |

### Authority

| Field | Type | Notes |
| :---- | :--- | :---- |
| `authorityCode` | string | **Required.** Your local authority code — must match an authority your API key is authorised for. Returns `403` if missing or mismatched |
| `authorityOfficeCode` | string | **Required.** The sampling office code |
| `authorityReference` | string | Your system's own sample reference |
| `authorityDateTimeSampleTaken` | string | **Required.** ISO 8601 datetime, e.g. `"2025-10-15T10:30:00"`. Seconds are optional — `"2025-10-15T10:30"` is also accepted. Use `"T03:00:00"` if time is unknown |
| `authoritySamplingOfficerCode` | string | **Required.** Sampling officer code. GUID-length values are supported |
| `authoritySamplingOfficerName` | string | Sampling officer full name |
| `authoritySamplingOfficerEmail` | string | Sampling officer email address |
| `authorityComments` | string | Free-text comments from the sampling officer |
| `authorityPurchaseCost` | number | Cost to purchase the sample (decimal) |
| `authorityAnalysisCost` | number | Cost of analysis (decimal) |

### Laboratory

| Field | Type | Notes |
| :---- | :--- | :---- |
| `laboratoryCode` | string | **Required.** Code of the assigned laboratory |
| `laboratoryRoutineAnalysisRequired` | boolean | **Required.** Whether routine analysis is requested — `true` or `false`, not a string |
| `laboratoryAnalysisRequiredDetail` | string | **Required.** Free-text description of analysis required |

### Premises

| Field | Type | Notes |
| :---- | :--- | :---- |
| `premisesBusinessId` | string | **Required.** Premises business identifier |
| `premisesTradingName` | string | **Required.** Trading name of the premises |
| `premisesAddressLine1` | string | Address line 1 |
| `premisesAddressLine2` | string | Address line 2 |
| `premisesAddressLine3` | string | Address line 3 |
| `premisesAddressLine4` | string | Address line 4 |
| `premisesPostcode` | string | Postcode |
| `premisesFoodHygieneRiskRating` | string | Food hygiene risk rating code — see `foodhygieneriskcategory` reference data |
| `premisesFoodStandardsRiskRating` | string | Food standards risk rating code — see `foodstandardsriskcategory` reference data |
| `premisesTypeCode` | string | **Required.** Premises type code — see `foodpremisestype` / `feedpremisestype` reference data |

### Reason

| Field | Type | Notes |
| :---- | :--- | :---- |
| `reasonCode` | string | **Required.** Reason code — see `foodreasontaken` / `feedreasontaken` reference data |
| `reasonTypeCode` | string | **Required.** Reason type code — see `foodreasontype` / `feedreasontype` reference data |
| `reasonFollowupSampleReference` | string | Original sample reference if this is a follow-up |
| `reasonFoodborneIllnessDetail` | string | If sample relates to a foodborne illness, supply detail here. If the flag was previously set but no detail exists, use `"Yes"` as a placeholder |

### Survey

| Field | Type | Notes |
| :---- | :--- | :---- |
| `surveyBodyCode` | string | Survey body code — see `foodsurveybody` / `feedsurveybody` reference data |
| `surveyReference` | string | Survey reference number — required when `surveyBodyCode` is provided |

### Sample Detail

| Field | Type | Notes |
| :---- | :--- | :---- |
| `detailDescription` | string | Description of the sample — required for food samples |
| `detailCategoryCode` | string | Level 4 category code, e.g. `"08.01.02.01"` — required for food samples; see `foodcategorylevel4` reference data |
| `detailBrandName` | string | Brand name |
| `detailNatureOfProductCode` | string | Nature of product code — see `foodnatureofproduct` reference data |
| `detailSampleTakenFromCode` | string | Where the sample was taken from — see `foodtakenfrom` reference data |
| `detailManufacturer` | string | Manufacturer name |
| `detailDistributor` | string | Distributor name |
| `detailImporter` | string | Importer name |
| `detailCountryOfOrigin` | string | Country of origin (full country name) |

### Animal Feed (ANIMAL FEED records only)

| Field | Type | Notes |
| :---- | :--- | :---- |
| `feedRegistrationNumber` | string | Feed registration number |
| `feedDateOfManufacture` | string | ISO 8601 date (e.g. `"2025-08-01"`) |
| `feedAnimalSpeciesCode` | string | Animal species code — see `feedanimalspecies` reference data |
| `feedLabelRegistrationNumber` | string | Label registration number |
| `feedLabelBusinessTypeCode` | string | Label business type code — see `feedlabelbusinesstype` reference data |
| `feedLabelBusinessAddress` | string | Label business address |

### Packaging

| Field | Type | Notes |
| :---- | :--- | :---- |
| `packagingProvidedCode` | string | **Required.** Packaging provided code — see `foodpackagingtype` reference data |
| `packagingMaterialCode` | string | **Required.** Packaging material code — see `packagingmaterial` reference data |
| `packagingOtherMaterialDetail` | string | Description if material code indicates "other" — required when `packagingMaterialCode = "O"` |
| `packagingPackQuantity` | number | Pack quantity (integer) |
| `packagingPackUnitsCode` | string | Pack units code — see `packagingunit` reference data |
| `packagingBatchNumber` | string | **Required.** Batch number |
| `packagingHealthMark` | string | Health mark |
| `packagingLabellingDetail` | string | Free-text labelling detail |

### Durability

| Field | Type | Notes |
| :---- | :--- | :---- |
| `durabilityCode` | string | **Required.** Durability type code — see `fooddurability` / `feeddurability` reference data |
| `durabilityDay` | number | Day component of durability date (integer). Use `0` if not applicable |
| `durabilityMonth` | number | Month component (integer). Use `0` if not applicable |
| `durabilityYear` | number | Year component (integer). Use `0` if not applicable |

### Condition

| Field | Type | Notes |
| :---- | :--- | :---- |
| `conditionCode` | string | **Required.** Condition code — see `foodcondition` / `feedcondition` reference data |
| `conditionOtherDetail` | string | Free-text description when `conditionCode` indicates "other" |
| `conditionTemperatureWhenTaken` | number | Temperature at time of sampling (decimal, °C). **Must be numeric — not a string.** When `conditionCode = "F"` (frozen), value must be below 0. Send `null` if not measured |

### Code of Practice

| Field | Type | Notes |
| :---- | :--- | :---- |
| `codeOfPracticeCompliant` | boolean | **Required.** Whether sample complies with code of practice |
| `codeOfPracticeNotCompliantDetail` | string | Reason for non-compliance — required when `codeOfPracticeCompliant = false` |
| `codeOfPracticeTransportCompliant` | boolean | **Required.** Whether transport complies with code of practice |

---

## Migration Guide — Previous API Field Names

If you are migrating from an earlier version of this API, the following table maps the old field names to the current ones.

The main changes are: an `fs` prefix was added to record-level fields; the `food` prefix was removed from detail/packaging/durability/condition fields to allow the same fields to work for both food and feed; and two separate date+time fields were merged into a single datetime.

| Old Field Name | Current Field Name | Notes |
| :------------- | :----------------- | :---- |
| `recordTypeCode` | `fsRecordTypeCode` | Valid values unchanged: `"FOOD"` or `"ANIMAL FEED"` |
| `analysisTypeCode` | `fsAnalysisTypeCode` | Value changed: use `"C"` or `"M"` — `"chemical"` and `"microbiology"` are no longer accepted |
| `nationalReference` | `fsReference` | |
| `statusCode` | `fsStatusCode` | |
| `additionalInformation` | `fsAdditionalInformation` | |
| `dateSampleTaken` + `timeSampleTaken` | `authorityDateTimeSampleTaken` | Merge into one field: `"2025-10-15T10:30:00"`. Use `T03:00:00` if time is unknown |
| `purchaseCost` | `authorityPurchaseCost` | |
| `analysisCost` | `authorityAnalysisCost` | |
| `routineAnalysis` | `laboratoryRoutineAnalysisRequired` | |
| `analysisRequired` | `laboratoryAnalysisRequiredDetail` | |
| `premisesName` | `premisesTradingName` | |
| `reasonSurveyBodyCode` | `surveyBodyCode` | |
| `reasonSurveyReference` | `surveyReference` | |
| `reasonIsFoodBorneIllness` + `reasonFoodBorneIllnessDetails` | `reasonFoodborneIllnessDetail` | Merged into a single descriptive field. If details are empty but the flag was set, use `"Yes"` |
| `foodDetailFoodDescription` | `detailDescription` | |
| `foodDetailFoodCategoryCode` | `detailCategoryCode` | |
| `foodDetailBrandName` | `detailBrandName` | |
| `foodDetailNatureOfProduct` | `detailNatureOfProductCode` | |
| `foodDetailSampleTakenFrom` | `detailSampleTakenFromCode` | Field name now has `Code` suffix — validated against reference data |
| `foodDetailManufacturer` | `detailManufacturer` | |
| `foodDetailDistributor` | `detailDistributor` | |
| `foodDetailImporter` | `detailImporter` | |
| `foodDetailCountryOfOrigin` | `detailCountryOfOrigin` | |
| `foodDetailPackagingProvidedCode` | `packagingProvidedCode` | |
| `foodDetailPackagingMaterialCode` | `packagingMaterialCode` | |
| `foodDetailOtherMaterial` | `packagingOtherMaterialDetail` | |
| `foodDetailPackQuantity` | `packagingPackQuantity` | |
| `foodDetailPackUnitsCode` | `packagingPackUnitsCode` | |
| `foodDetailBatchNumber` | `packagingBatchNumber` | |
| `foodDetailHealthMark` | `packagingHealthMark` | |
| `foodDetailDurabilityCode` | `durabilityCode` | |
| `foodDetailDurabilityDay` | `durabilityDay` | |
| `foodDetailDurabilityMonth` | `durabilityMonth` | |
| `foodDetailDurabilityYear` | `durabilityYear` | |
| `foodDetailConditionCode` | `conditionCode` | |
| `foodDetailOtherCondition` | `conditionOtherDetail` | |
| `foodDetailTemperature` | `conditionTemperatureWhenTaken` | Numeric only — not a string. Must be below 0 when condition is frozen |
| `copCompliant` | `codeOfPracticeCompliant` | |
| `copExplanation` | `codeOfPracticeNotCompliantDetail` | |
| `copTransportCompliant` | `codeOfPracticeTransportCompliant` | |

---

## Desktop Client Field Mapping

If you are migrating from the UKFSS Desktop application (FSSNet), the following maps the Desktop XML field names to the API field names.

| Desktop Field | API Field |
| :------------ | :-------- |
| RECORD\_TYPE | `fsRecordTypeCode` |
| ANALYSIS\_TYPE | `fsAnalysisTypeCode` |
| CODE | `fsReference` |
| SAMPLE\_STATUS\_\_CODE | `fsStatusCode` |
| ADDITIONAL\_INFO | `fsAdditionalInformation` |
| SAMPLE\_TIMESTAMP | `authorityDateTimeSampleTaken` |
| LOCAL\_AUTHORITY\_\_CODE | `authorityCode` |
| LOCAL\_AUTH\_OFFICE\_\_CODE | `authorityOfficeCode` |
| LA\_SAMPLENO | `authorityReference` |
| SAMPLE\_OFFICER\_\_CODE | `authoritySamplingOfficerCode` |
| SAMPLE\_OFFICER\_NAME | `authoritySamplingOfficerName` |
| SAMPLE\_OFFICER\_EMAIL\_ADDRESS | `authoritySamplingOfficerEmail` |
| SAMPLE\_COMMENTS | `authorityComments` |
| PURCHASE\_COST | `authorityPurchaseCost` |
| ANALYSIS\_COST | `authorityAnalysisCost` |
| LABORATORY\_\_CODE | `laboratoryCode` |
| ANALYSIS\_TEXT | `laboratoryAnalysisRequiredDetail` |
| BUSINESS\_ID | `premisesBusinessId` |
| PREMISES\_NAME | `premisesTradingName` |
| BUSINESS\_ADDR\_1 | `premisesAddressLine1` |
| BUSINESS\_ADDR\_2 | `premisesAddressLine2` |
| BUSINESS\_ADDR\_3 | `premisesAddressLine3` |
| BUSINESS\_ADDR\_4 | `premisesAddressLine4` |
| BUSINESS\_POSTCODE | `premisesPostcode` |
| RISK\_CATEGORY | `premisesFoodHygieneRiskRating` |
| RISK\_CATEGORY\_FOOD\_STAND\_\_CODE | `premisesFoodStandardsRiskRating` |
| PREMISES\_TYPE\_\_CODE | `premisesTypeCode` |
| REASON\_\_CODE | `reasonCode` |
| SAMPLE\_TYPE\_\_CODE | `reasonTypeCode` |
| INDEX\_NUMBER | `reasonFollowupSampleReference` |
| DETAILS | `reasonFoodborneIllnessDetail` |
| SURVEY\_BODY\_\_CODE | `surveyBodyCode` |
| SURVEY\_NUMBER | `surveyReference` |
| CATEGORY\_\_CODE | `detailCategoryCode` |
| FOOD\_DESCRIPTION | `detailDescription` |
| BRAND\_NAME | `detailBrandName` |
| PRODUCT\_\_CODE | `detailNatureOfProductCode` |
| SAMPLE\_TAKEN\_FROM | `detailSampleTakenFromCode` |
| MANUFACTURER | `detailManufacturer` |
| DISTRIBUTER | `detailDistributor` |
| IMPORTER | `detailImporter` |
| COUNTRY | `detailCountryOfOrigin` |
| REGISTRATION\_NUMBER | `feedRegistrationNumber` |
| DATE\_OF\_MANUFACTURE | `feedDateOfManufacture` |
| ANIMAL\_SPECIES\_\_CODE | `feedAnimalSpeciesCode` |
| LABEL\_REGISTRATION\_NUMBER | `feedLabelRegistrationNumber` |
| LABEL\_BUSINESS\_TYPE\_\_CODE | `feedLabelBusinessTypeCode` |
| LABEL\_BUSINESS\_ADDRESS | `feedLabelBusinessAddress` |
| PACKAGING\_\_CODE | `packagingProvidedCode` |
| MATERIAL\_\_CODE | `packagingMaterialCode` |
| OTHER\_MATERIAL | `packagingOtherMaterialDetail` |
| QUANTITY | `packagingPackQuantity` |
| PACK\_SIZE | `packagingPackUnitsCode` |
| BATCH\_NUMBER | `packagingBatchNumber` |
| HEALTH\_MARK | `packagingHealthMark` |
| LABELLING\_DETAILS | `packagingLabellingDetail` |
| DURABILITY\_\_CODE | `durabilityCode` |
| DURABILITY\_DAY | `durabilityDay` |
| DURABILITY\_MONTH | `durabilityMonth` |
| DURABILITY\_YEAR | `durabilityYear` |
| CONDITION\_\_CODE | `conditionCode` |
| OTHER\_CONDITION | `conditionOtherDetail` |
| TEMPERATURE | `conditionTemperatureWhenTaken` |
| FLAG\_COP\_CONDITION | `codeOfPracticeCompliant` |
| NOT\_COP | `codeOfPracticeNotCompliantDetail` |
| FLAG\_COP\_LAB | `codeOfPracticeTransportCompliant` |

For further background on expected values, see the [UKFSS User Guide](https://docs.ukfss.org.uk/desktop-user-guide-v9/html/index.html) — specifically the Food and Feed sample entry sections.

---

## Constraints & Validation

- `fsRecordTypeCode` must be `"FOOD"` or `"ANIMAL FEED"` (with space) — `"ANIMAL_FEED"` (underscore) and `"FEED"` are not valid
- `fsAnalysisTypeCode` must be `"C"` or `"M"` — the long-form strings `"chemical"` and `"microbiology"` are not accepted
- `authorityDateTimeSampleTaken` must be ISO 8601: `"YYYY-MM-DDTHH:MM"` or `"YYYY-MM-DDTHH:MM:SS"` — seconds are optional
- Empty fields must be `null`, not empty strings
- `conditionTemperatureWhenTaken` is a decimal number — do not send string values such as `"Frozen"` or `"Ambient"`. When `conditionCode = "F"` (frozen), the value must be below 0
- Boolean fields must be `true`, `false`, or `null` — not strings
- `fsReference` and `fsStatusCode` are assigned by UKFSS — omit them for new samples
- `authorityCode` is required in the request body — missing or mismatched values return `403` before sample validation runs
- HTTP 200 indicates the request was received. Check the `success` field in the response body to determine whether the sample passed validation

---

[← Reference Endpoints](reference-endpoints.md)
