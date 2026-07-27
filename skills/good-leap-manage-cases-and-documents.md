---
name: Clear GoodLeap loan cases and upload documents
description: Resolve loan stipulations (cases) and upload required documents via pre-signed URLs on a GoodLeap loan.
api: openapi/good-leap-developer-api-openapi.yml
operations: [cases, caseUploadUrl, sendLoanDocuments, homeImprovementContractUploadUrl, verifyLoanDetails]
---

# Clear GoodLeap loan cases and upload documents

After a loan is submitted it may carry cases (stipulations) that must be cleared before funding.

## Steps

1. **List cases.** `GET /posfinancing/rest/v2/loans/{id}/cases` (`cases`) to see open stipulations on the loan.
2. **Get an upload URL.** `GET /posfinancing/rest/v2/loans/{id}/cases/{caseId}/uploadurl` (`caseUploadUrl`) returns a pre-signed URL.
3. **Upload the file.** `PUT` the document bytes directly to the returned pre-signed URL (not a GoodLeap host).
4. **Home Improvement Contract.** For the HIC, `GET /posfinancing/rest/v2/loans/{id}/uploadurls?uploadType=HIC` (`homeImprovementContractUploadUrl`), then `PUT` to the pre-signed URL.
5. **Send documents / verify.** `POST /posfinancing/rest/v2/loans/{id}/documents` (`sendLoanDocuments`) and `POST /posfinancing/rest/v2/loans/{id}/details` (`verifyLoanDetails`) to confirm loan details.

## Rules

- Uploads are always a two-step pre-signed-URL pattern: request the URL from GoodLeap, then `PUT` to the external URL.
- Authenticate with the JWT bearer token from `generateJwtToken`.
- In sandbox, `Clear Cases` in the Toolbox (`POST /toolbox/rest/v2/loans/{id}/clear`) simulates case resolution.
