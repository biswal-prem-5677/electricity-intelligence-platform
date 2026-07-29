# Future — Analytics and AI beyond Version 1

**Status:** Parked. Not a V1 commitment. Nothing here may block V1.
**Unparking rule:** an item moves to a release only when ≥100 devices have produced ≥90 days of quality-flagged data, the item has a named owner, an evaluation dataset, a safety/claim review, and a measurable acceptance gate.

## Why this is parked

V1 analytics is deterministic arithmetic and threshold rules. That is not a limitation — it is the correct sequencing. Every capability below needs either (a) a labelled dataset that does not exist yet, or (b) a billing cycle of real customer data. Building the model before the data exists produces a model tuned to imagination.

## The AI philosophy that survives into every future release

AI ranks and explains opportunities found in measured data. It never invents a measurement, certifies electrical safety, controls a load, or asserts appliance identity unless confidence and evidence meet a published threshold. Measurement is the product; analytics makes it useful.

## Parked capabilities

| Capability | Customer output | Method | Data prerequisite | Earliest |
|---|---|---|---|---|
| Bill forecast with interval | Expected month-end bill + confidence band | Quantile forecast over cumulative kWh vs. naive baseline | 2 full billing cycles × 100 sites | V1.1 |
| Budget tracking | Remaining budget, daily allowance, projected overspend | Customer budget + bill forecast | Bill forecast validated | V1.1 |
| Billing verification | Difference between expected and invoice, with possible causes | Aligned billing dates, tariff version, taxes and fixed charges | 50 aligned real invoices | V2 |
| Personalised recommendation | Specific quantified action with range | Counterfactual from measured pattern + tariff | Rules-engine precision proven in V1 pilot | V2 |
| Time-of-day tariff optimisation | Suggested load schedule under ToD slabs | Versioned tariff engine + measured load profile | ToD tariff available in launch state | V2 |
| Anomaly / fault candidate | Unexpected cycling, current or continuous-run pattern | Change-point + anomaly model with technician review loop | ≥200 technician-labelled events | V2/V3 |
| Circuit health trend | Trend score and maintenance candidate | Circuit-specific longitudinal trend | 12 months of same-circuit data | V3 |
| Carbon estimate | Emissions with named grid factor, version and date | kWh × published emission factor | None — cheap. Parked only because value is low | V1.1 |
| Home/site energy score | Transparent score decomposed into drivers | Normalised measured trends, never an opaque peer rank | Cohort of ≥500 comparable sites | V3 |
| Anonymous cohort benchmark | Similar-site percentile | Privacy-reviewed consented cohort | Minimum cohort + re-identification review | V3+ |
| Appliance life estimate | Range of remaining service risk | Manufacturer + technician evidence + longitudinal measurement | Cannot be inferred from current signature alone | Research |
| Generative assistant | Explains an authorised user's own insights | Retrieval over customer-authorised data + approved help material | V2 data model stable | V2+ |
| Agentic actions | Drafts explanations only | Allowlisted read-only tools | — | V2+ |

## Non-negotiable guardrails for any future model

- Every rule or model has a card: purpose, inputs, training population, exclusions, offline metric, cohort/fairness check, owner, version, promotion result, rollback trigger, expiry date.
- Train/test splits prevent leakage across both time and site.
- Customer feedback is a label *candidate*, not ground truth.
- Poor input quality suppresses the output; it never degrades silently.
- Champion/challenger offline, then a small online cohort, then promotion with a named human approver.
- A customer-facing explanation always names the measurement, the time window and the comparison — never just an AI label.
- A generative assistant may not alter device configuration, dispatch an electrician, make billing or legal claims, access another tenant, or override alert/OTA controls.

## Explainability standard (carried forward from the original AI SRS)

An insight must show: measured circuit/site, observation window, comparison baseline, confidence, likely impact range, recommended action, and when to call an electrician.

> "Kitchen socket circuit averaged 0.48 kW between 00:00–05:00 for 5 of the last 7 nights, 0.39 kW above its prior baseline. Check always-on loads; this is not a diagnosis."

That sentence is achievable with V1 rules. Everything above it in this document is an improvement on that sentence, not a replacement for it.
