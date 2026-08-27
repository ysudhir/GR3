---
gr3: gr3:de.bbk.awv-z4@4.2.0
type: gr3.record
title: External Payments Report (Z4) — Zahlungen im Außenwirtschaftsverkehr
jurisdiction: DE
region: EMEA
regulator: Deutsche Bundesbank
legal_basis:
  - cite: AWV §67(1)
    source: src/de/awv-2024.de.pdf#sec-67
    lang: de
report:
  code: Z4
  channel: Bundesbank AMS portal
  format: XML (AMS schema v3)
obligation:
  who: resident entities, including bank branches
  trigger:
    prose: >
      Incoming or outgoing cross-border payments above EUR 12,500 or
      equivalent, excluding goods invoices settled within 12 months.
    rule: |
      amount_eur > 12500
      and counterparty.country != "DE"
      and "goods_settlement_short_term" not in exemptions
  frequency: monthly
  deadline: { calendar: "7th calendar day of following month", tz: Europe/Berlin }
data_contract:
  - element: dict:payment.amount        # ISO 20022 pacs.008 …/IntrBkSttlmAmt
  - element: dict:counterparty.country  # ISO 20022 pacs.008 …/Cdtr/PstlAdr/Ctry
  - element: dict:purpose.code          # maps to Bundesbank Kennzahl list
lifecycle:
  status: effective
  effective_from: 2025-01-01
  knowledge_from: 2024-11-18
  supersedes: gr3:de.bbk.awv-z4@4.1.0
  review_by: 2026-11-01
  approvals:
    maker: agent.jaro-extractor@2.4
    checker: sme.de-payments
    four_eyes: regops.lead-emea
translations:
  - lang: en
    of: src/de/awv-2024.de.pdf
    method: agent.jaro-translator@2.3, human-reviewed, segment-aligned
links:
  implements: [[src.de.awv-2024]]
  maps-to: [[dict.payment.amount]], [[dict.counterparty.country]], [[dict.purpose.code]]
---

> **Illustrative record.** Content is abbreviated for demonstration — not legal guidance.

## Source clause (original is evidence)

> „Zahlungen von mehr als 12 500 Euro oder Gegenwert, die Inländer von
> Ausländern entgegennehmen oder an Ausländer leisten, sind der Deutschen
> Bundesbank zu melden …" — `src/de/awv-2024.de.pdf#sec-67`

Aligned translation (interpretation, not evidence): "Payments of more than
EUR 12,500 or equivalent value which residents receive from non-residents or
make to non-residents must be reported to the Deutsche Bundesbank …"

## Interpretation notes

- Bundesbank confirmed in circular 2024/11 that netted treasury flows are
  reportable **gross**, not net. Decided against netting after written
  guidance. — `sme.de-payments`, 2024-11-15
- IBAN-less instant payments: pending clarification — draft v4.3.0 in checker
  review (circular 2026/07).

## Change log

- **4.2.0** — threshold basis clarified per circular 2024/11 (anchor `#sec-67`)
- **4.1.0** — superseded 2024-12-31
