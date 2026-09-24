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
| `authorityOfficeCode` | string | The sampling office's full code: your authority code followed by the office's own code, e.g. `999HQ` for office `HQ` in authority `999`. Must be an office set up for your authority in UKFSS |
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
  "authorityCode": "999",
  "authorityOfficeCode": "999HQ",
  "authorityReference": "LA-2025-001234",
  "authorityDateTimeSampleTaken": "2025-10-15T10:30:00",
  "authoritySamplingOfficerCode": "SO123",
  "authoritySamplingOfficerName": "J. Smith",
  "authoritySamplingOfficerEmail": "j.smith@council.gov.uk",
  "authorityComments": "Sample taken from display cabinet",
  "authorityPurchaseCost": 4.99,
  "authorityAnalysisCost": null,
  "laboratoryCode": "PATST",
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
    "fsReference": "99900000001",
    "fsStatusCode": "ENTERED",
    ...
    "messages": [
      { "message": "Validation Complete" }
    ]
  }
}
```

#### `success: false` — request error

Returned when the request cannot be processed (e.g. a malformed body). No record is created or updated.

```json
{
  "success": false,
  "data": null,
  "errors": [
    {
      "field": null,
      "code": "save_error",
      "message": "An item with the same key has already been added. Key: fsRecordTypeCode (Parameter 'key')"
    }
  ]
}
```

#### `success: false` — saved but failed validation

The record is stored, but the `errors` array lists the problems that must be resolved before the sample can proceed to the laboratory. The `data` object contains the saved sample.

```json
{
  "success": false,
  "data": {
    "fsId": 98739,
    "fsReference": "VZTC",
    "fsStatusCode": "ENTERED",
    "..."
  },
  "errors": [
    {
      "field": "fsRecordTypeCode",
      "code": "required",
      "message": "fsRecordTypeCode is required"
    }
  ]
}
```

### Storing `fsId` and `fsReference`

On a successful submission the response includes two identifiers — **store both**:

| Field | Type | Use |
| :---- | :--- | :-- |
| `fsId` | integer | Internal UKFSS record identifier. Use for `GetSingleRecord` and include in re-submissions alongside `fsReference`. |
| `fsReference` | string | The UKFSS sample number — appears on paperwork and in the UKFSS portal. Read-only once assigned. Include in every re-submission. |

#### Updating an existing record

The presence of `fsReference` in the body signals that a submission is an update. Three combinations are supported:

| Submitted | Behaviour |
| :-------- | :-------- |
| `fsReference` only | Looked up by `fsReference`; record updated |
| `fsId` + `fsReference` | Both cross-checked — must belong to the same record |
| `fsId` only | Rejected — `fsReference` is required alongside `fsId` |
| Neither | New record created; `fsReference` assigned by UKFSS |

`fsReference` is assigned by UKFSS on creation and cannot be changed — including it in a re-submission identifies the record to update, it does not alter the reference.

---

### Submission Rules

- Include `fsReference` in every re-submission — its presence signals an update rather than a new record
- If `fsId` is also included, both values are cross-checked to confirm they belong to the same record
- `fsReference` is read-only — it identifies the record but cannot be changed by a submission
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

## 3. List Samples Ready for Export

Retrieve every `VALIDATED` sample for your authority that is awaiting export to a laboratory. Use this to build the list of `sampleReference` values for a submit-to-lab batch.

### Endpoint

```http
GET /api/v1/sample-export/candidate-samples?authorityCode={authorityCode}&personality={personality}
```

### Parameters

| Parameter | Type | Required | Description |
| :-------- | :--- | :------- | :---------- |
| `authorityCode` | string | Yes | Your local authority code — must match the LA associated with your API key |
| `personality` | string | Yes | `"FOOD"` or `"ANIMAL FEED"`. `"ALL"` is not yet available via this endpoint |

### Example Request

```bash
curl --location 'https://test.ukfss.org.uk/api/v1/sample-export/candidate-samples?authorityCode=999&personality=FOOD' \
--header 'api-key: {api-key}'
```

### Response

An array of candidate samples. Only samples currently in `VALIDATED` status are included — a sample already exported, or one still `ENTERED`, will not appear.

```json
[
  {
    "id": 100001,
    "reference": "99900000123",
    "laReference": "LA-2025-001234",
    "officerCode": "SO123",
    "officerName": "J. Smith",
    "officerEmail": "j.smith@council.gov.uk",
    "officeCode": "999HQ",
    "officeName": "Headquarters",
    "laboratoryCode": "PATST",
    "laboratoryName": "Example Laboratory",
    "premisesCode": "BUS-12345",
    "premises": "Fresh Foods Ltd",
    "foodDescription": "Pre-packed chicken slices",
    "analysisTypeCode": "C",
    "dateTaken": "2025-10-15T10:30:00"
  }
]
```

| Field | Notes |
| :---- | :---- |
| `reference` | The `fsReference` value — use this in `sampleReferences` when submitting to a lab |
| `laboratoryCode` | The lab this sample is currently assigned to. A submit-to-lab request's `laboratoryCode` must match this exactly, or the sample is rejected |
| `analysisTypeCode` | `"C"` (chemical) or `"M"` (microbiology) |

---

## 4. Submit a Batch to the Lab

Submit a batch of your authority's own `VALIDATED` samples to a laboratory — the external equivalent of clicking "Export" in the UKFSS portal.

**Whole-batch contract:** every referenced sample must currently be `VALIDATED`, belong to your authority, and be assigned to the laboratory you specify. If any one of them isn't, **nothing is exported** — the response lists exactly which references failed and why, so you can correct the list and re-submit. There is no partial success.

### Endpoint

```http
POST /api/v1/sample-export/submit-to-lab
```

### Request Body

| Field | Type | Required | Notes |
| :---- | :--- | :------- | :---- |
| `authorityCode` | string | Yes | Your local authority code |
| `personality` | string | Yes | `"FOOD"` or `"ANIMAL FEED"` |
| `laboratoryCode` | string | Yes | Must match the `laboratoryCode` already assigned to every sample in the batch |
| `sampleReferences` | array of string | Yes | The `fsReference` values to export — at least one required |
| `comment` | string | No | Free-text note. Appended to the lab's notification under its own heading — it does not replace the standard sample breakdown the lab receives |

### Example Request

```bash
curl --location --request POST 'https://test.ukfss.org.uk/api/v1/sample-export/submit-to-lab' \
--header 'api-key: {api-key}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "authorityCode": "999",
  "personality": "FOOD",
  "laboratoryCode": "PATST",
  "sampleReferences": ["99900000123", "99900000124"],
  "comment": "Second delivery today, cold chain intact"
}'
```

### Response

#### Success

```json
{
  "batchId": 1234,
  "sampleCount": 2,
  "sampleCodes": ["99900000123", "99900000124"]
}
```

`sampleCount` always equals the number of references sent — there is no partial success. `batchId` identifies this export in UKFSS's own records.

#### `409 Conflict` — one or more samples not eligible

```json
{
  "message": "One or more samples are not eligible for this batch. No samples were exported.",
  "failures": [
    {
      "sampleReference": "99900000125",
      "reason": "Not found, not VALIDATED, or not assigned to this laboratory."
    },
    {
      "sampleReference": "99900000123",
      "reason": "Listed more than once in this batch."
    }
  ]
}
```

A reference can fail for any of these reasons:

- It doesn't exist, isn't `VALIDATED`, or isn't assigned to the `laboratoryCode` you specified
- It's listed more than once in the same request
- It went stale between your last `candidate-samples` read and this submission (edited, unvalidated, re-routed to a different lab, or already exported by someone else)

Re-fetch `candidate-samples` and retry with a corrected list.

### Submission Rules

- Check `candidate-samples` first — every `sampleReference` must already be `VALIDATED` and assigned to the `laboratoryCode` you specify
- A duplicate reference in the same request is rejected, not silently de-duplicated
- The whole batch succeeds or the whole batch fails — there is no partial export
- `personality` must be `"FOOD"` or `"ANIMAL FEED"` (with space) — `"ALL"` is not yet available via this endpoint

---

[← Getting Started](getting-started.md) | [Next: Reference Endpoints →](reference-endpoints.md)
