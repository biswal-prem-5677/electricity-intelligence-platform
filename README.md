# Electricity Intelligence Platform

**Baseline:** V1.0 · **Status:** Phase 0 — Architecture Freeze and Reality Check · **Next gate:** 🚦 Gate 0
**Primary market:** India · **Owner:** Product and Engineering

---

## What we are building

**Circuit-8** — a single-phase, branch-circuit energy monitor for small commercial sites in India. One service CT plus eight branch CTs, a metering AFE behind a single shared isolated voltage reference, a pre-certified Wi-Fi/BLE module, and a mobile app.

**Measure → Store → Visualize → Rules.** That is V1 in four words.

It is a behind-the-meter monitoring appliance. It is **not** a utility billing meter, not a protection device, and not an MCB/RCCB replacement. The utility meter and invoice remain authoritative.

## What we are deliberately not building in V1

No relay or any switching path · no three-phase · no AI, ML or NILM · no enterprise features · no DIY installation. Every one of these is documented, costed and scheduled in [future/](future/) — none of it is cancelled, and none of it may block V1.

## The governing rule

> **If a document, feature or task does not directly help us build, test, validate or launch Version 1, it must not block Version 1. It goes to the Future Vision roadmap.**

## Start here

| If you want to… | Read |
|---|---|
| Understand the product and the customer | [01_project_definition.md](01_project_definition.md) |
| Know what V1 does and does not do | [02_product_requirements.md](02_product_requirements.md) |
| See the frozen hardware architecture | [03_hardware_architecture.md](03_hardware_architecture.md) 🔒 |
| Know what to do next week | [11_project_plan.md](11_project_plan.md) → [12_progress_tracker.md](12_progress_tracker.md) |
| Understand why something is the way it is | [14_decision_log.md](14_decision_log.md) |

## The 15 core documents

| # | Document | Owner |
|---|---|---|
| 01 | [Project Definition](01_project_definition.md) — customer, segment, competition, positioning | Product |
| 02 | [Product Requirements](02_product_requirements.md) — MoSCoW scope freeze | Product |
| 03 | [Hardware Architecture](03_hardware_architecture.md) 🔒 **FROZEN** — F1–F8 | Hardware |
| 04 | [Hardware SRS](04_hardware_srs.md) — requirements, metrology, PCB, DVP&R | Hardware |
| 05 | [Firmware SRS](05_firmware_srs.md) — FWR-001…013, storage budget, security, OTA | Embedded |
| 06 | [Cloud & Backend SRS](06_cloud_backend_srs.md) — MQTT contract, data model, deployment | Backend |
| 07 | [Mobile App SRS](07_mobile_app_srs.md) — five flows, UX honesty rules, minimal admin | Mobile |
| 08 | [Testing & Validation](08_testing_validation.md) — metrology plan, certification, pilot | Quality |
| 09 | [Component Selection](09_component_selection.md) — AFE benchmark, CT characterisation | Hardware |
| 10 | [BOM & Cost](10_bom_and_cost.md) — BOM, NRE, unit economics, payback | Product |
| 11 | [Project Plan](11_project_plan.md) — phases 0–7, tasks, gates | Founder |
| 12 | [Progress Tracker](12_progress_tracker.md) — the only status document | Founder |
| 13 | [Risk Register](13_risk_register.md) — R-01…R-20 | Founder |
| 14 | [Decision Log](14_decision_log.md) — D-001…D-010, F1–F8 | Founder |
| 15 | [Future Roadmap](15_future_roadmap.md) — the gatekeeper for `future/` | Product |

## `future/` — parked, not cancelled

[AI_V2](future/AI_V2.md) · [NILM](future/NILM.md) · [Three_Phase](future/Three_Phase.md) · [Future_Hardware](future/Future_Hardware.md) · [Future_AI](future/Future_AI.md) · [Enterprise](future/Enterprise.md) · [Commercial](future/Commercial.md) · [Research](future/Research.md)

Nothing leaves this folder without meeting the five unparking conditions in [15_future_roadmap.md](15_future_roadmap.md) §3 — starting with **Gate 6 passed**.

## The five decisions that shape everything

| | Decision | Because |
|---|---|---|
| **D-002** | Beachhead is small commercial, not premium homes | 4–12 month payback vs 29–70 months |
| **D-003** | Committed accuracy ±2.0%; ±1.0% only as a stretch with a stated envelope | Split-core CT phase error at low current is real |
| **D-004** | Bundled frames, 5-minute default, session-gated live stream | ~2,700× cloud cost difference |
| **D-005** | No AI or ML in V1 | ML needs ground truth we must ship hardware to earn |
| **F6** | No relay, contactor or triac, anywhere | Removes the highest-consequence failure mode and a large slice of certification scope |

## Plan shape

**8 phases · 14–18 months · 3–5 people · ₹41–94 lakh excluding salaries.**

```
P0 Reality check → P1 Components → P2 EVT → P3 Firmware
→ P4 Cloud+Mobile → P5 DVT → P6 Pilot → P7 Launch
   G0        G1       G2   G3      G4        G5      G6      G7
```

Every gate has measurable criteria. **Percentages are banned as a progress measure.**

## The two tests that decide whether this is a business

1. **Gate 5 —** does it measure to ±2.0% against a traceable reference, safely, in a real panel?
2. **Gate 6 —** does a paying customer take an action and see the bill fall?

Everything else is in service of those two questions.

## Working rules

- The architecture is frozen. Reopening it needs a logged decision with a named approver, schedule impact and cost impact.
- Any addition to V1 scope must name what it removes ([12](12_progress_tracker.md) §9).
- A requirement without current evidence is open. A bench demonstration does not close it.
- No published claim without the HWR-007 checklist. **"99% accurate" is prohibited.**
- No engineering build enters a customer panel before the hazard review approves it. **This gate has no waiver path.**
