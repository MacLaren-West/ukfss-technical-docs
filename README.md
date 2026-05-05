# UKFSS Laboratory API

The UKFSS Laboratory API provides a **queue-based workflow** for laboratories to retrieve samples, confirm their status, and upload processed laboratory results.

The API supports the following operations:

1. **Download pending samples** assigned to the laboratory
2. **Accept, reject, or reset** each sample
3. **Upload laboratory results** for accepted samples
4. **Retrieve an individual sample** by ID or reference
5. **Look up reference data** for determinations and outcome codes

Each sample is uniquely identified by an `fsId`, which is used consistently across all API operations.

---

## Documentation

| Section | Description |
| :------ | :---------- |
| [Getting Started](getting-started.md) | Environments, authentication, and high-level workflow |
| [Core Workflow](core-workflow.md) | Download pending samples, set sample status, submit results |
| [Reference Endpoints](reference-endpoints.md) | Look up samples, determinations, outcomes, and reference data |
| [Appendix](appendix.md) | Data model, field mappings, constraints, and client field mapping |
