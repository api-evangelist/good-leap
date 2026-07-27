---
name: Submit and track a GoodLeap loan application
description: Authenticate as a partner, calculate a payment offer, submit a loan application, then track its status through the lifecycle.
api: openapi/good-leap-developer-api-openapi.yml
operations: [generateJwtToken, monthlyLoanPayments, submitLoanApplication, loanStatus, loanTimeline, nextAvailableLoanActions]
---

# Submit and track a GoodLeap loan application

Use the GoodLeap Developer API (POS financing v2) to originate and follow a sustainable home-improvement loan. You must already hold partner credentials for `developer.goodleap.com`.

## Steps

1. **Authenticate.** `POST /posfinancing/rest/v2/auth/token` (operation `generateJwtToken`) with your `organizationId`. Send the returned JWT as `Authorization: Bearer <jwt>` on every subsequent call; refresh with `refreshJwtToken` before expiry.
2. **(Optional) Quote a payment.** `GET /posfinancing/rest/v2/payments` (`monthlyLoanPayments`) to calculate estimated monthly payments for an offer before applying.
3. **Submit the application.** `POST /posfinancing/rest/v2/loans` (`submitLoanApplication`) with the applicant object. The response returns the application outcome and a loan id.
4. **Track status.** Poll `GET /posfinancing/rest/v2/loans/{id}/status` (`loanStatus`) and `GET /posfinancing/rest/v2/loans/{id}/timeline` (`loanTimeline`) to follow lifecycle progress.
5. **Decide next action.** `GET /posfinancing/rest/v2/loans/{id}/actions` (`nextAvailableLoanActions`) tells you which transitions (cases, documents, milestones) are available next.

## Rules

- Auth is JWT bearer, not OAuth2. Scope/organization is established at token time via `organizationId`.
- Pagination on list endpoints uses `page` and `pageSize`.
- No idempotency-key is documented; do not blindly retry `submitLoanApplication` — check `loanStatus` first.
- Run against `https://sandbox01-api.goodleap.com` first; use Toolbox triggers to simulate lender transitions in sandbox.
