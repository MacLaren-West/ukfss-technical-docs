# Reference Endpoints

[← Back to overview](README.md)

---

## Get Individual Sample

Retrieve a single sample by `fsId` or `fsReference`. Useful for checking the current status of a specific sample without downloading the full pending queue.

### Endpoint

```http
GET /api/lab/sample?fsId={fsId}
GET /api/lab/sample?fsReference={fsReference}
```

One of `fsId` or `fsReference` must be provided. Results are scoped to laboratories associated with the calling API key.

### Example Request

```bash
curl --location 'https://test.ukfss.org.uk/api/lab/sample?fsId=100001' \
--header 'api-key: {api-key}'
```

---

## Determinations

Returns the full list of determination codes, including test substance and units. Results can be filtered by sample type.

### Endpoint

```http
GET /api/lab/get-determinations
```

### Query Parameters

| Parameter | Type | Required | Description |
| :-------- | :--- | :------- | :---------- |
| `forFood` | boolean | No | If `true`, returns only determinations applicable to food samples |
| `forFeed` | boolean | No | If `true`, returns only determinations applicable to animal feed samples |

Both parameters can be combined. If neither is supplied, all determinations are returned.

### Example Requests

```bash
# All determinations
curl --location 'https://test.ukfss.org.uk/api/lab/get-determinations' \
--header 'api-key: {api-key}'

# Food only
curl --location 'https://test.ukfss.org.uk/api/lab/get-determinations?forFood=true' \
--header 'api-key: {api-key}'

# Animal feed only
curl --location 'https://test.ukfss.org.uk/api/lab/get-determinations?forFeed=true' \
--header 'api-key: {api-key}'
```

The response is the live reference data from the UKFSS database. For background on the determination structure and a sample extract, see the [Generic LIMS documentation](https://docs.ukfss.org.uk/generic-lims/html/generic-lims-and-fss.html#appendix-vi-lab-determinations-extract).

---

## Outcome Codes

Returns the full list of outcome codes used in result and sample-level outcomes. Results can be filtered by sample type.

### Endpoint

```http
GET /api/lab/get-outcomes
```

### Query Parameters

| Parameter | Type | Required | Description |
| :-------- | :--- | :------- | :---------- |
| `forFood` | boolean | No | If `true`, returns only outcomes applicable to food samples |
| `forFeed` | boolean | No | If `true`, returns only outcomes applicable to animal feed samples |

Both parameters can be combined. If neither is supplied, all outcomes are returned.

### Example Requests

```bash
# All outcomes
curl --location 'https://test.ukfss.org.uk/api/lab/get-outcomes' \
--header 'api-key: {api-key}'

# Food only
curl --location 'https://test.ukfss.org.uk/api/lab/get-outcomes?forFood=true' \
--header 'api-key: {api-key}'

# Animal feed only
curl --location 'https://test.ukfss.org.uk/api/lab/get-outcomes?forFeed=true' \
--header 'api-key: {api-key}'
```

The response is the live reference data from the UKFSS database. For background on the outcome code structure and the full fail code breakdown by category, see the [Generic LIMS documentation](https://docs.ukfss.org.uk/generic-lims/html/generic-lims-and-fss.html#appendix-vii-outcome-failcodes).

---

## Reference Data

```http
GET /api/SampleEntry/GetReferenceData?dataType={datatype}
```

### Supported `dataType` values

- `foodPremisesType`
- `feedPremisesType`
- `foodDurability`
- `feedDurability`
- `foodCondition`
- `feedCondition`
- `feedLabelBusinessType`
- `foodPackagingType`
- `foodNatureOfProduct`
- `foodReasonTaken`
- `feedReasonTaken`
- `foodSurveyBody`
- `feedSurveyBody`
- `foodHygieneRiskCategory`
- `foodStandardsRiskCategory`
- `foodCategoryLevel4`

---

[← Core Workflow](core-workflow.md) | [Next: Appendix →](appendix.md)
