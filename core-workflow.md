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

[← Getting Started](getting-started.md) | [Next: Reference Endpoints →](reference-endpoints.md)
