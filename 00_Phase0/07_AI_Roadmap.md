# AI Roadmap

## AI philosophy

AI ranks and explains opportunities found in measured data. It does not invent measurements, certify electrical safety, control loads autonomously, or state that an appliance is identified unless confidence and evidence meet a published threshold.

| Stage | Capability | Method | Release gate |
|---|---|---|---|
| MVP | Tariff-aware cost estimate; baseline and threshold alerts | Deterministic rules + robust statistics | Explainability and alert-precision pilot evidence |
| Phase 2 | Forecast and personalised recommendations | Quantile forecast, segment models | Back-test against holdout and no harmful recommendation evidence |
| Phase 3 | Fault/anomaly candidates and appliance-health trends | Change-point/anomaly models with human label loop | Technician review precision and clear escalation wording |
| Future | NILM suggestions | Hybrid waveform/circuit/plug data | Per-device confidence, false-positive limits and customer correction |
| Future | Generative assistant | Retrieval over customer-authorised data and approved knowledge | Strict tool permissions, citations and safety policy |

## Model requirements

Every model has a version, data lineage, intended population, input-feature schema, offline metric, online guardrail, rollback owner and expiry/review date. Train/test splits must prevent leakage across time and household/site. Customer feedback is a label candidate, not unquestioned truth.

## Explainability standard

An insight must show: measured circuit/site, observation window, comparison baseline, confidence, likely impact range, action, and when to call an electrician. Example: “Kitchen socket circuit averaged 0.48 kW between 00:00–05:00 for 5 of the last 7 nights, 0.39 kW above its prior baseline. Check always-on loads; this is not a diagnosis.”

## Explicit deferrals

NILM is not an MVP dependency. Agentic AI may draft an explanation but cannot issue device commands, alter tariffs, suppress safety notifications, or access another tenant/site without explicit authorisation.

## Complete analytics roadmap

| Module | Customer output | Measurement and method | Stage / acceptance guardrail |
|---|---|---|---|
| Bill prediction | Expected month-end bill and interval | Cumulative kWh, tariff/slab version, billing-cycle dates; deterministic calculation first | MVP when tariff is fully configured; report confidence only for forecast component |
| Budget tracking | Remaining budget, daily allowance, projected overspend | Customer-set budget plus bill prediction | MVP after bill-prediction validation; never imply utility balance |
| Energy waste | Persistent/after-hours/standby candidate | Circuit baseline and duration rules | MVP; customer action is optional and explainable |
| Personalised recommendation | Specific quantified action, e.g. reduce a measured load window | Counterfactual range from measured pattern and tariff | MVP/Phase 2; show assumptions and range, never guarantee saving |
| Tariff optimisation | Suggested schedule/behaviour under configured slab or time-of-day tariff | Versioned tariff engine and measured load profile | MVP for static slabs; ToD, solar/battery later |
| Billing verification | Difference between expected and invoice, with possible causes | Aligned billing dates, tariff, taxes/fixed charges and utility bill input | Phase 2; flag for review, never claim bill error |
| Appliance/circuit health | Trend score and maintenance candidate | Direct circuit/plug behaviour plus technician labels | Phase 3; no diagnosis or safety certification |
| Fault candidate | Unexpected cycling, current or continuous running pattern | Quality-gated anomaly model and technician review loop | Phase 3; false-fault rate target set per labelled cohort |
| Appliance-life estimate | Range of remaining service risk | Manufacturer/technician evidence and longitudinal direct measurement | Research; cannot be inferred from generic current alone |
| Carbon impact | Estimated emissions / contextual equivalence | Published grid-emissions factor version and kWh | Phase 2; label factor, geography and date |
| Home energy score | Transparent score with improvement drivers | Normalised measured trends, not opaque peer rank | Phase 3; score decomposes into components |
| Anonymous benchmark | Similar-cohort percentile only | Large consented, privacy-reviewed cohort | Research; minimum cohort/privacy threshold required |
| NILM | Confidence-ranked appliance suggestion | Whole-home waveform plus directly measured ground truth | V3+ research; manual correction and per-class validation |

## KPIs and measurement definitions

| KPI | Definition | Initial target / decision rule |
|---|---|---|
| Bill-estimate error | Absolute error against an aligned bill, excluding clearly itemised non-energy charges | Set a segment-specific target after 50 aligned bills; do not promise 95% before evidence |
| Fault false-alert rate | Technician/customer-reviewed false candidates divided by reviewed candidates | <5% only for a model/cohort that has enough labels; otherwise report observed rate |
| Recommendation acceptance | Users marking a recommendation acted upon or scheduling service | ≥30% is a product hypothesis, not success proof |
| Measured saving | Normalised change after user action vs baseline | Report range and uncertainty; avoid attributing unrelated weather/occupancy changes |
| Alert burden | Alerts per active site/week and dismissal rate | Keep median ≤2/week unless user requests more |

## Data architecture extension

    Metered records → quality checks → aligned billing/tariff records → feature store
                                                                  │
               technician/customer-reviewed labels ───────────────┤
                                                                  ▼
                                            rule/model registry → evidence-backed insight

Raw waveform retention is exceptional and consent/event bounded. Models must be evaluated separately for plug, switch and circuit measurements because these sources have different attribution certainty.
