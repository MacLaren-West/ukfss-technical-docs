# Core Workflow

[← Back to overview](README.md)

---

## 1. Download Pending Samples

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
*(2 of 10 records shown, test environment only)*

```json
{
  "samples": [
    {
      "fsId": 100001,
      "fsRecordTypeCode": "FOOD",
      "fsAnalysisTypeCode": "C",
      "fsReference": "99900000001",
      "fsStatusCode": "EXPORTED",
      ...
    },
    {
      "fsId": 100002,
      "fsRecordTypeCode": "FOOD",
      "fsAnalysisTypeCode": "C",
      "fsReference": "99900000002",
      "fsStatusCode": "EXPORTED",
      ...
    }
  ]
}
```

### Pending Queue Definition

The pending queue contains only samples that have **not yet been accepted, rejected, or analysed**.

- Accepted samples do not appear
- Cancelled (rejected) samples do not appear
- Analysed samples do not appear

---

## 2. Set Sample Status

### Endpoint

```http
POST /api/lab/set-lab-sample-status
```

### Request Body

| Field | Type | Required | Description |
|------|------|----------|-------------|
| fsId | integer | Yes | Sample identifier |
| labStatus | string | Yes | `accept`, `reject`, or `pending` |
| labStatusDescription | string | Conditional | Required if rejecting |

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

#### Reset to Pending Example

```json
{
  "fsId": 488667,
  "labStatus": "pending",
  "labStatusDescription": "Accepted in error, returning to queue"
}
```

### Status Rules

- Statuses control visibility in the pending queue — the system does not reject results based on sample status
- **Accept** removes the sample from the pending queue
- **Reject** marks the sample as cancelled and removes it from the pending queue; there is no separate cancel operation
- **Reset to pending** returns an accepted or rejected sample to the queue, provided no results have been submitted
- Once results exist, the status cannot be changed

---

## 3. Submit Sample Laboratory Results

### Endpoint

```http
POST /api/lab/submit-sample-lab-results
```

### Submission Rules

- Results can be submitted regardless of the sample's accept/reject status
- Submitting results changes the sample's status to **analysis complete** and removes it from the pending queue
- Each sample is matched on `fsReference`. `fsId` is optional and isn't used to find the sample; the response reports the matched sample's `fsId`
- A sample is **rejected** if its `fsReference` is missing or matches no sample, or if the sample isn't assigned to your laboratory
- A missing value for any other **Required** or **Conditional** sample or result field in the [field mappings](appendix.md#data-model--field-mappings) produces a **warning**, and the sample is still accepted. If your laboratory uses **strict validation**, the sample is rejected instead. Contact MacLaren West support to change your laboratory's validation mode.

### Example Payload

```json
{
  "samples": [
    {
      "fsReference": "FS-24-000123",
      "authorityCode": "999",
      "authorityOfficeCode": "999HQ",
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

### Example Response

For the payload above with `laboratoryComments` left out:

```json
{
  "success": true,
  "data": [
    {
      "sampleIndex": 0,
      "fsId": 100001,
      "success": true,
      "messages": [
        { "message": "laboratoryComments is required", "severity": "warning" }
      ]
    }
  ]
}
```

- `data` has one entry per submitted sample. `sampleIndex` is the sample's position in the `samples` array, starting at 0
- The top-level `success` is `true` only when every sample in the request was accepted
- `messages` is `null` when there is nothing to report. Otherwise each message has a `severity`: `error` means the sample was rejected, `warning` means it was accepted
- If your laboratory uses strict validation, messages have no `severity` field. Every message is an error, so `messages` is `null` whenever a sample is accepted

---

[← Getting Started](getting-started.md) | [Next: Reference Endpoints →](reference-endpoints.md)
