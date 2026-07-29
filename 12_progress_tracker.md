# 12 — Progress Tracker

**Document ID:** EIP-12 · **Version:** 1.1 · **Status:** Living document — update weekly · **Owner:** Founder / Execution lead

---

## 1. How to use this

This is the **only** status document. Update it every week.

**Percentages are banned** (R22). "60% done" on a hardware project means nothing — a board that is 95% laid out and 0% verified is worth less than one that is 40% laid out with the isolation calculations signed. Progress is reported as **which evidence stage an item has reached**, because that is the thing that actually changes.

## 2. The five stages

Every work item advances through five stages **in order**. A stage is entered only when the previous one is ✅.

| Stage | Question it answers | Closed by |
|---|---|---|
| **Evidence** | Do we know enough to design this? Measured data, survey results, a written opinion, a benchmark | A document or dataset someone else could check |
| **Implementation** | Does the thing exist? Schematic, layout, code, service, screen | An artifact in the repo or on a bench |
| **Testing** | Does it work in our hands, under our conditions? | A test report with a verdict |
| **Validation** | Does it work in the real world — real panel, real installer, real customer, traceable reference? | Field or traceable-lab evidence |
| **Done** | Released, with every claim reviewed and every requirement's evidence current | Gate sign-off by a named approver |

**Status marks:** ✅ complete · 🟡 in progress · ❌ not started · ⛔ blocked · ➖ not applicable

**The rule that gives this teeth:** an item cannot be marked ✅ at a stage because it feels finished. It is ✅ when the closing artifact in the right-hand column above exists and is findable. Anything else is 🟡 (R13).

## 3. Overall position

| | |
|---|---|
| **Current phase** | Phase 0 — Bench Prototype tier |
| **Funding tier** | Tier 1 (₹0.5–2 L) — self-fundable |
| **Next gate** | 🚦 Gate 0 |
| **Gate 0 target date** | _set at kickoff_ |
| **Spend to date** | ₹0 of ₹0.5–2 L for this tier |
| **Top risk right now** | **R-13 — will 9 CTs physically fit in a real Indian distribution board?** |
| **Architecture status** | 🔒 Frozen ([03](03_hardware_architecture.md)) |
| **Planning status** | 🔒 Frozen — project is in Implementation Engineering ([00](00_execution_principles.md) §VII) |

## 4. Workstream board

### Hardware

| Item | Evidence | Implementation | Testing | Validation | Done |
|---|---|---|---|---|---|
| DB survey (≥30 boards) | ❌ | ➖ | ➖ | ➖ | ❌ |
| CT selection | ❌ | ❌ | ❌ | ❌ | ❌ |
| Metering AFE selection | 🟡 | ❌ | ❌ | ❌ | ❌ |
| MCU module / flash / secure element | ❌ | ❌ | ❌ | ❌ | ❌ |
| Isolated voltage reference | ❌ | ❌ | ❌ | ❌ | ❌ |
| Isolation architecture | ❌ | ❌ | ❌ | ❌ | ❌ |
| Schematic | ❌ | ❌ | ❌ | ❌ | ❌ |
| PCB layout | ❌ | ❌ | ❌ | ❌ | ❌ |
| Enclosure | ❌ | ❌ | ❌ | ❌ | ❌ |
| Calibration procedure + fixture | ❌ | ❌ | ❌ | ❌ | ❌ |
| **Accuracy ≤ ±2.0% (HWR-003a)** | ❌ | ❌ | ❌ | ❌ | ❌ |
| EMC | ❌ | ➖ | ❌ | ❌ | ❌ |
| Safety / classification | ❌ | ➖ | ❌ | ❌ | ❌ |

**Metering AFE — why Evidence is 🟡 and not ✅:** the evaluation is *specified* ([implementation/01_energy_meter_ic_evaluation.md](implementation/01_energy_meter_ic_evaluation.md)) but nothing has been *measured*. Evidence closes when the benchmark report exists, not when the benchmark plan does (R13). Leading candidate ADE7816; **not selected.**

**⚠ Watch item — potential ECR:** whether the ADE7816 provides per-channel reactive power. PRD-001, CLR-002, the [06](06_cloud_backend_srs.md) §3 frame and MOB-003 all assume `reactivePowerVAR` and `powerFactor`. Resolvable from the datasheet in ~1 day, at zero cost, and it may eliminate a candidate before any board is ordered — **do it first.**

### Firmware

| Item | Evidence | Implementation | Testing | Validation | Done |
|---|---|---|---|---|---|
| Metrology driver + aggregation | ❌ | ❌ | ❌ | ❌ | ❌ |
| Packed binary journal (7-day) | ❌ | ❌ | ❌ | ❌ | ❌ |
| Secure boot + signed OTA | ❌ | ❌ | ❌ | ❌ | ❌ |
| BLE commissioning | ❌ | ❌ | ❌ | ❌ | ❌ |
| MQTT bundled + session-gated live | ❌ | ❌ | ❌ | ❌ | ❌ |
| Clock discipline / outage journal | ❌ | ❌ | ❌ | ❌ | ❌ |

### Cloud

| Item | Evidence | Implementation | Testing | Validation | Done |
|---|---|---|---|---|---|
| Ingest + validation + quarantine | ❌ | ❌ | ❌ | ❌ | ❌ |
| Device registry + OTA cohorts | ❌ | ❌ | ❌ | ❌ | ❌ |
| Residual channel + PQ log | ❌ | ❌ | ❌ | ❌ | ❌ |
| Rules engine (AIR-001…008) | ❌ | ❌ | ❌ | ❌ | ❌ |
| Security + tenant isolation | ❌ | ❌ | ❌ | ❌ | ❌ |
| Runbooks (×6) | ❌ | ❌ | ❌ | ❌ | ❌ |

### Mobile

| Item | Evidence | Implementation | Testing | Validation | Done |
|---|---|---|---|---|---|
| Installer commissioning flow | ❌ | ❌ | ❌ | ❌ | ❌ |
| Daily-use screens + residual + PQ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Alerts + notifications | ❌ | ❌ | ❌ | ❌ | ❌ |
| Admin surface (5 capabilities) | ❌ | ❌ | ❌ | ❌ | ❌ |

### Commercial

| Item | Evidence | Implementation | Testing | Validation | Done |
|---|---|---|---|---|---|
| Segment validation (D-002) | ❌ | ➖ | ➖ | ❌ | ❌ |
| Pricing / willingness to pay | ❌ | ➖ | ➖ | ❌ | ❌ |
| **Verified customer payback** | ❌ | ➖ | ➖ | ❌ | ❌ |
| Installer network | ❌ | ❌ | ❌ | ❌ | ❌ |
| Support / RMA / warranty process | ❌ | ❌ | ❌ | ❌ | ❌ |

## 5. Gate status

| Gate | Meaning | Tier | Status | Evidence location |
|---|---|---|---|---|
| 🚦 G0 | Reality check passed; architecture frozen | 1 | ❌ | |
| 🚦 G1 | Components selected from measured data | 2 | ❌ | |
| 🚦 G2 | EVT hardware works | 3 | ❌ | |
| 🚦 G3 | Firmware feature-complete | 3 | ❌ | |
| 🚦 G4 | End-to-end pilot-ready | 3 | ❌ | |
| 🚦 G5 | **Accuracy and safety proven** | 4 | ❌ | |
| 🚦 G6 | **Pilot proves payback** | 4 | ❌ | |
| 🚦 G7 | Commercial release criteria met | 5 | ❌ | |

## 6. Phase 0 task board

| # | Task | Owner | Status | Evidence | Note |
|---|---|---|---|---|---|
| 0.1 | DB survey, ≥30 boards | PM, HW | ❌ | | Highest-value task in the project |
| 0.2 | 15–20 segment interviews | PM | ❌ | | |
| 0.3 | Regulatory classification opinion | PM, EXT | ❌ | | Longest lead time — **start on day 1** |
| 0.4 | Metrology bench decision | HW, PM | ❌ | | |
| 0.5 | Off-the-shelf bench proof | HW | ❌ | | |
| 0.6 | Freeze architecture and scope | PM, HW | ❌ | | |

### Gate 0 checklist

- [ ] ≥30 DBs surveyed, ≥70% assessed installable
- [ ] ≥5 businesses state a bill problem and a price
- [ ] Classification opinion received; certification path ≤ ₹10 L
- [ ] Bench proof ≤ ±5% on ≥3 load types
- [ ] Committed path to a traceable reference, with dates
- [ ] [03](03_hardware_architecture.md) and [02](02_product_requirements.md) signed

Later-phase task boards are created **when the preceding gate passes**, copied from [11_project_plan.md](11_project_plan.md). Maintaining eight empty boards is documentation work, not execution work (R18).

## 7. Requirement coverage

| Group | Count | Implemented | Tested | Validated |
|---|---|---|---|---|
| Must-have (M1–M16) | 16 | 0 | 0 | 0 |
| Hardware (HWR-001…010) | 10 | 0 | 0 | 0 |
| Metrology (MET-001…006) | 6 | 0 | 0 | 0 |
| Firmware (FWR-001…013) | 13 | 0 | 0 | 0 |
| Cloud (CLR-001…010) | 10 | 0 | 0 | 0 |
| Mobile (MOB-001…009) | 9 | 0 | 0 | 0 |
| Admin (ADM-001…005) | 5 | 0 | 0 | 0 |
| Security (SEC-001…008) | 8 | 0 | 0 | 0 |
| Rules/analytics (AIR-001…008) | 8 | 0 | 0 | 0 |
| **Total** | **85** | **0** | **0** | **0** |

**Implemented ≠ Tested ≠ Validated.** Tested means it passed in our hands. Validated means it passed against a traceable reference or in a real installation.

## 8. Assumption ledger (R11)

Every assumption is a debt. It is closed by evidence, not by repetition.

| ID | Assumption | Load-bearing for | Closes at | Status |
|---|---|---|---|---|
| A-1 | Customers act on data and save ~8% | D-002 payback model, the entire commercial case | G6 | ❌ Open |
| A-2 | 9 CTs physically fit in ≥70% of target DBs | The product existing at all | G0 | ❌ Open |
| A-3 | ±2.0% is achievable with low-cost split-core CTs + calibration | D-003, every published claim | G5 | ❌ Open |
| A-4 | Certification path costs ≤ ₹10 L | Funding tiers 4–5 | G0 | ❌ Open |
| A-5 | An ordinary electrician can commission in ≤90 min | Install economics, R-11 | G4 | ❌ Open |
| A-6 | Cloud cost ₹8–25/device/month under D-004 | Unit economics | G4 | ❌ Open |
| A-7 | Small commercial sites will pay ~₹12,000 + install | Pricing, revenue model | G6 | ❌ Open |
| A-8 | Second sources exist for every critical part | R-09, supply resilience | G1 | ❌ Open |

**A-1 and A-2 are the two that can end the project.** Both are answered in Tier 1, for under ₹2 lakh combined. That is not an accident — it is R2.

## 9. Open decisions awaiting closure

| ID | Decision | Needed by | Owner |
|---|---|---|---|
| OPEN-1 | Metering AFE part selection — spec'd in [IMPL-01](implementation/01_energy_meter_ic_evaluation.md), awaiting measured data | Gate 1 | HW |
| OPEN-2 | CT part and second source | Gate 1 | HW |
| OPEN-3 | MET-006 field CT replacement path | Gate 1 | HW, PM |
| OPEN-4 | Metrology bench: buy vs rent vs lab hire | Gate 0 | HW, PM |
| OPEN-5 | RTOS vs cooperative scheduler | Gate 2 | FW |
| OPEN-6 | Time-series store: PostgreSQL hypertables vs managed | Gate 4 | BE |
| OPEN-7 | Final V1 price point | Gate 6 | PM |
| OPEN-8 | 100 A claim — evidence or permanent deferral | Gate 5 | HW |

Closed decisions move to [14_decision_log.md](14_decision_log.md).

## 10. Weekly update format

```
Week of ____
Phase: __   Gate: __   Funding tier: __   Status: on track | at risk | slipped

Stage advances this week:   (item, stage entered, closing artifact)
Blocked:                    (item, what is blocking, who can unblock)
Assumptions closed:         (A-n, evidence)
Decisions needed from me:
Spend this week: ₹____   Tier cumulative: ₹____ of ₹____
Top risk change:
```

## 11. Scope-change log (R9)

Any addition to V1 scope is recorded here **with what it removes.** An addition with no removal is a schedule slip that has not been admitted yet.

| Date | Added | Removed | Approver | Schedule impact |
|---|---|---|---|---|
| — | — | — | — | — |
