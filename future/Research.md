# Future — Research and Opportunity Backlog

**Status:** Ideas held so they are not lost. None is a commitment. None may influence V1 scope.

## The rule for everything in this file

An opportunity becomes a roadmap item only when **all** of the following exist: a named customer who asked for it, a validated data source, a safety and regulatory assessment, unit economics, a privacy impact assessment, a success metric, and an accountable owner.

**Interesting is not sufficient.**

## Backlog

| Opportunity | Value hypothesis | Data dependency | Key risk | Earliest |
|---|---|---|---|---|
| Appliance health passport | Service history plus energy trend supports maintenance decisions | Direct circuit/plug data + technician labels | Misdiagnosis and liability | V3 |
| Predictive-maintenance subscription | Managed alerting and service outcome, recurring revenue | High-quality labelled faults | False positives drive field-service cost above the subscription price | V3 |
| Saving challenges and rewards | Voluntary goals improve retention | Reliable savings normalisation | Incentive fraud; dark-pattern risk | V2 |
| Society/community anonymous benchmark | "How do I compare to similar sites?" | Large consented, privacy-reviewed cohort | Re-identification; misleading comparison | V3 |
| Insurance-ready operating report | Evidence of monitored operating patterns | Insurer partnership + explicit consent | Regulatory exposure; adverse customer outcomes | Partnership research |
| Electricity reliability/usage profile for lenders or landlords | Consented energy evidence | Longitudinal authorised site data | Discrimination risk, credit regulation, weak causal value. **Strong candidate for permanent rejection** | Research only |
| Demand response participation | Paid load flexibility | Utility programme + control capability | Requires load switching and a utility relationship | V3+ |
| Power-quality reporting as a paid service | Documented voltage excursions and outages for commercial customers | Already measured in V1 | Low — this is a near-term upsell, not research | **V1.1 — see note below** |

## Note: power quality is not research, it is nearly free

Voltage excursion and outage logging is already a V1 requirement. The *product* version of it — a monthly power-quality report a shop owner can show a landlord, an equipment supplier or an insurer — needs no new measurement, no model, and no accuracy claim beyond what V1 already makes. It is the cheapest upsell in this entire folder. It sits here only because packaging and pricing it is a V1.1 decision.

## Standing research questions

1. What fraction of Indian single-phase small-commercial distribution boards physically accommodate 9 split-core CTs? *(Answered by the Phase 0 survey — this is the highest-value unknown in the project.)*
2. How much energy saving does circuit-level feedback actually produce for an Indian small-commercial site over 6 months, after novelty decay? *(Answered by the V1 pilot. Published literature suggests 5–12% for real-time feedback with decay; treat as an assumption until measured.)*
3. What is the real distribution of split-core CT phase error across Indian-market vendors at 1–5% of rated current? *(Answered by the Phase 1 CT characterisation study.)*
4. Does the free data from the DISCOM smart-meter rollout erode willingness to pay for total-consumption visibility? *(Assumed yes. Our defensible line is circuit attribution, not totals.)*
5. What is the actual CAC for an installed ₹12k–17k hardware product sold to Indian small commercial? *(Answered by the pilot.)*

## Ideas evaluated and set aside

- **Appliance life estimation from current signature alone** — not achievable. Remaining useful life is not recoverable from a current waveform without manufacturer or technician evidence.
- **Revenue-grade / utility billing metering** — a different product, a different certification regime, a different customer, and a different liability model. The utility meter remains authoritative in all our materials.
- **Protection functionality (breaker/RCCB replacement)** — permanently out of scope. The product is monitoring evidence, never a protective device.
