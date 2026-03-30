# UKFSS Laboratory API

## 1. Overview

The UKFSS Laboratory API provides a **queue-based workflow** for laboratories to retrieve samples, confirm their status, and upload processed laboratory results.

The API supports three core operations:

1. **Download pending samples** assigned to the laboratory  
2. **Accept or reject** each sample  
3. **Upload laboratory results** for accepted samples  

Each sample is uniquely identified by an `fsId`, which is used consistently across all API operations.

---

## 2. Environments & Base URL

### Test Environment

```txt
https://test.ukfss.org.uk
```

- Test environment limits pending samples to **10 records**
- Production environment returns **all pending samples**
- Test and production use **separate API keys**

---

## 3. Authentication

All API requests must include an API key issued to your laboratory.

```txt
Header Name: api-key
Header Value: {api-key}
```

API keys are **environment-specific** and cannot be shared between test and production.

### IP Address Allowlists

API keys can be restricted to specific IP addresses or ranges using CIDR notation. If an allowlist is configured for your key, requests from addresses not on the list will be rejected with a `401` response.

- If no allowlist is configured, **all source IPs are permitted** (default behaviour)
- Single IPs use `/32` notation: `203.0.113.45/32`
- Ranges use standard CIDR notation: `10.0.0.0/8`
- Multiple CIDR entries can be added to a single API key

Contact MacLaren West support to request an allowlist be added or updated for your API key.

---

## 4. High-Level Workflow

```text
1. Retrieve pending samples
2. Accept or reject each sample
3. Upload laboratory results for accepted samples
4. Analysed samples leave the pending queue
```

### Behavioural Guarantees

- Samples **must be accepted before results are submitted**
- Rejected samples are **cancelled** and cannot be processed
- Processed (analysed) samples **cannot be rejected**
- Analysed samples **do not appear** in the pending queue

---

## 5. Download Pending Samples

### Endpoint

```http
GET /api/lab/pending-samples
```

### Example Request

```bash
curl --location 'https://test.ukfss.org.uk/api/lab/pending-samples' \
--header 'api-key: {api-key}'
```

### Example Response  
*(2 of 10 records shown — test environment only)*

```json
{
  "samples": [
    {
      "fsId": 488669,
      "fsRecordTypeCode": "FOOD",
      "fsAnalysisTypeCode": "C",
      "fsReference": "80401520144",
      "fsStatusCode": "EXPORTED",
      ...
    },
    {
      "fsId": 488668,
      "fsRecordTypeCode": "FOOD",
      "fsAnalysisTypeCode": "C",
      "fsReference": "80401520141",
      "fsStatusCode": "EXPORTED",
      ...
    }
  ]
}
```

### Pending Queue Definition

The pending samples queue contains **unprocessed and unaccepted samples only**.

- Accepted but unanalysed samples remain pending  
- Analysed samples do not appear  
- Cancelled (rejected) samples do not appear  

---

## 6. Confirm Sample Acceptance or Rejection

### Endpoint

```http
POST /api/samples/status
```

### Request Body

| Field | Type | Required | Description |
|------|------|----------|-------------|
| fsId | integer | Yes | Sample identifier |
| labStatus | string | Yes | `accept` or `reject` |
| labStatusDescription | string | Conditional | Required if rejected |

#### Accept Example

```json
{
  "fsId": 488667,
  "labStatus": "accept"
}
```

#### Reject Example

```json
{
  "fsId": 488667,
  "labStatus": "reject",
  "labStatusDescription": "Not enough sample material"
}
```

### Rejection Rules

- Rejected samples are marked as **cancelled**
- Cancelled samples require **reactivation** before reprocessing
- Samples **cannot be rejected once results exist**

---

## 7. Submit Sample Laboratory Results

### Endpoint

```http
POST /api/lab/submit-sample-lab-results
```

### Submission Rules

- Results may only be submitted for **accepted samples**
- Submitting results changes the sample's status to **analysis complete**

### Example Payload

```json
{
  "samples": [
    {
      "fsId": "11234",
      "fsReference": "FS-24-000123",
      "authorityCode": "AB12",
      "authorityOfficeCode": "AB12-01",
      "authorityReference": "LA-7788",
      "laboratoryComments": "No anomalies in intake.",
      "laboratoryReference": "LIMS-998877",
      "laboratoryIsSatisfactory": true,
      "laboratoryAnalysisStartDate": "2025-10-20",
      "laboratorySampleFailureCode": "NONE",
      "laboratoryAnalysisReportDate": "2025-10-22",
      "results": [
        {
          "laboratoryResultId": "R-1",
          "resultTimestamp": "2025-10-21T11:30:00Z",
          "determinationCode": "DET001",
          "testSubstance": "NaCl",
          "testUnits": "mg/kg",
          "resultTypeCode": "NUMERIC",
          "formattedOutput": "12.3 mg/kg",
          "numericResult": 12.3,
          "numericResultQualifier": "=",
          "outcomeCode": "10100"
        }
      ],
      "outcomes": [
        {
          "outcomeCode": "10200"
        }
      ]
    }
  ]
}
```

---

## 8. Data Model & Field Mappings

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

## 9. Reference Data

```http
GET /api/SampleEntry/GetReferenceData?dataType={datatype}
```

### Supported `dataType` values

- foodpremisestype
- feedpremisestype
- fooddurability
- feeddurability
- foodCondition
- feedCondition
- feedlabelbusinesstype
- foodpackagingtype
- foodnatureofproduct
- foodreasontaken
- feedreasontaken
- foodsurveybody
- feedsurveybody
- foodhygieneriskcategory
- foodstandardsriskcategory
- foodCategoryLevel4

---

## 10. Constraints & Validation

- All dates are UTC
- `fsId` is immutable
- Payloads must strictly match the defined schema
- Additional JSON properties are rejected
- Each result must include either `textResult` or `numericResult`

## 11. Client Field Mapping

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
