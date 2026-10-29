# Slovakia Clearance/Status Notification Examples

This directory contains example PUF clearance notification files for Slovakia's e-invoicing
and e-reporting model.

> All party names, identifiers and references are example/anonymised data.

## Background

Slovakia adopts a **Peppol 5-corner model** for mandatory B2B e-invoicing and e-reporting,
effective **1 January 2027** (voluntary use earlier). Structured EN 16931 invoices are exchanged
peer-to-peer over the 4-corner Peppol network, and the invoice data is reported in near real time
to the **Financial Directorate of the Slovak Republic** — the 5th corner and the national Peppol
authority — through the **IS EFA** system.

Because exchange and reporting run over Peppol, the acknowledgement that flows back is a **Peppol
Message Level Status (MLS)**, which Pagero transforms into the PUF Clearance
Notification format.

## Example Files

| File | Outcome | Notes |
|------|---------|-------|
| `PUF_clearance_notification_Slovakia_APPROVED.xml` | `APPROVED` | MLS "AP" — report/transport accepted |
| `PUF_clearance_notification_Slovakia_REJECTED.xml` | `REJECTED` | MLS "RE" — with `cac:Status` issues (BV, SV) |
| `PUF_clearance_notification_Slovakia_WITHDRAWN_SUCCESS.xml` | `WITHDRAWN_SUCCESS` | MLS "AP" on a withdrawal request — the withdrawal was accepted |
| `PUF_clearance_notification_Slovakia_WITHDRAWN_FAILURE.xml` | `WITHDRAWN_FAILURE` | MLS "RE" on a withdrawal request — the withdrawal was not accepted, with an issue |

All reference the supplier `Dunaj Dodavatel s.r.o.` (endpoint scheme `0245`, the Peppol
participant identifier scheme for the Slovak tax identifier DIČ) and the sender
`Slovak Tax Authority`.

The response code returned by the authority (`AP` or `RE`) is repeated in the first `cac:Status`
as `cbc:StatusReasonCode`. For a negative response, the overall description and each issue follow
in further `cac:Status` elements.

> **Note on the REJECTED example:** This is not an outcome a customer should normally expect to
> receive. A correct, schema- and rule-compliant business file prevents a rejection clearance
> notification. The REJECTED example is provided only to illustrate the structure of a negative
> acknowledgement should one occur.

## Withdrawal responses

A withdrawal reverses a Tax Data Document that was reported earlier. In the Peppol Slovakia model
it is a "Disregard" Tax Data Document (type code `D`), which cancels the report made earlier on the
same invoice. The authority answers it with a Message Level Status, like any other Tax Data
Document, and that answer is delivered as a separate clearance notification for the same invoice.
The issuer therefore receives two notifications for one invoice: the response to the original
report (`APPROVED`) and the response to the withdrawal.

| Response code | Meaning |
|---------------|---------|
| `WITHDRAWN_SUCCESS` | The authority accepted the withdrawal request |
| `WITHDRAWN_FAILURE` | The authority did not accept the withdrawal request |

The withdrawal notification references the same invoice as the original notification, so the two
can be matched on `cac:DocumentReference/cbc:ID` and `puf:ReferencedDocumentInterchangeID`.
`PUF_clearance_notification_Slovakia_WITHDRAWN_SUCCESS.xml` references the invoice of
`PUF_clearance_notification_Slovakia_APPROVED.xml`.

> **Note on the WITHDRAWN_FAILURE example:** The issue text and location are illustrative. The
> example only shows the structure of a negative response to a withdrawal request.

## Status / classification codes

The rejected and withdrawal failure examples carry the **standard Peppol MLS issue-classification
codes** (MLS BT-015) as `Reason code:` in the issue text. They are format-standard rather than
Slovak-specific:

| Code | Meaning |
|------|---------|
| `BV` | Business rule violation, fatal |
| `BW` | Business rule violation, warning |
| `FD` | Failure of delivery |
| `SV` | Syntax violation |
