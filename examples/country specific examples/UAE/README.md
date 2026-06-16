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

Both reference the taxpayer `Emirates Trading Company LLC` (endpoint scheme `AE:TIN`) and the
sender `UAE Ministry of Finance`.

> **Note on the REJECTED example:** This is not an outcome a customer should normally expect to
> receive. A correct, schema- and rule-compliant business file prevents a rejection clearance
> notification. The REJECTED example is provided only to illustrate the structure of a negative
> acknowledgement should one occur.

## Status / classification codes

The rejected example uses the **standard Peppol MLS issue-classification codes** (MLS BT-015),
which are format-standard rather than UAE-specific:

| Code | Meaning |
|------|---------|
| `BV` | Business rule violation, fatal |
| `BW` | Business rule violation, warning |
| `FD` | Failure of delivery |
| `SV` | Syntax violation |
