# 11 — Project Plan: Zero to Pilot to V1

**Document ID:** EIP-11 · **Version:** 1.0 · **Status:** Baseline · **Owner:** Founder / Execution lead
**Supersedes:** Stage Gate & Volume Map, Prebuild Governance plan, all roadmap sections in the previous suite

---

## 1. The plan in one line

**Eight phases, 12–18 months, 3–5 people, one product: Circuit-8, single-phase, no relay, no AI.**

## 2. Rules that govern this plan

1. **The governing rule.** If a document, feature or task does not directly help us build, test, validate or launch V1, it must not block V1. It goes to [15_future_roadmap.md](15_future_roadmap.md).
2. **No phase starts before its predecessor's gate passes.** Gates have measurable criteria, never a percentage.
3. **The architecture is frozen** ([03](03_hardware_architecture.md)). Reopening it requires a logged decision with named approver, schedule impact and cost impact.
4. **Money is spent in the order of risk retirement.** The cheapest experiments that can kill the project run first.
5. Owner roles are **roles**, not headcount. In a 3-person team one person holds several.

**Owner role legend:** HW = hardware/metrology · FW = embedded · BE = backend · MOB = mobile · QA = test · PM = product/founder · EXT = external (lab, EMS, consultant, electrician).

---

## 3. Phase 0 — Architecture Freeze and Reality Check

**Duration:** 4–6 weeks · **Cost:** ₹1.5–4 lakh · **Purpose:** kill the project cheaply if it deserves to be killed.

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 0.1 | **Distribution-board survey** — photograph and measure ≥30 real Indian DBs across the target segment | Target segment defined (D-002) | Survey report: DB types, conductor sizes, spare space, CT clearance, Wi-Fi reality | 3 wk | — | PM, HW | ≥30 DBs documented; a stated % where a 9-CT install physically fits | **R-13.** If CTs don't fit, the product doesn't exist | 40k |
| 0.2 | **Segment validation interviews** — 15–20 small commercial owners | 0.1 in progress | Interview notes; willingness-to-pay evidence; the actual pain statement | 3 wk | — | PM | ≥10 interviews; ≥5 state a bill problem they'd pay to solve | Segment may not care | 20k |
| 0.3 | **Regulatory classification opinion** | Product description, no relay | Written opinion on product classification and applicable standards | 4 wk | — | PM, EXT | Written opinion in hand | Classification could add ₹5–10 lakh and 3 months | 50k–2L |
| 0.4 | **Metrology bench decision** — buy vs rent vs lab hire | 0.3 | Costed decision with a booked path to a traceable reference | 2 wk | 0.3 | HW, PM | A specific lab/instrument is committed with dates | Lead times on instruments are long | 10k |
| 0.5 | **Off-the-shelf bench proof** — measure a real load with an eval board + CT, compare against a reference clamp meter | 0.4 | Bench report: can we measure at all, and how far off are we? | 2 wk | 0.4 | HW | A measured error figure exists for ≥3 load types | — | 50k |
| 0.6 | **Freeze architecture and scope** | 0.1–0.5 | [03](03_hardware_architecture.md) signed; [02](02_product_requirements.md) MoSCoW signed | 1 wk | all | PM, HW | Both documents signed by named approvers | — | — |

### 🚦 Gate 0 — Go / No-Go (measurable)

- [ ] ≥30 DBs surveyed and **≥70% assessed as physically installable** with the 9-CT topology
- [ ] ≥5 interviewed businesses state a bill problem and a price they would pay
- [ ] Written classification opinion received, with a costed certification path **≤ ₹10 lakh**
- [ ] Bench proof achieves **≤ ±5%** against a reference on ≥3 load types (a crude number, but it proves the physics and the team)
- [ ] A committed path to a traceable reference exists, with dates
- [ ] [03](03_hardware_architecture.md) and [02](02_product_requirements.md) signed

**No-go action:** if DB fit fails, redesign the form factor (fewer channels, a different sensor, or a metering-point product) before spending on a PCB. **Do not proceed to Phase 1 on optimism.**

---

## 4. Phase 1 — Component Selection and Bench Benchmark

**Duration:** 6–8 weeks · **Cost:** ₹3–8 lakh

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 1.1 | **AFE benchmark** — build eval boards for ≥2 candidates incl. **ADE7816-class 6-channel** | Gate 0 | Comparative report: accuracy, low-current phase error, 9-ch throughput, cost, lead time | 5 wk | — | HW | Measured data for ≥2 parts; a selection with a named approver | Wrong AFE = full respin. **R-03** | 1.5L |
| 1.2 | **CT characterisation** — ≥3 candidate CTs, ≥20 samples each | Gate 0 | Phase error vs current curve, unit-to-unit spread, fit test in surveyed DBs | 5 wk | 0.1 | HW | Selection with second source identified | **R-03, R-13** | 1L |
| 1.3 | Close the **MET-006** field-replacement path | 1.2 | Decision recorded in [14](14_decision_log.md) | 1 wk | 1.2 | HW, PM | One of three paths chosen and costed | **R-15** | — |
| 1.4 | Select MCU module (ETA-valid), flash, secure element, PSU, protection | Gate 0 | [09](09_component_selection.md) completed with part numbers, second sources, 1k/10k costs | 4 wk | — | HW | Every critical part has a second source or a logged risk | Availability | 50k |
| 1.5 | **Isolation and safety architecture review** | 1.4 | Creepage/clearance calculations, isolation drawing, reviewed by an independent engineer | 3 wk | 1.4 | HW, EXT | Independent reviewer signs the isolation architecture | Safety rework is expensive later | 75k |
| 1.6 | Backend and mobile skeletons — auth, MQTT ingest, schema, one chart | Gate 0 | Deployed staging environment ingesting simulated device data end-to-end | 6 wk | — | BE, MOB | A simulated device's frame appears in the app | — | 50k |

### 🚦 Gate 1

- [ ] AFE and CT selected **from measured data**, with reports attached
- [ ] Bench accuracy with the selected parts **≤ ±2.5%** at mid-range (proving ±2.0% is reachable after calibration)
- [ ] CT physically fits in ≥70% of surveyed DBs — **verified with the actual selected part**
- [ ] Every critical part has a second source or a logged, accepted risk
- [ ] Isolation architecture independently reviewed and signed
- [ ] A simulated device frame renders in the mobile app from the staging backend
- [ ] Updated BOM lands within the [10](10_bom_and_cost.md) target band at 1,000 units

---

## 5. Phase 2 — EVT: First Custom Hardware

**Duration:** 10–14 weeks · **Cost:** ₹6–12 lakh

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 2.1 | Schematic design | Gate 1 | Reviewed schematic, net classes, domain labelling | 4 wk | — | HW | Independent schematic review incl. HWR-004 no-relay check | — | — |
| 2.2 | PCB layout to the [04](04_hardware_srs.md) §5 rules | 2.1 | Layout, stack-up signoff, creepage audit, 3D interference check | 4 wk | 2.1 | HW | Release checklist complete, no verbal exceptions | Isolation errors | — |
| 2.3 | Build 20 EVT units | 2.2 | 20 assembled boards + genealogy records | 3 wk | 2.2 | HW, EXT | ≥15 units power up and enumerate all 9 channels | Yield | 2.5L |
| 2.4 | Enclosure prototype and DB fit test | 0.1, 2.2 | Printed enclosures fit-tested in ≥5 DBs per approved type | 3 wk | 2.2 | HW | HWR-010 fit evidence photographed | **R-13** | 1L |
| 2.5 | **EMC pre-scan** | 2.3 | Emissions + immunity pre-scan report | 2 wk | 2.3 | HW, EXT | Report exists; failures have a root-cause plan at PCB/enclosure level | Respin trigger | 1.5L |
| 2.6 | First calibration procedure + fixture concept | 2.3, 0.4 | Documented per-CT gain and phase calibration procedure | 3 wk | 2.3 | HW | 5 units calibrated and re-measured | — | 1L |

### 🚦 Gate 2 — EVT exit

- [ ] ≥15 of 20 units functional across all 9 channels
- [ ] Post-calibration accuracy **≤ ±3%** at mid-range on ≥5 units (the DVT-ready threshold, not the final claim)
- [ ] EMC pre-scan complete with a root-cause plan for every failure
- [ ] Enclosure fits ≥5 real DBs per approved type
- [ ] Independent schematic/BOM/netlist review confirms **no switching path** (HWR-004)
- [ ] Full EVT exit package per [04](04_hardware_srs.md) §9 released

---

## 6. Phase 3 — Firmware to Feature-Complete

**Duration:** 12–16 weeks, overlapping Phase 2 from week 6 · **Cost:** ₹1–3 lakh

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 3.1 | Metrology driver + 1 s/1 min/5 min aggregation | 2.3 | Working acquisition on real hardware | 4 wk | 2.3 | FW | 24 h run, ≥99.9% valid records (FWR-007) | — | — |
| 3.2 | **Packed binary journal**, 7-day retention | 3.1 | FWR-009/010 implemented; storage budget verified on hardware | 3 wk | 3.1 | FW | 7-day outage test with zero double counting (FWR-003) | Flash sizing | — |
| 3.3 | Secure boot, signed OTA with anti-rollback, dual slots | 2.3 | SEC-002 implemented | 4 wk | 2.3 | FW | Invalid + obsolete images fail safely and are journaled | Bricking risk | — |
| 3.4 | BLE commissioning, proximity- and time-bound | 3.1 | SEC-004 / FWR-006 implemented | 3 wk | 3.1 | FW, MOB | Unauthorised nearby actor cannot claim a device in test | — | — |
| 3.5 | MQTT/TLS, **bundled frames**, session-gated live | 3.1, 1.6 | FWR-008 + D-004 implemented | 3 wk | 1.6 | FW, BE | Broker metrics show **zero** 1 s traffic with no open session | **R-01 cost blowout** | — |
| 3.6 | Clock discipline, outage/excursion journaling, residual inputs | 3.2 | FWR-011/012/013 | 3 wk | 3.2 | FW | Deliberate outage reported within ±2 s | — | — |
| 3.7 | HIL rack + nightly fault injection | 3.1 | ≥3-unit rack running nightly | 2 wk | 3.1 | QA, FW | Nightly report published; failures raise tickets | — | 75k |

### 🚦 Gate 3

- [ ] All FWR-001…013 implemented with acceptance evidence
- [ ] 7-day WAN-outage test: full recovery, **zero double counting**
- [ ] Zero 1-second traffic without an open session, proven by broker metrics
- [ ] OTA install + rollback demonstrated on ≥10 units
- [ ] Nightly HIL green for 10 consecutive nights

---

## 7. Phase 4 — Cloud and Mobile to Pilot-Ready

**Duration:** 10–14 weeks, overlapping Phase 3 · **Cost:** ₹2–4 lakh

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 4.1 | Ingest, validation, quarantine, time-series storage | 1.6 | CLR-001/002/003 | 4 wk | — | BE | Malformed and cross-tenant packets reach quarantine, never a chart | — | — |
| 4.2 | Device registry, desired/reported, staged OTA cohorts | 3.3 | CLR-004 + ADM-003 | 3 wk | 3.3 | BE | Cohort rollout with manual stop demonstrated | — | — |
| 4.3 | Residual channel, power-quality log, deterministic rules engine | 4.1 | CLR-008/009 + AIR-001…008 | 4 wk | 4.1 | BE | Every insight carries evidence, rule version and a limitation | Embarrassing false insights | — |
| 4.4 | Session-gated live endpoint + timeout | 3.5 | CLR-007, `POST /live-session` | 2 wk | 3.5 | BE | Server-side timeout verified | **R-01** | — |
| 4.5 | Mobile: commissioning flow (installer) | 3.4 | MOB-002 | 5 wk | 3.4 | MOB | A non-author electrician commissions a unit using only the app | Installer usability | — |
| 4.6 | Mobile: daily-use screens, residual, PQ log, alerts | 4.3 | MOB-003/007/008/009 | 5 wk | 4.3 | MOB | Stale state visually distinct from zero; live session closes on navigate-away | — | — |
| 4.7 | Admin surface (5 capabilities only) | 4.2 | ADM-001…005 | 3 wk | 4.2 | BE | Support can resolve a simulated ticket without DB access | Scope creep to enterprise | — |
| 4.8 | Security test round + threat-model review | 4.1–4.7 | SEC-005/007/008 report | 3 wk | 4.7 | BE, QA | No unaccepted critical finding | — | 1.5L |
| 4.9 | Runbooks: packet storm, cert revocation, deletion request, rollback, region loss, compromised cohort | 4.8 | Six runbooks, each rehearsed once | 2 wk | 4.8 | BE | Each runbook executed in a tabletop | — | — |

### 🚦 Gate 4

- [ ] End-to-end: real device → cloud → app, with correct residual and PQ log
- [ ] An electrician who did not build it commissions a unit in **≤90 minutes** using only the app
- [ ] Security report signed with no unaccepted critical finding
- [ ] All six runbooks rehearsed
- [ ] Data export and deletion workflows demonstrated on a test tenant

---

## 8. Phase 5 — DVT and Validation

**Duration:** 10–14 weeks · **Cost:** ₹8–18 lakh

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 5.1 | Respin PCB with EVT + EMC fixes | Gate 2, 2.5 | DVT revision released | 4 wk | — | HW | Release checklist complete | — | 1.5L |
| 5.2 | Build 50 DVT units | 5.1 | 50 units with genealogy | 3 wk | 5.1 | HW, EXT | ≥45 pass electrical fixture test | Yield | 4L |
| 5.3 | **Traceable metrology campaign** per [08](08_testing_validation.md) §3 | 5.2, 0.4 | Accuracy report: ≥6 current × ≥3 PF × ≥3 temp points on ≥5 units | 4 wk | 5.2 | HW, QA, EXT | **HWR-003a ≤ ±2.0% met**, uncertainty stated | **R-04.** The make-or-break test | 4L |
| 5.4 | Safety + EMC pre-compliance to the classification scope | 5.2, 0.3 | Lab reports | 4 wk | 5.2 | HW, EXT | No unresolved failure | Respin trigger | 3L |
| 5.5 | 30-day soak on ≥20 units | 5.2, Gate 3 | Soak report | 5 wk | 5.2 | QA | Zero data-integrity defects; no memory leak or counter drift | — | 50k |
| 5.6 | Production test fixture + calibration jig | 5.2, 2.6 | Fixture that calibrates and tests a unit in a bounded cycle time | 5 wk | 2.6 | HW, EXT | 20 units calibrated through the fixture, results within spec | Fixture lead time | 3L |
| 5.7 | Installer work instruction, PPE spec, site rejection criteria, training | 5.4 | Approved installation package | 3 wk | 5.4 | PM, HW | **Hazard review approves — the no-waiver gate** | Safety | 50k |

### 🚦 Gate 5 — the hardest gate

- [ ] **HWR-003a met on ≥5 units with a traceable report and stated uncertainty**
- [ ] Safety + EMC evidence complete, no unresolved failure
- [ ] 30-day soak passed with zero data-integrity defects
- [ ] Production fixture calibrates and tests a unit within its cycle-time target
- [ ] **Hazard review has approved installation — no waiver exists or can be granted**
- [ ] DVP&R in [04](04_hardware_srs.md) §8 shows no open row without an approved waiver

---

## 9. Phase 6 — Pilot

**Duration:** 12–16 weeks · **Cost:** ₹5–10 lakh

| # | Task | Prereq | Deliverable | Dur. | Depends | Owner | Completion criterion | Risk | Cost ₹ |
|---|---|---|---|---|---|---|---|---|---|
| 6.1 | Recruit 20–30 pilot sites (mostly small commercial) | Gate 5, 0.2 | Signed pilot agreements incl. data consent | 4 wk | — | PM | ≥20 sites signed | Recruitment slower than expected | 1L |
| 6.2 | Product liability insurance in force | Gate 5 | Policy document | 2 wk | Gate 5 | PM | **Policy active before the first install** | Blocking | 1L |
| 6.3 | Install 20–30 sites with trained electricians | 6.1, 6.2, 5.7 | Commissioning records + photos per site | 6 wk | 6.1 | PM, EXT | Median commissioning ≤90 min; rework <10%; rejections recorded | Install reality | 1.5L |
| 6.4 | Per-site reference spot check | 6.3 | Comparison against a reference clamp meter at each site | 4 wk | 6.3 | QA | Within the HWR-003a envelope at **every** site | Field variance | 50k |
| 6.5 | Operate, support, iterate for 60+ days | 6.3 | Support log, uptime report, insight acceptance data | 10 wk | 6.3 | all | ≥95% of expected 5-minute intervals delivered | — | 1L |
| 6.6 | **Payback validation** | 6.5 | Documented action taken and bill change per site | 6 wk | 6.5 | PM | **≥1 verified action per site within 60 days** | **The commercial make-or-break** | — |
| 6.7 | Willingness-to-pay interviews at the real price | 6.6 | Pricing evidence | 3 wk | 6.6 | PM | ≥60% would recommend at the tested price | — | — |

### 🚦 Gate 6 — Commercial Go / No-Go

- [ ] All six [08](08_testing_validation.md) §5 pilot targets met
- [ ] Support load per site is sustainable at the modelled cost
- [ ] Real unit economics from [10](10_bom_and_cost.md) §5 recomputed with **actuals**, still positive
- [ ] ≥60% of pilot customers would recommend at the tested price
- [ ] Zero safety incidents

**No-go actions, in order of preference:** re-price · narrow the segment · reduce channel count to cut the CT stack · re-scope to a metering-point-only product. **Do not respond to a failed payback test by adding features.**

---

## 10. Phase 7 — Commercial V1 Launch

**Duration:** 12–16 weeks · **Cost:** ₹15–35 lakh (dominated by certification, tooling and first inventory)

| # | Task | Prereq | Deliverable | Dur. | Owner | Completion criterion | Cost ₹ |
|---|---|---|---|---|---|---|---|
| 7.1 | Full safety + EMC certification | Gate 6 | Certificates | 8 wk | HW, EXT | Certificates in hand for the exact shipping configuration | 3–10L |
| 7.2 | Enclosure tooling | Gate 6 | Tooled parts, first-article approval | 10 wk | HW, EXT | First articles pass fit and flame requirements | 3–12L |
| 7.3 | PVT build, 200–500 units | 7.1, 7.2, 5.6 | PVT lot with yield and calibration data | 6 wk | HW, EXT | Yield ≥95%; every unit calibrated through the fixture | 6–15L |
| 7.4 | Installer network in the launch city | Gate 6 | ≥5 trained, contracted electricians | 6 wk | PM | Each has completed a supervised install | 1L |
| 7.5 | Support, RMA and warranty process live | Gate 6 | Documented process with SLAs and a warranty reserve account | 4 wk | PM | A simulated RMA completes end to end | 50k |
| 7.6 | Claim review of all published material | 7.1 | HWR-007 checklist signed for every claim | 2 wk | PM, HW | **No "99% accurate" anywhere. No 100 A claim without evidence** | — |
| 7.7 | 5-year firmware support commitment published | 7.1 | Public support policy | 1 wk | PM | Policy published with a resourcing plan | — |

### 🚦 Gate 7 — Ship

All eight [08](08_testing_validation.md) §7 release criteria met, with no open critical or high defect and no unapproved waiver.

---

## 11. Timeline

```
Month:  1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18
P0    ████
P1        ██████
P2            ██████████
P3              ██████████████
P4                  ████████████
P5                          ██████████
P6                                  ████████████
P7                                          ████████████
Gates:   G0    G1      G2  G3  G4      G5      G6          G7
```

**Realistic band: 14–18 months.** The 12-month case requires no PCB respin beyond the planned DVT revision, no certification surprise, and no CT-fit failure. Plan for 16.

## 12. Total cost to V1 launch (excluding salaries)

| Phase | ₹ |
|---|---|
| 0 | 1.5–4 L |
| 1 | 3–8 L |
| 2 | 6–12 L |
| 3 | 1–3 L |
| 4 | 2–4 L |
| 5 | 8–18 L |
| 6 | 5–10 L |
| 7 | 15–35 L |
| **Total** | **₹41 L – 94 L** |

Add salaries for 3–5 people over 14–18 months and a ≥6-month runway buffer beyond the planned pilot date. **Raise against Gate 1 and Gate 5 evidence, not against a deck.**

## 13. What is deliberately not in this plan

AI/ML of any kind · NILM · three-phase · relay/control · enterprise features · multi-site · carbon and credit scoring · agentic assistants · manufacturing genealogy platforms. All are in [future/](future/) and governed by [15_future_roadmap.md](15_future_roadmap.md). **None of them may become a V1 task without a logged decision that names what it removes.**
