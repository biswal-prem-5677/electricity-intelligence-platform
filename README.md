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

> **If a document, feature or task does not directly help us build, test, validate or launch Version 1, it must not block Version 1. It goes to [future/](future/).**

One of 26 rules in **[00_execution_principles.md](00_execution_principles.md)** — the project's constitution, which outranks every other document here.

## Start here

| If you want to… | Read |
|---|---|
| Know the rules everything else obeys | **[00_execution_principles.md](00_execution_principles.md)** 🔒 |
| Understand the product and the customer | [01_project_definition.md](01_project_definition.md) |
| Know what V1 does and does not do | [02_product_requirements.md](02_product_requirements.md) |
| See the frozen hardware architecture | [03_hardware_architecture.md](03_hardware_architecture.md) 🔒 |
| Know what to do next week | [11_project_plan.md](11_project_plan.md) → [12_progress_tracker.md](12_progress_tracker.md) |
| Understand why something is the way it is | [14_decision_log.md](14_decision_log.md) |

## The 15 core documents

| # | Document | Owner |
|---|---|---|
| 00 | [Execution Principles](00_execution_principles.md) 🔒 **CONSTITUTIONAL** — R1–R26 | Founder |
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
| 14 | [Decision Log](14_decision_log.md) — D-001…D-016, F1–F8 | Founder |

**These 15 are the complete planning set.** They are maintained, not extended — see *Current phase* below.

## [`future/`](future/) — parked, not cancelled

Staged views: [README](future/README.md) · [V2](future/V2.md) · [V3](future/V3.md) · [Moonshots](future/Moonshots.md)
Capabilities: [AI_V2](future/AI_V2.md) · [NILM](future/NILM.md) · [Three_Phase](future/Three_Phase.md) · [Future_Hardware](future/Future_Hardware.md) · [Future_AI](future/Future_AI.md) · [Enterprise](future/Enterprise.md) · [Commercial](future/Commercial.md) · [Research](future/Research.md)

Nothing leaves this folder without meeting the five unparking conditions in [future/README.md](future/README.md) — starting with **Gate 6 passed**.

## The five decisions that shape everything

| | Decision | Because |
|---|---|---|
| **D-002** | Beachhead is small commercial, not premium homes | 4–12 month payback vs 29–70 months |
| **D-003** | Committed accuracy ±2.0%; ±1.0% only as a stretch with a stated envelope | Split-core CT phase error at low current is real |
| **D-004** | Bundled frames, 5-minute default, session-gated live stream | ~2,700× cloud cost difference |
| **D-005** | No AI or ML in V1 | ML needs ground truth we must ship hardware to earn |
| **F6** | No relay, contactor or triac, anywhere | Removes the highest-consequence failure mode and a large slice of certification scope |

## Plan shape

**8 phases · 14–18 months · 3–5 people.**

```
P0 Reality check → P1 Components → P2 EVT → P3 Firmware
→ P4 Cloud+Mobile → P5 DVT → P6 Pilot → P7 Launch
   G0        G1       G2   G3      G4        G5      G6      G7
```

Every gate has measurable criteria. **Percentages are banned as a progress measure** — work is tracked as Evidence → Implementation → Testing → Validation → Done.

## Money: fund by tier, not by total

**₹41–94 lakh is the Commercial MVP figure, not the entry price.** The first tier costs under ₹2 lakh, is self-fundable, and carries the two questions that can kill the project.

| Tier | Buys | Cost | Ends at |
|---|---|---|---|
| **1 Bench Prototype** | Eval board + CTs + reference meter, DB survey, classification opinion | **₹0.5–2 L** | G0 |
| **2 Engineering Prototype** | AFE benchmark, CT characterisation, component selection | ₹3–8 L | G1 |
| **3 EVT** | First custom PCB, 20 units, EMC pre-scan, firmware + cloud | ₹9–19 L | G2–G4 |
| **4 DVT + Pilot** | 50 units, traceable metrology, safety/EMC, 20–30 site pilot | ₹13–28 L | G5–G6 |
| **5 Commercial MVP** | Certification, tooling, PVT lot, installer network | ₹15–35 L | G7 |

Excludes salaries. Raise against **Gate 1** and **Gate 5** evidence, not against a deck.

## The two tests that decide whether this is a business

1. **Gate 5 —** does it measure to ±2.0% against a traceable reference, safely, in a real panel?
2. **Gate 6 —** does a paying customer take an action and see the bill fall?

Everything else is in service of those two questions.

## Current phase — Implementation Engineering

> **Planning is frozen (R25).** The bottleneck is no longer *what should we build* — it is *how do we build the first working unit*.

The architecture is frozen. Do not redesign the product, add features or expand scope. If a **verified engineering blocker** appears, raise an [ECR](00_execution_principles.md#engineering-change-request--the-only-way-the-architecture-moves) — never modify the architecture silently.

| Phase | Produces | Done when | Gate |
|---|---|---|---|
| **1 Component Engineering** | MCU · AFE · CT · PSU · memory · radio · protection · connectors | **BOM v1.0 frozen** | G1 |
| **2 System Architecture** | Block diagram · comms diagram · power tree · signal flow · mechanical constraints. *No PCB yet* | Design is partitionable | G1 |
| **3 Electrical Design** | Schematic: protection, isolation, PSU, AFE, MCU, programming header | Independent review signed | G2 |
| **4 PCB** | Stack-up, layout, creepage audit, fab package | A fab can quote and build | G2 |
| **5 Firmware** | Drivers · AFE · MQTT · OTA · storage · calibration | FWR-001…013 evidenced | G3 |
| **6 Backend** | Broker · API · storage · auth | CLR-001…010 evidenced | G4 |
| **7 Mobile** | Installer + customer flows. *Only now* | Electrician commissions using only the app | G4 |
| **8 Validation** | Bench · field · pilot | G5 and G6 passed | G5–G6 |

Artifacts live in [`implementation/`](implementation/). Every one must include implementation reasoning, alternatives considered, trade-offs, expected cost, engineering effort and validation method. If a document doesn't move the prototype closer to reality, it belongs in [future/](future/).

| Artifact | Closes | Status |
|---|---|---|
| [IMPL-01 — Energy Metering AFE Evaluation](implementation/01_energy_meter_ic_evaluation.md) | OPEN-1 | 🟡 Specified; awaiting measured data |
| [IMPL-01A — AFE Benchmark Plan](implementation/01A_afe_benchmark_plan.md) | OPEN-4 | 🟡 Ready to execute |

**The first irreversible decision is the metering AFE.** **ADE7816** is the *preferred hypothesis* — two parts cover all 9 channels behind one shared voltage reference, against three parts for every 3-phase-oriented alternative. It is **not selected.** Selection comes from the benchmark, scored against weights fixed before any data exists.

Four zero-cost checks come first, tracked in [12](12_progress_tracker.md) §4: evaluation-board availability (gating), per-channel reactive power (a possible ECR against PRD-001), written lifecycle status, and whether an accessible EMS can assemble and inspect QFN at prototype volume.

## Working rules

The full set is [00_execution_principles.md](00_execution_principles.md). The five that get tested most often:

- **R1** Never build before validating. If you can't state what the build will prove, you're not ready to build it.
- **R5** Every feature answers *who benefits, how much, how measured* — or it isn't a feature.
- **R11** Every assumption must eventually become evidence. Tracked in [12](12_progress_tracker.md) §8.
- **R16** No hardware change without an ADR. "It would be better if…" is not grounds; a critical engineering or safety problem is.
- **R19** Safety gates have no waiver path. Schedule pressure is not an input to that decision.

**Measure before you claim. Validate before you build. Park anything that does not ship V1.**
