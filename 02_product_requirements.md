# 02 — Product Requirements and V1 Scope

**Document ID:** EIP-02 · **Version:** 1.0 · **Status:** Scope frozen · **Owner:** Product
**Supersedes:** Product Requirements SRS, MVP Scope, AI System SRS, AI Roadmap, Future Opportunity Backlog

---

## 1. Scope freeze

The V1 feature set below is **frozen**. A new feature enters V1 only if it solves a critical engineering or safety problem — not because it is valuable, not because it is cheap, and not because a customer asked. Everything else goes to [15_future_roadmap.md](15_future_roadmap.md).

Adding a feature to V1 requires: a written statement of which V1 feature it *replaces*, approval recorded in [14_decision_log.md](14_decision_log.md), and a schedule impact stated in [11_project_plan.md](11_project_plan.md).

## 2. MoSCoW

### MUST HAVE — V1 does not ship without these

| # | Feature | Why it is a Must |
|---|---|---|
| M1 | Mains measurement: V, I, active/reactive/apparent power, PF, frequency, cumulative active energy | The product |
| M2 | Eight branch channels: active power and energy per labelled circuit | The differentiator |
| M3 | **Residual channel**: mains − Σ branches, displayed as "Unmonitored" | Without it the numbers visibly don't add up and trust dies in week one |
| M4 | Measured accuracy within the published spec, proven against a traceable reference | Everything else is worthless if this fails |
| M5 | 5-minute historical aggregates, retained per policy | Charts, baselines, cost |
| M6 | 1-second live view, ≤2 s P95 latency, **session-gated streaming** | Live view sells the product; always-on 1 s streaming bankrupts it |
| M7 | 7-day local buffer of 1-minute aggregates + event journal, no double counting on recovery | Indian connectivity reality |
| M8 | Monotonic energy counters surviving power loss, reset and cloud outage | Non-negotiable metrology integrity |
| M9 | Tariff configuration and deterministic cost estimate | Turns kWh into a decision |
| M10 | **Power-quality and outage log**: voltage excursions, sags, outage duration and count | Free from data we already capture; high emotional value in India; no accuracy claim required |
| M11 | Rule-based alerts: after-hours load, high continuous load, voltage excursion, device offline | The operational hook for small commercial |
| M12 | Installer commissioning workflow with mandatory safety checklist and site rejection | Safety and install quality |
| M13 | Circuit labelling, category and CT orientation setup | Makes the data legible |
| M14 | Secure boot, signed OTA with rollback, per-device identity, TLS | Cannot be retrofitted later |
| M15 | Customer data export and deletion request | DPDP |
| M16 | Data freshness / stale-state indication distinct from zero consumption | Honest UX; prevents the worst class of support ticket |

### SHOULD HAVE — V1 if the schedule allows, V1.1 otherwise

| # | Feature |
|---|---|
| S1 | Weekly summary notification |
| S2 | CSV/PDF export of the power-quality log |
| S3 | Quiet hours and per-category alert preferences |
| S4 | Installer offline commissioning (complete without connectivity, sync later) |
| S5 | Support-diagnostics consent with a bounded time window |
| S6 | Baseline maturity indicator ("insights available after 7 days") |

### COULD HAVE — only if genuinely free

| # | Feature |
|---|---|
| C1 | Carbon estimate (kWh × published, versioned grid factor) |
| C2 | Hindi + one regional language, human-reviewed |
| C3 | Home-screen widget |
| C4 | Multi-user access sharing within one site |

### WILL NOT HAVE in V1 — parked, not cancelled

Three-phase · relay/load control · Circuit-16 · smart plug · behind-switch module · solar or DC measurement · appliance identification (NILM) · ML forecasting · fault diagnosis · appliance health scores · billing verification against a real invoice · neighbourhood benchmarks · enterprise SSO · multi-site dashboard · open third-party API · subscription tier · payments/recharge · high-resolution waveform retention as a normal feature · utility integration.

Each is documented in [future/](future/) with an unparking gate.

## 3. Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| PRD-001 | Show live mains and labelled branch power with a freshness state | P95 end-to-end live update ≤2 s on a healthy connection while a live session is open; "stale" appears after 15 min without telemetry |
| PRD-002 | Show a residual "Unmonitored" channel computed as mains minus the sum of branch active power | Residual is never displayed as negative; a persistent negative residual raises a commissioning warning (likely CT orientation error) |
| PRD-003 | Store 5-minute history and produce daily/monthly cost estimates | A recomputed view matches source records exactly; the estimate names its tariff version |
| PRD-004 | Support circuit labels, categories and CT orientation configuration | Changes are audited and effective-dated; historical raw data is never rewritten |
| PRD-005 | Alert only from documented rules with visible evidence | Every alert shows circuit, interval, baseline, suggested action and limitation |
| PRD-006 | Record and report voltage excursions and supply outages | Outage start/end reconstructed from the device journal after power returns; monthly summary available |
| PRD-007 | Support qualified-installer commissioning and explicit site rejection | Activation is impossible without a completed checklist; a rejection records its reason |
| PRD-008 | Permit customer data export and deletion request | Export is tenant-scoped; deletion is auditable and policy-bound |
| PRD-009 | Suppress any cost or insight output when calibration, clock or measurement quality is invalid | Rules-engine integration test proves suppression with a visible reason |

## 4. Non-functional requirements

| Area | Requirement |
|---|---|
| Usability | Target installer commissioning ≤90 minutes excluding panel preparation. *(The earlier 45-minute target was optimistic for unfamiliar Indian DBs; 45 min becomes the target at install #50, not install #1.)* |
| Reliability | The device continues measuring, buffering and displaying local status through cloud loss |
| Performance | 95% of app history queries under 3 s at pilot scale |
| Security | Per-device identity, RBAC, audit records — mandatory, see [06_cloud_backend_srs.md](06_cloud_backend_srs.md) |
| Tenancy | No customer may read another tenant's records. Server-enforced, always |
| Privacy | Collect only what the stated function needs; 1-second data is occupancy-revealing and is not retained in the cloud by default |

## 5. V1 analytics — deliberately not AI

V1 analytics is **deterministic arithmetic and threshold rules**. There is no model, no training, no inference.

| ID | Rule | Method |
|---|---|---|
| AIR-001 | Establish a per-circuit baseline only after ≥7 days of valid data | Robust statistics (median / percentile), not a mean |
| AIR-002 | Tariff-aware cost estimate from versioned customer inputs | Pure arithmetic. Reproducible from kWh + tariff version. Never alters measured kWh |
| AIR-003 | After-hours load rule | Circuit active power above threshold during a customer-defined closed window, for N consecutive intervals |
| AIR-004 | High continuous load rule | Circuit above threshold continuously for N hours versus its own baseline |
| AIR-005 | Voltage excursion rule | Voltage outside a configured band for a configured duration |
| AIR-006 | Every insight carries evidence, window, baseline, action and limitation | Review sample contains all fields and no diagnostic or guaranteed-savings language |
| AIR-007 | Rules are versioned and individually disableable per site | A bad rule is switched off without a deployment |
| AIR-008 | Track dismissals and false positives per rule | Feedback is tenant-scoped and feeds rule tuning — not a training set |

**Why this is right:** rules are debuggable, explainable to a customer, and produce the labelled data that any future model will need. Machine learning becomes possible after ~100 devices × 90 days. See [future/AI_V2.md](future/AI_V2.md).

## 6. Data the product produces

| Field | Cadence | Note |
|---|---|---|
| Timestamp, sequence, clock quality | Every record | UTC + quality flag |
| Voltage, frequency | 1 s live / aggregated | Mains only (single shared voltage reference) |
| Per-channel current, active/apparent/reactive power, PF, cumulative active energy | 1 s live / aggregated | Nine channels bundled in **one** frame per interval |
| Channel ID, direction, calibration version | Every record | `direction` reserved for future solar/export |
| Board temperature | 1 min | Reported as "not instrumented" if no qualified sensor is fitted |
| RSSI, network state, buffer depth, reset reason, firmware version | Heartbeat | Role- and consent-controlled |
| Relay state | — | **N/A. Circuit-8 has no relay** |

## 7. Customer journey

1. Site survey confirms single-phase supply, CT clearance, panel type and Wi-Fi at the DB — **or rejects the site.** Rejection is a safety outcome, not a lost sale.
2. Installer authenticates, scans the device, acknowledges safety steps, installs CTs, maps channels, verifies against known loads, hands over.
3. Owner labels circuits, enters tariff details, sets a closing time and notification preferences.
4. Seven days of baseline accumulate; the app says so plainly rather than showing empty charts.
5. Evidence-backed alerts and a weekly review begin. The owner acts, dismisses, or calls an electrician.
6. Feedback is recorded for rule tuning — never treated as ground truth without review.

## 8. Traceability

Requirement prefixes: **PRD** product · **HWR** hardware · **FWR** firmware · **CLR** cloud · **AIR** analytics · **MOB** mobile · **SEC** security · **TST** verification · **MFG** manufacturing · **OPS** operations.

Every implementation, test and release claim must cite an identifier. Coverage is tracked in [08_testing_validation.md](08_testing_validation.md).
