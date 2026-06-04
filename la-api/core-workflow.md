# Core Workflow

[← Back to overview](README.md)

---

## 1. Submit a Sample

### Endpoint

```http
POST /api/v1/sample-entry/save           # recommended
POST /api/v1/SampleEntry/Save            # supported
POST /api/SampleEntry/Save               # supported
```

### Request Body

The body is a single JSON object. Fields not listed in the required tables below are optional and may be omitted or sent as `null`.

---

#### Always Required

| Field | Type | Valid Values / Notes |
| :---- | :--- | :------------------- |
| `fsRecordTypeCode` | string | `"FOOD"` or `"ANIMAL FEED"` |
| `fsAnalysisTypeCode` | string | `"C"` (chemical) or `"M"` (microbiology) |
| `authorityCode` | string | Your local authority code — must match the LA associated with your API key. The request is rejected with `403` if this does not match |
| `authorityDateTimeSampleTaken` | string | ISO 8601 datetime, e.g. `"2025-10-15T10:30:00"` or `"2025-10-15T10:30"` |
| `authorityOfficeCode` | string | The sampling office code |
| `authoritySamplingOfficerCode` | string | Sampling officer code. GUID values are supported |
| `laboratoryCode` | string | Code of the assigned laboratory — must be a valid lab code |
| `laboratoryRoutineAnalysisRequired` | boolean | `true` or `false` |
| `laboratoryAnalysisRequiredDetail` | string | Free-text description of analysis required |
| `premisesBusinessId` | string | Premises business identifier |
| `premisesTradingName` | string | Trading name of the premises |
| `premisesTypeCode` | string | See `foodpremisestype` / `feedpremisestype` reference data |
| `reasonCode` | string | See `foodreasontaken` / `feedreasontaken` reference data |
| `reasonTypeCode` | string | See `foodreasontype` / `feedreasontype` reference data |
| `packagingProvidedCode` | string | See `foodpackagingtype` reference data |
| `packagingMaterialCode` | string | See `packagingmaterial` reference data |
| `packagingBatchNumber` | string | Batch number |
| `durabilityCode` | string | See `fooddurability` / `feeddurability` reference data |
| `conditionCode` | string | See `foodcondition` / `feedcondition` reference data |
| `codeOfPracticeCompliant` | boolean | `true` or `false` |
| `codeOfPracticeTransportCompliant` | boolean | `true` or `false` |

#### Conditionally Required

| Field | Type | Required When |
| :---- | :--- | :------------ |
| `detailDescription` | string | `fsRecordTypeCode = "FOOD"` |
| `detailCategoryCode` | string | `fsRecordTypeCode = "FOOD"` |
| `surveyReference` | string | `surveyBodyCode` is provided |
| `packagingOtherMaterialDetail` | string | `packagingMaterialCode = "O"` |
| `codeOfPracticeNotCompliantDetail` | string | `codeOfPracticeCompliant = false` |

---

#### Notes

> **`fsRecordTypeCode`:** Use `"ANIMAL FEED"` (with space). `"ANIMAL_FEED"` (with underscore) and `"FEED"` are not valid.

> **`fsAnalysisTypeCode`:** `"C"` = chemical analysis, `"M"` = microbiological analysis. Use the short codes only — the long-form strings `"chemical"` and `"microbiology"` are not accepted.

> **`authorityDateTimeSampleTaken`:** ISO 8601 format. Seconds are optional — both `"2025-10-15T10:30"` and `"2025-10-15T10:30:00"` are accepted.

> **`conditionTemperatureWhenTaken`:** Numeric field (decimal °C). Do not send string values such as `"Frozen"` — use `null` if the temperature is not measured. When `conditionCode = "F"` (frozen), the value must be below 0.

> **Empty fields:** Send `null`, not empty strings.

---

### Example Request

```bash
curl --location --request POST 'https://test.ukfss.org.uk/api/v1/sample-entry/save' \
--header 'api-key: {api-key}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "fsRecordTypeCode": "FOOD",
  "fsAnalysisTypeCode": "C",
  "fsReference": null,
  "fsStatusCode": null,
  "fsAdditionalInformation": null,
  "authorityCode": "806",
  "authorityOfficeCode": "806-01",
  "authorityReference": "LA-2025-001234",
  "authorityDateTimeSampleTaken": "2025-10-15T10:30:00",
  "authoritySamplingOfficerCode": "SO123",
  "authoritySamplingOfficerName": "J. Smith",
  "authoritySamplingOfficerEmail": "j.smith@council.gov.uk",
  "authorityComments": "Sample taken from display cabinet",
  "authorityPurchaseCost": 4.99,
  "authorityAnalysisCost": null,
  "laboratoryCode": "LAB001",
  "laboratoryRoutineAnalysisRequired": false,
  "laboratoryAnalysisRequiredDetail": "Chloramphenicol, Tetracycline",
  "premisesBusinessId": "BUS-12345",
  "premisesTradingName": "Fresh Foods Ltd",
  "premisesAddressLine1": "1 High Street",
  "premisesAddressLine2": null,
  "premisesAddressLine3": null,
  "premisesAddressLine4": null,
  "premisesPostcode": "AB1 2CD",
  "premisesFoodHygieneRiskRating": "A",
  "premisesFoodStandardsRiskRating": "A",
  "premisesTypeCode": "E",
  "reasonCode": "E",
  "reasonTypeCode": "R",
  "reasonFollowupSampleReference": null,
  "reasonFoodborneIllnessDetail": null,
  "surveyBodyCode": null,
  "surveyReference": null,
  "detailDescription": "Pre-packed chicken slices",
  "detailCategoryCode": "08.01.02.01",
  "detailBrandName": "Farm Fresh",
  "detailNatureOfProductCode": "R",
  "detailSampleTakenFromCode": null,
  "detailManufacturer": "N/A",
  "detailDistributor": "N/A",
  "detailImporter": "N/A",
  "detailCountryOfOrigin": "UNITED KINGDOM",
  "feedAnimalSpeciesCode": null,
  "packagingProvidedCode": "C",
  "packagingMaterialCode": "O",
  "packagingOtherMaterialDetail": "Carton",
  "packagingPackQuantity": null,
  "packagingPackUnitsCode": null,
  "packagingBatchNumber": "BATCH-001",
  "packagingHealthMark": null,
  "packagingLabellingDetail": null,
  "durabilityCode": "U",
  "durabilityDay": 20,
  "durabilityMonth": 10,
  "durabilityYear": 2025,
  "conditionCode": "O",
  "conditionOtherDetail": "Other",
  "conditionTemperatureWhenTaken": null,
  "codeOfPracticeCompliant": false,
  "codeOfPracticeNotCompliantDetail": "Port Health legislative requirements used",
  "codeOfPracticeTransportCompliant": false
}'
```

---

### Response

Every call to `Save` returns HTTP 200 with a JSON body. **HTTP 200 means the request was received — it does not mean the sample passed validation.**

The `success` field in the response body indicates whether the sample is valid:

```json
{
  "success": true | false,
  "data": {
    ...sample fields...,
    "messages": [
      { "message": "..." }
    ]
  }
}
```

#### `success: true` — valid and saved

```json
{
  "success": true,
  "data": {
    "fsId": 100001,
    "fsReference": "80100000001",
    "fsStatusCode": "ENTERED",
    ...
    "messages": [
      { "message": "Validation Complete" }
    ]
  }
}
```

#### `success: false` — saved but failed validation

The record is stored, but the `messages` array lists the problems that must be resolved before the sample can proceed to the laboratory.

```json
{
  "success": false,
  "data": {
    "fsId": 100002,
    "fsReference": null,
    ...
    "messages": [
      { "message": "fsRecordTypeCode is required" },
      { "message": "Validation Complete" }
    ]
  }
}
```

### Storing `fsId` and `fsReference`

On a successful submission the response includes two identifiers — **store both**:

| Field | Type | Use |
| :---- | :--- | :-- |
| `fsId` | integer | Use for API calls — pass to `GetSingleRecord` and any future endpoints |
| `fsReference` | string | The UKFSS sample number (e.g. `"80100000001"`) — appears on paperwork and in the UKFSS portal |

If `fsReference` is included in a subsequent submission it updates the existing record rather than creating a new one. If it is omitted, a new record is always created.

---

### Submission Rules

- Omit `fsReference` for new samples — it is assigned by UKFSS on creation
- Never set `fsStatusCode` — it is always assigned and managed by UKFSS. If a status transition is needed in future, a dedicated endpoint will be provided for that purpose
- `fsRecordTypeCode` must be `"FOOD"` or `"ANIMAL FEED"` (with space)
- `fsAnalysisTypeCode` must be `"C"` (chemical) or `"M"` (microbiology)
- Feed-specific fields (`feedAnimalSpeciesCode`, etc.) apply only to `"ANIMAL FEED"` samples
- `authorityCode` is required in the request body — the request is rejected with `403` if it is missing or does not match the LA associated with your API key

---

## 2. Retrieve a Sample

Retrieve a previously submitted sample by its `fsId`.

### Endpoint

```http
GET /api/v1/sample-entry/get-single-record?fsId={fsId}   # recommended
GET /api/v1/SampleEntry/GetSingleRecord?fsId={fsId}       # supported
GET /api/SampleEntry/GetSingleRecord?fsId={fsId}          # supported
```

### Parameters

| Parameter | Type | Required | Description |
| :-------- | :--- | :------- | :---------- |
| `fsId` | integer | Yes | The `fsId` returned in the `Save` response |

### Example Request

```bash
curl --location 'https://test.ukfss.org.uk/api/v1/sample-entry/get-single-record?fsId=100001' \
--header 'api-key: {api-key}'
```

---

[← Getting Started](getting-started.md) | [Next: Reference Endpoints →](reference-endpoints.md)
