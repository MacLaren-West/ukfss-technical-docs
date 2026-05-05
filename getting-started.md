# Getting Started

[← Back to overview](README.md)

---

## Environments & Base URL

### Test Environment

```txt
https://test.ukfss.org.uk
```

- Test environment limits pending samples to **10 records**
- Production environment returns **all pending samples**
- Test and production use **separate API keys**

---

## Authentication

All API requests must include an API key issued to your laboratory.

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

---

## High-Level Workflow

```text
1. Retrieve pending samples
2. Accept, reject, or reset each sample
3. Upload laboratory results for accepted samples
4. Analysed samples leave the pending queue
```

### Behavioural Guarantees

- Samples **must be accepted before results are submitted**
- Rejected samples are **cancelled** and cannot be processed
- A rejected sample can be **reset to pending** to undo a rejection, provided no results have been submitted
- Processed (analysed) samples **cannot be rejected or reset**
- Analysed samples **do not appear** in the pending queue

---

[Next: Core Workflow →](core-workflow.md)
