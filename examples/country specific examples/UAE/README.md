# UAE Clearance/Status Notification Examples

This directory contains example PUF clearance notification files for the United Arab Emirates
e-invoicing model.

> All party names, identifiers and references are example/anonymised data.

## Background

The UAE adopts a **Decentralised CTC and Exchange (DCTCE) 5-corner Peppol model** for e-invoicing
and e-reporting. Structured invoices are exchanged peer-to-peer over the Peppol network, and the
invoice data is reported to the **UAE Ministry of Finance** e-invoicing system (the reporting
corner / national authority).

Because exchange and reporting run over Peppol, the acknowledgement that flows back is a **Peppol
Message Level Status (MLS)**, which Pagero transforms into the PUF Clearance
Notification format.

## Example Files

| File | Outcome | Notes |
|------|---------|-------|
| `PUF_clearance_notification_UAE_APPROVED.xml` | `APPROVED` | MLS "AP" — report/transport accepted |
| `PUF_clearance_notification_UAE_REJECTED.xml` | `REJECTED` | MLS "RE" — with `cac:Status` issues (BV, SV) |
| `PUF_clearance_notification_UAE_WITHDRAWN_SUCCESS.xml` | `WITHDRAWN_SUCCESS` | MLS "AP" on a withdrawal request — the withdrawal was accepted |
| `PUF_clearance_notification_UAE_WITHDRAWN_FAILURE.xml` | `WITHDRAWN_FAILURE` | MLS "RE" on a withdrawal request — the withdrawal was not accepted, with an issue |

All reference the taxpayer `Emirates Trading Company LLC` (endpoint scheme `0235`, the Peppol
participant identifier scheme for the UAE TIN) and the sender `UAE Ministry of Finance`.

The response code returned by the authority (`AP` or `RE`) is repeated in the first `cac:Status`
as `cbc:StatusReasonCode`. For a negative response, the overall description and each issue follow
in further `cac:Status` elements.

> **Note on the REJECTED example:** This is not an outcome a customer should normally expect to
> receive. A correct, schema- and rule-compliant business file prevents a rejection clearance
> notification. The REJECTED example is provided only to illustrate the structure of a negative
> acknowledgement should one occur.

## Withdrawal responses

A withdrawal reverses a Tax Data Document that was reported earlier. The authority answers the
withdrawal request with its own response, which is delivered as a separate clearance notification
for the same invoice. The issuer therefore receives two notifications for one invoice: the response
to the original report (`APPROVED`) and the response to the withdrawal.

| Response code | Meaning |
|---------------|---------|
| `WITHDRAWN_SUCCESS` | The authority accepted the withdrawal request |
| `WITHDRAWN_FAILURE` | The authority did not accept the withdrawal request |

The withdrawal notification references the same invoice as the original notification, so the two
can be matched on `cac:DocumentReference/cbc:ID` and `puf:ReferencedDocumentInterchangeID`.
`PUF_clearance_notification_UAE_WITHDRAWN_SUCCESS.xml` references the invoice of
`PUF_clearance_notification_UAE_APPROVED.xml`.

> **Note on the WITHDRAWN_FAILURE example:** The issue text is illustrative. The example only shows
> the structure of a negative response to a withdrawal request.

## Status / classification codes

The rejected and withdrawal failure examples carry the **standard Peppol MLS issue-classification
codes** (MLS BT-015) as `Reason code:` in the issue text. They are format-standard rather than
UAE-specific:

| Code | Meaning |
|------|---------|
| `BV` | Business rule violation, fatal |
| `BW` | Business rule violation, warning |
| `FD` | Failure of delivery |
| `SV` | Syntax violation |
