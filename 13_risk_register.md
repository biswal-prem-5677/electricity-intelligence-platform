# 13 — Risk Register

**Document ID:** EIP-13 · **Version:** 1.0 · **Status:** Living document — review at every gate · **Owner:** Founder / Execution lead
**Supersedes:** Prebuild Governance, Compliance and Risk Plan (risk sections)

---

## 1. Scale

**Likelihood / Impact:** L = low · M = medium · H = high.
**Release blocker** means V1 cannot ship while the risk is open and unmitigated.

## 2. Register

| ID | Risk | L | I | Mitigation | Owner | Gate | Blocker |
|---|---|---|---|---|---|---|---|
| **R-01** | Cloud cost explodes from always-on 1-second telemetry | M | H | D-004: bundled frames + session-gated live stream; broker metrics prove zero unsessioned 1 s traffic | BE | G3 | ✅ |
| **R-02** | Certification scope and cost turn out larger than assumed | M | H | Classification opinion obtained in **Phase 0**, before any tooling spend; ≤₹10 L path is a Gate 0 criterion | PM | G0 | ✅ |
| **R-03** | Metering accuracy target unreachable with low-cost split-core CTs | H | H | Two-tier spec (D-003); CT characterisation across ≥20 samples at Phase 1; AFE benchmark includes low-current phase-error correction | HW | G5 | ✅ |
| **R-04** | DVT metrology campaign fails HWR-003a | M | H | Progressive accuracy thresholds at each gate (±5% G0 → ±2.5% G1 → ±3% post-cal G2 → ±2.0% G5) so failure surfaces early and cheaply | HW | G5 | ✅ |
| **R-05** | Safety incident during installation | L | H | No relay (F6); hazardous domain minimised (F5); no-waiver hazard gate; trained contracted electricians only; **never a DIY guide**; liability insurance before first install | HW, PM | G5 | ✅ |
| **R-06** | Device identity cloned or credentials extracted | L | H | Secure element (F8), secure boot, signed OTA with anti-rollback, mutual TLS, clone test in the security round | FW | G4 | ✅ |
| **R-07** | Malicious or bad OTA bricks a fleet | L | H | Dual slots, trial boot, automatic rollback, cohort staging with a health stop rule and a manual stop | FW, BE | G3 | ✅ |
| **R-08** | Data-privacy obligation missed (DPDP Rules 2025) | M | M | India-region storage as a control; consent, export and deletion workflows built and tested; obligations reassessed against the actual pilot date | BE, PM | G4 | ✅ |
| **R-09** | Supply chain: single-source critical part or long lead time | M | M | Second source or a logged accepted risk for every critical part is a Gate 1 criterion; lifecycle status checked at selection | HW | G1 | |
| **R-10** | Overclaiming in marketing creates legal and trust exposure | M | M | HWR-007 claim-review checklist; "99% accurate" prohibited; no utility-billing claim; 100 A claim deferred until evidence exists | PM | G7 | ✅ |
| **R-11** | Commissioning too slow or too hard for ordinary electricians | M | H | Target relaxed to ≤90 min; installer flow tested with gloves in poor light; a non-author electrician must commission using only the app at Gate 4 | MOB, PM | G4 | |
| **R-12** | Consumer payback too long to sustain a business | H | H | **D-002**: beachhead pivoted to single-phase small commercial (4–12 month payback vs 29–70 for homes); verified payback is a Gate 6 criterion | PM | G6 | ✅ |
| **R-13** | **CTs do not physically fit in real Indian distribution boards** | M | H | Phase 0 survey of ≥30 DBs **before any PCB spend**; ≥70% fit is a Gate 0 criterion; re-verified with the selected part at Gate 1 and with the enclosure at Gate 2 | HW, PM | G0 | ✅ |
| **R-14** | Scope creep — AI, three-phase, relay or enterprise features re-enter V1 | H | M | Architecture freeze (D-001); governing rule; scope-change log in [12](12_progress_tracker.md) §9 requires naming what is removed | PM | all | |
| **R-15** | Per-CT calibration conflicts with field CT replaceability | M | M | MET-006 defines three allowed paths; the path is chosen and costed at Gate 1; matched pre-characterised spares preferred | HW | G1 | |
| **R-16** | Firmware storage budget fails; 7-day retention impossible | L | M | Packed binary journal (FWR-010) with the budget calculated in [05](05_firmware_srs.md) §5; 16 MB flash minimum is a component requirement, not a preference | FW | G3 | |
| **R-17** | Rules engine produces embarrassing or wrong insights | M | M | Deterministic rules only, no ML in V1; every insight carries evidence, rule version and a stated limitation; suppressed when calibration or clock quality is invalid; <10% "this is wrong" rate is a pilot criterion | BE | G6 | |
| **R-18** | Key-person dependency on the hardware/metrology engineer | H | H | Design decisions recorded in documents rather than in one head; independent reviews at schematic, layout and isolation; DVP&R evidence stored centrally | PM | all | |
| **R-19** | Runway exhausted before Gate 6 evidence exists | M | H | Cheapest killing experiments run first (Phase 0 = ₹1.5–4 L); raise against Gate 1 and Gate 5 evidence; ≥6-month buffer beyond the planned pilot date | PM | all | |
| **R-20** | A larger competitor enters the Indian small-commercial segment | M | M | Speed to a verified-payback pilot is the defence; installed base and measured ground truth are the moat, not features | PM | — | |

## 3. Risks that were deliberately eliminated by design

| Eliminated risk | How |
|---|---|
| Load-switching failure, contact weld, unintended disconnection | **F6 — no relay anywhere.** Verified by schematic, BOM and netlist review |
| Radio authorisation cost and schedule (₹2–6 L, months) | **F7 — pre-certified module with valid Indian ETA** |
| 120° phase error across branches | **F4 + site criteria** — single-phase only; three-phase is a separate hardware platform ([future/Three_Phase.md](future/Three_Phase.md)) |
| Customer touching hazardous potential | **F5 — every customer-accessible interface is SELV** |
| ML model failure in front of a paying customer | **No AI in V1.** Deterministic rules only ([future/AI_V2.md](future/AI_V2.md)) |

**This is the most valuable table in the register.** Each row is a risk that no longer needs managing because a decision removed it.

## 4. Review cadence

Reviewed at every gate and whenever a new risk is identified. A risk is closed only with written evidence. **A risk marked "release blocker" cannot be closed by schedule pressure** — only by mitigation or by a logged decision from a named approver accepting it.
