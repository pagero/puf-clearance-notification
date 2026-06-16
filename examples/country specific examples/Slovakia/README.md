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

Both reference the supplier `Dunaj Dodavatel s.r.o.` (Peppol EAS `0158` = Slovak IČO) and the
sender `Slovak Tax Authority`.

> **Note on the REJECTED example:** This is not an outcome a customer should normally expect to
> receive. A correct, schema- and rule-compliant business file prevents a rejection clearance
> notification. The REJECTED example is provided only to illustrate the structure of a negative
> acknowledgement should one occur.

## Status / classification codes

The rejected example uses the **standard Peppol MLS issue-classification codes** (MLS BT-015),
which are format-standard rather than Slovak-specific:

| Code | Meaning |
|------|---------|
| `BV` | Business rule violation, fatal |
| `BW` | Business rule violation, warning |
| `FD` | Failure of delivery |
| `SV` | Syntax violation |
