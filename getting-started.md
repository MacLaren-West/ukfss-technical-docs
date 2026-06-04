# Getting Started

[← Back to overview](README.md)

---

## Environments & Base URL

| Environment | Base URL |
| :---------- | :------- |
| Test | `https://test.ukfss.org.uk` |
| Live | `https://api.ukfss.org.uk` |

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
2. Optionally set sample status (accept, reject, or reset)
3. Upload laboratory results
```

Samples leave the pending queue when **accepted**, **rejected**, or **analysed**.

### Status Behaviour

Sample statuses exist to control visibility in the pending queue — the system does not reject results based on status.

- **Accept** — removes the sample from the pending queue; use to acknowledge receipt
- **Reject** — marks the sample as cancelled and removes it from the pending queue; use if the sample cannot be processed
- **Reset to pending** — returns a previously accepted or rejected sample to the queue
- A sample can only be reset if **no results have been submitted** for it
- Once results are submitted, the status cannot be changed

---

[Next: Core Workflow →](core-workflow.md)
