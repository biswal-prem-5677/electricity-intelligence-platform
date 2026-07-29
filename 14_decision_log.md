# 14 — Decision Log

**Document ID:** EIP-14 · **Version:** 1.0 · **Status:** Living document · **Owner:** Founder / Execution lead

---

## 1. Rules

A decision enters this log when it constrains future work. Each records: **what was decided · why · what was rejected · what it costs to reverse.** A decision is reversed only by a new logged decision with a named approver, a stated schedule impact and a stated cost impact. **"It would be better if…" is not grounds to reopen a decision. A critical engineering or safety problem is.**

---

## D-001 — Freeze the V1 architecture

**Decided:** the Circuit-8 hardware architecture in [03_hardware_architecture.md](03_hardware_architecture.md) is frozen at Phase 0 exit, including F1–F8.

**Why:** the project's failure mode was never a shortage of ideas — it was an inability to stop adding them. A frozen architecture converts an open design space into an executable plan for 3–5 people over 12–18 months.

**Rejected:** keeping the architecture open until EVT "in case something better appears."

**Cost to reverse:** depends on the element; F5 (isolation) and F6 (no relay) are the most expensive, because both change the certification scope.

---

## D-002 — Beachhead is single-phase small commercial, not premium homes

**Decided:** the V1 target customer is a single-phase small commercial site (shop, clinic, small office, restaurant, small workshop). Premium homes are a secondary market, not the launch market.

**Why:** the payback arithmetic decides this, not preference.

| Segment | Bill ₹/mo | 8% saving | System ~₹14,000 | Payback |
|---|---|---|---|---|
| Small commercial | 15,000–45,000 | 1,200–3,600 | | **4–12 months** |
| Premium home | 2,500–6,000 | 200–480 | | 29–70 months |

A 29–70 month payback is not a product; it is a hobby purchase. Small commercial also has a decision-maker who already reads the bill, a business reason to act, and tolerance for a professional installation.

**Rejected:** consumer-first launch; simultaneous consumer + commercial launch.

**Cost to reverse:** low in hardware terms — the same device serves both. High in go-to-market terms: channel, pricing and messaging are already being built around this.

**Note:** the 8% saving is an **assumption** from published behavioural-feedback literature. It must be replaced by the pilot's measured result at Gate 6.

---

## D-003 — Two-tier accuracy specification

**Decided:** the committed specification is **≤ ±2.0%** system active-energy error across the full approved envelope (HWR-003a). **≤ ±1.0%** is a stretch goal within a narrower published envelope (HWR-003b), usable only after a DVT report exists and only with the envelope printed beside it.

**Why:** low-cost split-core CTs show significant phase error below ~1% of rated current, plus real unit-to-unit gap and permeability spread. Emporia publishes ±2% and they are not careless. A blanket ±1% commitment guarantees a crisis at DVT with the product already sold on the number.

**Rejected:** ±1% across the full envelope; publishing accuracy without an envelope.

**Cost to reverse:** improving to ±1% means better CTs, tighter per-unit calibration and probably more calibration time per unit — a BOM and production-cost increase, not a firmware change.

**Consequence:** HWR-007 — every claim states topology, CT part, calibration method, envelope and reference standard. **"99% accurate" is prohibited.** It is a marketing phrase, not a metrology specification.

---

## D-004 — Telemetry is bundled and session-gated

**Decided:** all nine channels are transmitted in **one bundled frame** per interval. The continuous cloud tier is **5-minute** aggregates. The 1-second live stream runs **only while an app live session is open**, with a server-side timeout.

**Why:** the arithmetic.

- Nine separate 1-second messages: ~23.3 M messages and ~5 GB per device per month → 23 billion messages and 5 TB/month at 1,000 devices.
- One bundled 5-minute frame: ~8,640 messages and ~15 MB per device per month → **15 GB/month** at 1,000 devices.

Roughly a 2,700× difference. The live view is what customers love; always-on it is what would bankrupt the cloud bill.

**Rejected:** always-on 1-second streaming; one MQTT message per channel.

**Cost to reverse:** none technically — but the cost curve is the reason the decision exists.

**Consequences:** FWR-008, CLR-007, MOB-009, the MQTT contract in [06](06_cloud_backend_srs.md) §3, and R-01.

---

## D-005 — No AI, no ML in V1

**Decided:** V1 is **Measure → Store → Visualize → Rules.** All analytics are deterministic, explainable rules (AIR-001…008). No machine learning, no NILM, no forecasting model, no LLM assistant, no agentic anything.

**Why:** ML needs labelled data from deployed devices. We have none, and we cannot buy a substitute — public disaggregation datasets do not transfer to Indian loads and Indian wiring. Building models before the fleet exists is building on an assumption. It also removes the most likely source of an embarrassing wrong answer in front of a paying customer.

**Rejected:** shipping a forecasting or disaggregation feature in V1 as a differentiator.

**Unparking gate:** ≥100 devices with ≥90 days of quality data, a named owner, an evaluation dataset, a safety/claim review, and an acceptance gate. See [future/AI_V2.md](future/AI_V2.md) and [future/NILM.md](future/NILM.md).

---

## D-006 — Circuit-8 is the product name; Home-8 and DIN-8 are retired

**Decided:** the V1 product is **Circuit-8**. The earlier names Home-8 and DIN-8 are superseded.

**Why:** "Home-8" contradicts D-002. "DIN-8" describes a mounting method, not a product. "Circuit-8" describes exactly what it does: eight circuits.

**Cost to reverse:** trivial now, expensive after packaging and certification documents carry the name.

---

## D-007 — The "two mains CTs" wording is retired

**Decided:** the V1 topology is **one service CT plus eight branch CTs** (F2). A CT surrounds exactly one conductor, never live and neutral together (F3).

**Why:** the earlier "two mains CTs" phrasing was ambiguous and could have been read as a CT around live and neutral together, where the opposing fields cancel and the reading is invalid. On a normal single-phase service there is one live conductor to measure.

**Consequence:** F3 is an installer training point and a commissioning check (HWR-009 plausibility detection).

---

## D-008 — Three-phase is a new hardware platform, not a firmware option

**Decided:** three-phase support is out of V1 and will require a distinct hardware platform (Site-3P).

**Why:** V1 uses a single shared isolated voltage reference (F4). Branches on different phases are 120° apart from that reference, so active power and power factor are wrong for those branches. **There is no firmware workaround for a missing voltage reference.**

**Unparking gate:** ≥10 written customer requests, or ≥3 sites per month lost to phase rejection, **and** the V1 pilot gate passed. See [future/Three_Phase.md](future/Three_Phase.md).

---

## D-009 — 100 A claim deferred

**Decided:** the published service-current figure is **63 A nominal**. A 100 A maximum claim does not appear in any document, datasheet or marketing material until CT, thermal and installation evidence exists.

**Why:** the previous suite contradicted itself — one document deferred the claim while another stated 100 A as a maximum. An unsupported rating is a safety and liability exposure, not an optimistic spec.

**Closes at:** Gate 5 (OPEN-8), either with evidence or as a permanent deferral.

---

## D-010 — Documentation reduced to 15 core documents plus a future/ folder

**Decided:** the previous 41-document suite is replaced by 15 core V1 documents, a README, and a `future/` folder of 8 parked-capability documents. Nothing was deleted — non-V1 material was rescheduled.

**Why:** the constraint was never information. Documentation that does not help build, test, validate or launch V1 competes with V1 for the only genuinely scarce resource, which is engineering attention.

**Governing rule, applied throughout:** *If a document, feature or task does not directly help us build, test, validate or launch V1, it must not block V1. Move it to the Future Vision roadmap.*

---

---

## D-011 — Execution principles become constitutional

**Decided:** [00_execution_principles.md](00_execution_principles.md) is created as the project's constitution — 24 rules (R1–R24). Where any other document conflicts with it, the principles win and the other document is corrected.

**Why:** the decisions in this log are individually sound and collectively fragile. Each was made calmly; each will be tested on a day when the demo is close and the shortcut looks harmless. A rule written down in advance is the only thing that survives that day. The rules also make the *reasoning* portable — a new engineer can infer why a decision was made without reading twelve documents.

**Rejected:** leaving the principles distributed across the documents where they were first stated. They were unenforceable there because nobody knew the complete set.

**Cost to reverse:** amending a rule requires a logged decision with a named approver. That friction is the feature.

---

## D-012 — Five funding tiers replace a single project budget

**Decided:** the ₹41–94 lakh figure is reclassified as the **Commercial MVP** cost, not "the budget." Spend is structured in five tiers, each with its own go/no-go: Bench Prototype (₹0.5–2 L) → Engineering Prototype (₹3–8 L) → EVT (₹9–19 L) → DVT + Pilot (₹13–28 L) → Commercial MVP (₹15–35 L).

**Why:** a single large total makes the project look like it needs external capital to *begin*, which is false and is a common reason hardware projects never start. The first tier is self-fundable and carries the two questions that can kill the project — do the CTs fit, and can we measure at all. The tiers also separate what a competent engineer can do on a bench from what requires an EMS, a lab and someone else's money.

**Rejected:** presenting a single project cost; front-loading capital before the fatal unknowns are retired.

**Consequence:** raises are timed against **Gate 1** (measured accuracy from selected parts) and **Gate 5** (traceable metrology and safety evidence) — the two moments the project becomes materially more fundable. Recorded in [10_bom_and_cost.md](10_bom_and_cost.md) §1a and R23.

---

## D-013 — Progress is reported by evidence stage, never by percentage

**Decided:** [12_progress_tracker.md](12_progress_tracker.md) tracks every item through **Evidence → Implementation → Testing → Validation → Done**, with ✅ / 🟡 / ❌ / ⛔ per stage. Percentage completion is banned as a progress measure.

**Why:** "60% done" is meaningless on a hardware project and is actively misleading. A board 95% laid out with no isolation calculations is worth less than one 40% laid out with them signed. Stage tracking also exposes the failure mode this project is most prone to — implementation racing ahead of evidence, which is exactly what R1 exists to prevent.

**Consequence:** an assumption ledger (A-1…A-8) was added, because "Evidence ❌" needed somewhere to name *which* evidence is missing and when it closes. Recorded in R11 and R22.

---

## D-014 — Planning is frozen; the project enters Implementation Engineering

**Decided:** the 15 core documents are the **complete** planning set. No further planning documents will be written; they are now maintained, not extended. Every new file from this point must be an implementation artifact that lets someone act: select a component, start a schematic, write code, stand up a service, or run a test (R18).

**Sequence:** component selection → hardware block diagrams → electrical schematics → PCB design → firmware → cloud backend → mobile application → bench testing → field pilot.

**Why:** the bottleneck has moved. It is no longer *what should we build* — that is answered and frozen. It is *how do we build the first working unit*. Continuing to produce architecture documents past this point is the most comfortable way to avoid the harder work, and it consumes the only genuinely scarce resource, which is engineering attention.

**Rejected:** further elaboration of the specification suite; a detailed V2 plan; any documentation of parked capabilities beyond what `future/` already holds.

---

## D-015 — `15_future_roadmap.md` becomes the `future/` folder

**Decided:** the roadmap document is removed from the core numbered set and becomes [future/README.md](future/README.md), joined by staged views `V2.md`, `V3.md` and `Moonshots.md`. The core set is renumbered **00–14** and remains 15 documents.

**Why:** a "Future Roadmap" sitting in the main document list still reads as *something to keep reading*. A folder reads as *somewhere else*. The parked list is larger and more exciting than the V1 list and will pull attention away from the work that ships unless it is physically separated. This is a psychological control, and it is deliberate.

**Rejected:** keeping the roadmap in the numbered core set; deleting the parked material outright.

---

## D-016 — Planning freeze becomes enforceable (R25), and implementation phases are defined

**Decided:** R25 is added to the constitution — no new planning or architecture document may be added unless it unblocks an implementation task or resolves a **verified** engineering issue found during development. R26 is added — every completed unit of work is committed and pushed before the next begins. The implementation sequence is defined as eight phases in [00_execution_principles.md](00_execution_principles.md) §VII, mapped onto the existing gates G1–G6.

**Why:** D-014 declared planning frozen but gave the freeze no enforcement mechanism. A declaration without a test is a preference. R25 supplies the test: *name the implementation task this unblocks, or the verified issue it resolves.* Documentation creep is the most comfortable way to avoid engineering and is indistinguishable from progress right up to the moment someone asks for a part number.

**Also decided:** blockers are handled by **Engineering Change Request**, never by silent modification. An ECR states the blocker with evidence, quantifies it, proposes the *smallest* correction rather than the best redesign, names impacted frozen decisions, defines validation, and carries a named approver.

**Rejected:** an informal "we'll be disciplined about it" freeze; allowing architecture changes to be absorbed into design work without a record.

**Note on the phase list as proposed:** the source list for Phase 1 included a relay among the components to freeze. **It is excluded.** F6 and R21 prohibit any switching path, and a relay appearing in a BOM, schematic or netlist is a defect that means the review failed. No ECR was raised because no blocker exists — the omission is deliberate and load-bearing.

---

## D-017 — Self-consumption and environmental envelope become requirements (HWR-011, HWR-012)

**Decided:** [04_hardware_srs.md](04_hardware_srs.md) §2 gains **HWR-011** — device self-consumption ≤3.0 W typical at the AC input, ≤5.0 W peak during radio transmit, stretch ≤2.0 W — and **HWR-012** — operating ambient 0 to +50 °C, storage −20 to +70 °C, 5–95% RH non-condensing, with HWR-003a accuracy holding across that range.

**Why:** both were gaps found while writing [implementation/01_energy_meter_ic_evaluation.md](implementation/01_energy_meter_ic_evaluation.md), and neither could be scored against a candidate part because neither existed.

- **Self-consumption had no requirement at all.** The device is always on and is sold on reducing an electricity bill. At 3 W it draws ~26 kWh/year — roughly ₹210–265/year at commercial tariff, against a claimed saving of ₹14,400–43,200/year. The arithmetic is comfortable; the absence of a measured number was not. "Your energy saver consumes energy" will be asked by a customer, a competitor or a journalist, and the only good answer is a number with a test behind it.
- **The temperature envelope was referenced but never defined.** HWR-001 and HWR-003a both said "the approved temperature envelope" while no document stated what it was. A requirement that points at an undefined envelope is not testable, which violates R12.

**This is not an ECR.** Nothing frozen changed, no blocker was found, and F1–F8 are untouched. It is a requirement gap closed by amending an existing document, which R25 explicitly prefers over creating a new one.

**Rejected:** leaving self-consumption to be discovered at DVT — the point at which the AFE, the isolated voltage reference and the AC/DC module are already chosen and the budget is no longer allocatable.

**Consequence:** both are now scored criteria in the AFE evaluation (S9, S10) and measured in [implementation/01A_afe_benchmark_plan.md](implementation/01A_afe_benchmark_plan.md) §6.4. Requirement count rises from 85 to 87.

---

## 2. Frozen architecture decisions F1–F8

Recorded in full in [03_hardware_architecture.md](03_hardware_architecture.md) §3 and governed by D-001.

| # | Decision |
|---|---|
| F1 | Split-core CTs, one per conductor |
| F2 | 1 service CT + 8 branch CTs, single-phase only |
| F3 | A CT surrounds exactly one conductor |
| F4 | One shared, isolated voltage-reference path |
| F5 | Hazardous-domain content minimised; all service interfaces SELV |
| F6 | No load switching — no relay, contactor or triac, anywhere |
| F7 | Pre-certified Wi-Fi/BLE module with a valid Indian ETA |
| F8 | Secure element for non-exportable device identity |

## 3. Open decisions

Tracked in [12_progress_tracker.md](12_progress_tracker.md) §7 (OPEN-1…OPEN-8). They move here when closed.
