# Getting Started

[← Back to overview](README.md)

---

## Environments & Base URL

| Environment | Base URL |
| :---------- | :------- |
| Test | `https://test.ukfss.org.uk` |
| Live | `https://api.ukfss.org.uk` |

Test and production use **separate API keys**.

---

## Authentication

All API requests must include an API key issued to your local authority.

The preferred header is the standard HTTP `Authorization` header:

```txt
Authorization: Bearer {api-key}
```

The legacy `api-key` header is also supported for backwards compatibility:

```txt
api-key: {api-key}
```

API keys are **environment-specific** and cannot be shared between test and production.

### IP Address Allowlists

API keys can be restricted to specific IP addresses or ranges using CIDR notation. If an allowlist is configured for your key, requests from addresses not on the list will be rejected with a `401` response.

- If no allowlist is configured, **all source IPs are permitted** (default behaviour)
- Single IPs use `/32` notation: `203.0.113.45/32`
- Ranges use standard CIDR notation: `10.0.0.0/8`
- Multiple CIDR entries can be added to a single API key

Contact MacLaren West support to request an allowlist be added or updated for your API key.

### Authorisation

Each API key is scoped to one or more local authorities. The `authorityCode` in your request payload must match an authority your key is authorised for — submitting on behalf of a different authority will be rejected with a `403` response, even if the key itself is valid.

Your key's authorised authorities are set up by MacLaren West when the key is issued. Contact MacLaren West support if you receive unexpected `403` responses or need your key authorised for additional authorities.

---

## API Versioning

All endpoints are available in multiple forms — all work today:

| Form | Example | Status |
| :--- | :------ | :----- |
| Versioned, kebab-case | `/api/v1/sample-entry/get-single-record` | **Recommended** — stable long-term |
| Versioned, PascalCase | `/api/v1/SampleEntry/GetSingleRecord` | Supported — may be deprecated in a future version |
| Unversioned, PascalCase | `/api/SampleEntry/GetSingleRecord` | Supported — may be deprecated in a future version |

New integrations should use the versioned kebab-case form. If a breaking change is introduced it will ship as `/api/v2/sample-entry` rather than modifying existing endpoints. Deprecations will be communicated in advance.

---

## Rate Limiting

Requests are rate-limited per API key. If you exceed the limit you will receive a `429 Too Many Requests` response. Contact MacLaren West support if your integration needs a higher limit.

---

## High-Level Workflow

```text
1. Look up reference data to validate codes (optional, can be done offline)
2. Submit a sample record
3. UKFSS assigns an fsId and processes the record
4. Retrieve the sample at any time using its fsId
```

---

[Next: Core Workflow →](core-workflow.md)
