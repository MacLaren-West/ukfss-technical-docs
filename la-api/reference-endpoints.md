# Reference Endpoints

[← Back to overview](README.md)

---

## Reference Data

Returns valid codes for a named reference list. Use this to validate field values before submission and to keep your local copy of reference data in sync with the UKFSS database.

### Endpoint

```http
GET /api/v1/sample-entry/get-reference-data?dataType={dataType}   # recommended
GET /api/v1/SampleEntry/GetReferenceData?dataType={dataType}       # supported
GET /api/SampleEntry/GetReferenceData?dataType={dataType}          # supported
```

The `dataType` parameter is **case-insensitive**.

### Response Shape

Most reference lists return an array of objects with two fields:

```json
[
  { "value": "RETAIL", "label": "Retail outlet" },
  { "value": "CATERING", "label": "Catering premises" }
]
```

The `value` field is the code to use in your sample payload. The `label` is the human-readable display name. Always use `value` when submitting samples — do not use `label`.

`foodCategoryLevel4` and `feedAnimalSpecies` have different shapes — see below.

---

### Supported `dataType` Values

#### Food

| `dataType` | Description |
| :--------- | :---------- |
| `foodPremisesType` | Premises type codes (`premisesTypeCode`) for food samples |
| `foodDurability` | Durability codes (`durabilityCode`) for food |
| `foodCondition` | Condition codes (`conditionCode`) for food samples |
| `foodPackagingType` | Packaging provided codes (`packagingProvidedCode`) for food |
| `packagingMaterial` | Packaging material codes (`packagingMaterialCode`) — applies to both food and feed |
| `foodNatureOfProduct` | Nature of product codes (`detailNatureOfProductCode`) |
| `foodReasonTaken` | Reason codes (`reasonCode`) for food samples |
| `foodReasonType` | Reason type codes (`reasonTypeCode`) for food samples |
| `foodSurveyBody` | Survey body codes (`surveyBodyCode`) for food surveillance samples |
| `foodHygieneRiskCategory` | Food hygiene risk category codes (`premisesFoodHygieneRiskRating`) |
| `foodStandardsRiskCategory` | Food standards risk category codes (`premisesFoodStandardsRiskRating`) |
| `foodCategoryLevel4` | Food category hierarchy — all categories to level 4 (see below) |
| `foodTakenFrom` | Where-taken-from codes (`detailSampleTakenFromCode`) — food samples only |
| `packagingUnit` | Pack units codes (`packagingPackUnitsCode`) — applies to both food and feed |

#### Animal Feed

| `dataType` | Description |
| :--------- | :---------- |
| `feedPremisesType` | Premises type codes for feed samples |
| `feedDurability` | Durability codes for animal feed |
| `feedCondition` | Condition codes for animal feed samples |
| `feedLabelBusinessType` | Label business type codes (`feedLabelBusinessTypeCode`) |
| `feedReasonTaken` | Reason codes (`reasonCode`) for animal feed samples |
| `feedReasonType` | Reason type codes (`reasonTypeCode`) for animal feed samples |
| `feedSurveyBody` | Survey body codes for feed surveillance samples |
| `feedAnimalSpecies` | Animal species codes (`feedAnimalSpeciesCode`) — see below |

---

### Example Requests

```bash
# Premises types for food samples
curl --location 'https://test.ukfss.org.uk/api/v1/sample-entry/get-reference-data?dataType=foodPremisesType' \
--header 'api-key: {api-key}'

# Packaging material codes (food and feed)
curl --location 'https://test.ukfss.org.uk/api/v1/sample-entry/get-reference-data?dataType=packagingMaterial' \
--header 'api-key: {api-key}'

# Animal feed reason types
curl --location 'https://test.ukfss.org.uk/api/v1/sample-entry/get-reference-data?dataType=feedReasonType' \
--header 'api-key: {api-key}'
```

---

### `foodCategoryLevel4` — Food Category Hierarchy

This list returns the full food category tree to level 4. Each entry includes all four levels so you can display a breadcrumb or filter by higher-level groupings.

```bash
curl --location 'https://test.ukfss.org.uk/api/v1/sample-entry/get-reference-data?dataType=foodCategoryLevel4' \
--header 'api-key: {api-key}'
```

**Example item:**

```json
{
  "codelevel4": "15.01.01.99",
  "textlevel4": "Other Nuts",
  "codelevel3": "15.01.01",
  "textlevel3": "Nuts",
  "codelevel2": "15.01",
  "textlevel2": "Nuts and Nut Products, Snacks",
  "codelevel1": "15",
  "textlevel1": "Nuts and Nut Products, Snacks"
}
```

Use `codelevel4` as the value for `detailCategoryCode` in your sample payload. For further background on the category structure, see the [UKFSS Category Tree guidance](https://docs.ukfss.org.uk/category-tree-guidance/html/index.html).

---

### `feedAnimalSpecies` — Animal Species

The animal species list returns objects with a `value` code and a `label` taken from the `Description` column of the source table:

```json
[
  { "value": "09.01", "label": "Poultry" },
  { "value": "09.02", "label": "Cattle" }
]
```

Use `value` for `feedAnimalSpeciesCode` in your payload.

---

### Reference Data Stability

Reference data reflects the live UKFSS database and may change if codes are added, retired, or corrected. Major structural changes (e.g. a code being retired) will be communicated in advance. For routine updates (spelling corrections, new entries) the data is updated in place — we recommend re-fetching periodically rather than caching indefinitely.

---

[← Core Workflow](core-workflow.md) | [Next: Appendix →](appendix.md)
