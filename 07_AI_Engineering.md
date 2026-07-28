# Phase 7 — AI Engineering Plan

## Data and feature policy

Only use customer-authorised, quality-flagged telemetry. Features include load duration, schedule, rolling robust baseline, deviation, voltage/PF context, tariff window and circuit label/category. Do not infer identity, income, occupancy or protected traits unless separately assessed and necessary.

## Delivery sequence

| Capability | Initial method | Metric / safety gate |
|---|---|---|
| Bill estimate | Deterministic tariff calculation | Exact reproducibility from kWh + tariff version |
| Unusual load | Robust seasonal/time-of-week baseline | Precision/alert burden by cohort |
| Forecast | Quantile model with naive baseline | Improves baseline without harmful overconfidence |
| Fault candidate | Change point/anomaly + technician review | Reviewed precision, safe wording, escalation path |
| Appliance health | Circuit-specific trend | No diagnosis; owner review evidence |
| NILM | Research-only hybrid model | Per-class calibration and correction UX |

## MLOps and explainability

Every rule/model has a card: purpose, inputs, training population, exclusions, metric, fairness/cohort checks, owner, version, promotion result, rollback and expiry. Champion/challenger evaluates offline before a small online cohort. Inputs with poor quality suppress the result. A customer-facing explanation always names the measurement, time window and comparison—not just an AI label.

## Generative and agentic AI

If introduced, a generative assistant may retrieve approved help material and explain an authorised user’s own insights. It cannot alter device configuration, send an electrician, make billing/legal claims, access other tenants, or override alert/OTA controls. Tools are allowlisted, outputs are cited/filtered, and actions require explicit confirmation.

