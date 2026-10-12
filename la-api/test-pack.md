# Test Pack

[← Back to overview](README.md)

---

A ready-made set of requests for checking your integration against the **Test** environment. It covers the two ways a sample can reach the laboratory from your system:

- **A. Complete samples** are saved through the API and sent to the laboratory through the API, with no Portal step.
- **B. Incomplete samples** are saved through the API as drafts. An officer completes them in the UKFSS Portal and sends them to the laboratory from there.

Folder C is optional and covers correcting a draft through the API instead of the Portal, plus a few edge cases. Folder D checks your samples once test results have been returned.

**Download:** [`postman/ukfss-la-api-test-pack.postman_collection.json`](postman/ukfss-la-api-test-pack.postman_collection.json). It imports into Postman, or into Bruno, which imports Postman collections.

---

## Before you start

1. Import the collection.
2. Open the collection's variables and set `apiKey` to your **Test** API key. Then replace the placeholder values with your own:

   | Variable | Placeholder | Set to |
   | :------- | :---------- | :----- |
   | `baseUrl` | `https://test.ukfss.org.uk` | Leave as is |
   | `authorityCode` | `999` | Your local authority code |
   | `officeCode` | `999HQ` | One of your offices, in full: authority code followed by the office code |
   | `officerCode`, `officerName`, `officerEmail` | `SO123`, `J. Smith` | A sampling officer of yours |
   | `labCode` | `PATST` | Leave as is |

3. Run the folders in order. Each request uses values saved by the one before it, so run a folder from the top.

`PATST` is the UKFSS Test Laboratory, which we recommend for testing. Test samples sent to other laboratory codes on the Test environment are not passed to the real laboratory. The collection is for the Test environment only and should never be pointed at production.

The sample date is set to one hour ago on every run, and each run uses a new LA sample number starting `APITEST-`, so the test samples are easy to find in the Portal.

---

## How UKFSS treats a submission

| What you send | `success` | Status | What happens next |
| :------------ | :-------- | :----- | :---------------- |
| Every required field, all codes valid | `true` | `VALIDATED` | Listed by `candidate-samples` and can be sent with `submit-to-lab` |
| Fields missing or codes invalid | `false` | `ENTERED` (draft) | Saved, with the problems listed in `errors`. Can't be sent through the API. Shows in the Portal under Sample Browser > **All drafts** |
| A request UKFSS can't process (e.g. malformed JSON) | `false` | none | Nothing is saved |

A `200` response means the request was received. Always check `success` and `errors`.

---

## A. Complete sample, sent through the API

| Step | Request | Expected |
| :--- | :------ | :------- |
| A1 | Save a complete sample | `success: true`, status `VALIDATED`, `fsId` and `fsReference` returned |
| A2 | Get single record | `VALIDATED` |
| A3 | `candidate-samples` | The sample is listed |
| A4 | `submit-to-lab` | `200`, `sampleCount: 1` |
| A5 | Get single record | `EXPORTED` |
| A6 | `submit-to-lab` again | `409`. A sample can only be sent once, and nothing is exported |

## B. Incomplete sample, corrected and sent in the Portal

| Step | Request or action | Expected |
| :--- | :---------------- | :------- |
| B1 | Save a sample with no premises type, batch number or food category | `success: false`, status `ENTERED`, an error for each missing field |
| B2 | Get single record | `ENTERED` |
| B3 | `candidate-samples` | The sample is **not** listed |
| B4 | `submit-to-lab` | `409`. Drafts can't be sent through the API |
| Portal | Sample Browser > **All drafts**: open the `APITEST-` sample, fill in Premises type, Batch number and Food category, and save | The sample moves to **Ready to send** |
| B5 | Get single record | `VALIDATED` |
| Portal | Sample Browser > **Ready to send**: tick the sample, click **Export to lab** | The sample moves to **Sent** |
| B6 | Get single record | `EXPORTED` |

B5 and B6 show that your system can see what happened in the Portal by reading the record back with `get-single-record`.

## C. Optional: correcting through the API, and edge cases

| Step | Request | Expected |
| :--- | :------ | :------- |
| C1 | Save an incomplete sample | `ENTERED`, as B1 |
| C2 | Re-send the full sample with its `fsId` and `fsReference` | `success: true`, `VALIDATED`, same `fsReference` (the same record is updated) |
| C3 | `submit-to-lab` with the wrong `laboratoryCode` | `409`. The laboratory in the request must match the laboratory on the sample |
| C4 | `submit-to-lab` | `200` |
| C5 | Edit the sample sent in A4, leaving the batch number empty | `success: false`, error for `packagingBatchNumber`. The edit isn't saved. The response's `data` shows what you sent, not the stored record, so read the sample back to see its current state |
| C6 | Get single record for the A4 sample | Still sent (`EXPORTED`, or `COMPLETE` if results are back), batch number unchanged |

## D. Results from the Test Laboratory

`PATST` doesn't analyse anything. When you'd like results back, contact MacLaren West support with the UKFSS sample numbers (`fsReference`) and whether each should pass or fail, and we'll return test results as the laboratory would.

| Step | Request or action | Expected |
| :--- | :---------------- | :------- |
| D1 | Get single record for the folder A sample | `COMPLETE` |
| D2 | Get single record for the folder B sample | `COMPLETE` |
| Portal | Sample Management, or the Portal's MIS Export | The test results are shown and are included in the export |

Results usually show within a few minutes of being returned. The API reports the status only; the result values are available in the Portal.

---

## Points to note

- **Sample date.** `authorityDateTimeSampleTaken` must be within the last 30 days, or up to 7 days ahead.
- **Updates.** Include `fsReference` (and `fsId`, if you have it) to update a sample. Without `fsReference`, a new sample is created.
- **Status.** Never send `fsStatusCode`. UKFSS sets it.
- **Sending a batch.** `submit-to-lab` is all or nothing: if any sample in the request isn't ready to send, nothing is sent and the response lists which samples failed and why.
- **Laboratory on the sample.** The `laboratoryCode` on the sample decides where it can be sent. To change laboratory, update the sample first.
- **Valid codes.** Use `get-reference-data` for the current code lists rather than hard-coding them. See [Reference Endpoints](reference-endpoints.md).

---

[← Appendix](appendix.md) | [Back to overview →](README.md)
