# UKFSS Local Authority API

The UKFSS Local Authority API allows Local Authority information management systems to submit food and animal feed sample records directly to UKFSS, without requiring manual entry through the web portal.

The API supports the following operations:

1. **Submit samples** — push sample records from your system into UKFSS
2. **Retrieve a submitted sample** — look up a sample by its UKFSS record ID
3. **Look up reference data** — retrieve valid codes for food categories, premises types, conditions, and more

Each sample is uniquely identified by an `fsId` once created. The `authorityReference` field carries your own system's sample number throughout.

---

## Documentation

| Section | Description |
| :------ | :---------- |
| [Getting Started](getting-started.md) | Environments, authentication, and base URL |
| [Core Workflow](core-workflow.md) | Submit a sample and retrieve submitted records |
| [Reference Endpoints](reference-endpoints.md) | Look up valid codes and reference data |
| [Appendix](appendix.md) | Field reference, legacy mappings, and constraints |
