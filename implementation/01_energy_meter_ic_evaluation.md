# IMPL-01 — Energy Metering AFE Evaluation

**Phase:** 1 — Component Engineering · **Closes:** OPEN-1 · **Gate:** 🚦 G1 · **Owner:** Hardware
**Version:** 1.1 — revised after design review · **Status:** Evaluation specification — no part is selected until §12 evidence exists
**Execution:** [IMPL-01A — Benchmark Plan](01A_afe_benchmark_plan.md) · **Architecture:** frozen. This artifact selects a part *within* the frozen architecture; it does not revisit it.

---

## 1. The decision and why it is first

The metering AFE is the **first irreversible engineering decision**. It determines:

- how many ICs sit on the board, and therefore board area, cost and SPI topology
- whether per-channel phase-error correction exists at all — which decides whether HWR-003a (±2.0%) is reachable
- what the firmware metrology driver looks like (Phase 5 cannot start without it)
- the power tree's analogue rails and part of the HWR-011 self-consumption budget (Phase 2 cannot complete without it)
- the assembly process, via package type (§8) — which sets what an EMS can build and what yield to expect

Choose wrong and the correction is a full schematic and PCB respin: **₹4–6 lakh and 10–14 weeks.** Everything downstream in Phases 2–4 is blocked until this closes.

## 2. Requirements the part must satisfy

Derived from frozen documents. **A12–A16 were added in v1.1**; A15 and A16 rest on HWR-011 and HWR-012, which were gaps in the SRS closed by [D-017](../14_decision_log.md#d-017).

| # | Requirement | Source | Type |
|---|---|---|---|
| A1 | **9 current channels** — 1 service + 8 branch | F2 | Hard |
| A2 | **1 voltage channel**, shared across all current channels | F4 | Hard |
| A3 | Single-phase, 230 V / 50 Hz, 63 A nominal service | HWR-001 | Hard |
| A4 | Per-channel **gain and phase** calibration in the part | MET-001, HWR-003a | Hard |
| A5 | System active-energy error ≤ ±2.0% after calibration, full envelope | HWR-003a | Hard |
| A6 | Usable accuracy at **low current** — the branch case, not the service case | R-03 | Hard |
| A7 | Active power, apparent power, **reactive power, power factor**, RMS current, RMS voltage, frequency, per channel | PRD-001, 06 §3 frame | Hard — **see §9** |
| A8 | SPI throughput sufficient for 9 channels at the required aggregation rate | FWR-007, HWR-008 | Hard |
| A9 | 3.3 V SELV operation, no hazardous-domain content | F5 | Hard |
| A10 | India availability, lead time ≤12 weeks, **active lifecycle**, second source or a logged accepted risk | R-09 | Hard |
| A11 | Alias control provable at DVT | HWR-008 | Hard |
| **A12** | **A purchasable evaluation board exists**, in stock, deliverable to India in ≤4 weeks | *This evaluation existing at all* | **Hard — gating** |
| **A13** | Package is assemblable and **inspectable** by a small-volume Indian EMS | Manufacturability, §8 | Hard |
| **A14** | Documented register map, errata, application notes and reference firmware sufficient to bring up in ≤8 days | Phase 5 effort | Soft — scored |
| **A15** | Supply current contributes within the **≤3.0 W typical** device budget | **HWR-011** | Hard |
| **A16** | Specified across **0 to +50 °C** with characterised accuracy drift | **HWR-012** | Hard |

> **A12 is gating, not scored.** A part with no obtainable evaluation board cannot be benchmarked, and a part that is not benchmarked cannot be selected (R13). Confirm stock **before** ordering anything else — it is the cheapest way to shorten this list.

## 3. The constraint that decides this: channel arithmetic

**This is where the previous documentation was wrong.** The earlier shortlist contained only 3–4-channel parts for a 9-channel product, which silently implied three ICs, three SPI chip selects, three sets of calibration coefficients and three voltage-channel paths that must match each other.

| Candidate | Current ch. | Voltage ch. | Devices for 9 I | Spare ch. | Voltage paths to match |
|---|---|---|---|---|---|
| **ADE7816** | **6** | 1 | **2** | 3 | 2 |
| ADE9000 / ADE9078 | 4 (3 + neutral) | 3 | 3 | 3 | 3 |
| ATM90E36A | 4 | 3 | 3 | 3 | 3 |
| ATM90E32AS | 3 | 3 | 3 | **0** | 3 |
| STPM34 | 2 | 2 | 5 | 1 | 5 |
| ADE9153A | 1 | 1 | 9 | 0 | 9 |
| MCU ADC + discrete AFE | n/a | n/a | 1 | n/a | 1 |

**Every additional device multiplies five costs:** unit cost, board area, calibration time per unit in production, firmware complexity in synchronising energy-accumulation windows, and **solder-joint count, which is where yield is lost** (§8).

**ATM90E32AS at exactly 9 channels with zero spare is a specific trap** — one damaged channel during assembly scraps the board, and there is no headroom for the V1.1 neutral/earth-imbalance channel that [future/V2.md](../future/V2.md) lists as the leading next candidate.

## 4. Candidates

### 4.1 ADE7816 — Analog Devices · **preferred hypothesis**

Six current channels and one voltage channel in a single package, purpose-built for exactly this topology: one voltage reference, many branch currents.

**For:** two devices cover 9 channels with 3 spare · matches F4 conceptually — the part assumes a shared voltage reference · fewest voltage paths to match · lowest per-channel cost of the multi-channel parts · per-channel calibration registers · fewest solder joints of the multi-device options.

**Against:** **feature set must be confirmed (§9)** · older part, **lifecycle status is a real risk (§6)** · single-source · 6-channel granularity means the second device runs 3 channels idle · **LFCSP/QFN package is the hardest of the shortlist to inspect and rework (§8)**.

**Verdict:** benchmark. Selection hinges on §9 and §8.

### 4.2 ADE9000 / ADE9078 — Analog Devices

Higher-performance polyphase family with waveform buffers and richer power-quality measurement.

**For:** strongest metrology of the shortlist · genuine power-quality capability supporting M10 / CLR-009 at low incremental effort · waveform buffer is a real debugging asset at EVT · current parts with active support · strongest software ecosystem of the three.

**Against:** three devices for 9 channels · designed for 3-phase, so a large share of silicon and two-thirds of the voltage channels go unused · highest unit cost · three voltage paths to match · highest supply-current contribution to HWR-011.

**Verdict:** the fallback if ADE7816 fails §9. Costs more, retires accuracy risk.

### 4.3 ATM90E32AS / ATM90E36A — Microchip

The previous baseline in the superseded documentation.

**For:** widely used, good India availability, mature reference designs, competitive cost · Microchip's documentation and support ecosystem is a genuine advantage · **likely the most assemblable package of the shortlist (§8)**.

**Against:** 3 or 4 current channels means 3 devices · ATM90E32AS gives **zero spare channels** · 3-phase-oriented · three sets of coefficients to manage.

**Verdict:** benchmark. Loses on architecture fit, **may win on manufacturability** — which is exactly why it stays in.

### 4.4 STPM34 — STMicroelectronics

**For:** good 24-bit sigma-delta front end.

**Against:** **five devices** for 9 channels. Board area, cost, calibration time, solder-joint count and firmware complexity all become unreasonable.

**Verdict:** rejected on channel arithmetic. Not benchmarked.

### 4.5 ADE9153A — Analog Devices

Single-phase part with autocalibration, genuinely attractive against MET-006 and production calibration cost.

**Against:** one current channel. Nine devices is absurd.

**Verdict:** rejected for the branch array. **One narrow question retained:** could a single ADE9153A serve the *service* channel while an ADE7816 pair serves the 8 branches, buying autocalibration where accuracy matters most? Third device, second calibration model, second firmware driver — **provisional answer: no.** Recorded as evaluated and rejected unless the benchmark shows service-channel accuracy is the binding constraint.

### 4.6 MCU ADC + discrete AFE

**Verdict: rejected.** Building metrology from ADCs means owning anti-aliasing, calibration mathematics, temperature stability and phase correction with a 3–5 person team and no metrology heritage ([09](../09_component_selection.md) §5). Reconsider only if every purpose-built part fails, which would itself be an ECR.

## 5. Scoring — weights fixed before any measurement

**These weights are locked now, before data exists.** Scoring criteria chosen after seeing results are how a team confirms the answer it already liked. Changing a weight after benchmarking begins requires a logged note in [14](../14_decision_log.md) stating who changed it and why.

| # | Criterion | Weight | Scored from |
|---|---|---|---|
| S1 | Post-calibration accuracy across the envelope | **25** | Benchmark |
| S2 | Low-current accuracy | **15** | Benchmark |
| S3 | Channel fit — devices needed, spare channels | **12** | §3, fixed |
| S4 | Feature completeness (A7) | **10** | Datasheet + benchmark |
| S5 | Supply-chain health (§6 composite) | **10** | §6, evidence-based |
| S6 | Manufacturability — package, yield (§8) | **8** | §8 |
| S7 | Per-channel BOM cost | **7** | Quotes |
| S8 | Developer ecosystem (§7) | **6** | Measured bring-up time |
| S9 | Self-consumption contribution (A15) | **4** | Benchmark |
| S10 | Temperature drift (A16) | **3** | Benchmark |
| | **Total** | **100** | |

**Accuracy is 40 of 100 because accuracy is the product.** Supply chain is 10 because a perfect part you cannot buy scores zero on everything else in practice — see §6.

## 6. Supply-chain qualification — mandatory, before the benchmark

**More hardware projects die here than in firmware.** Each candidate needs this table completed with sourced evidence, not recollection. Blank cells are disqualifying at Gate 1.

| Field | ADE7816 | ADE9078 | ATM90E32AS |
|---|---|---|---|
| Manufacturer | Analog Devices | Analog Devices | Microchip |
| Full orderable part number (incl. package/temp suffix) | | | |
| Authorised distributors (Mouser/DigiKey/Arrow/element14) | | | |
| **India distributor / importer** | | | |
| **Evaluation board P/N — in stock? (A12)** | | | |
| Stock on hand across distributors | | | |
| 12-month stock history | | | |
| MOQ | | | |
| Lead time, quoted in writing | | | |
| **Lifecycle status — Active / NRND / EOL announced** | | | |
| Written lifecycle statement obtained (Y/N, date) | | | |
| Package variants available | | | |
| Alternate/second source | | | |
| **Lifecycle score (below)** | | | |

**Lifecycle scoring — repeatable, not a paragraph:**

| Status | Score |
|---|---|
| Active production, no end-of-life signal, appears in current selector guides | **10** |
| Active but mature — no successor announced, thinning distributor stock | **7** |
| Not Recommended for New Designs (NRND) | **3** |
| End-of-life announced, last-time-buy open | **1** |
| Obsolete | **0** |

**A score below 7 requires a documented last-time-buy plan or a named migration path before selection.** The ADE7816's age makes this the single most likely place this evaluation ends badly — an obsolescence notice arriving after tooling is a project-level event, not a sourcing inconvenience.

**No candidate in this market has a true second source.** That is structural, not an oversight. Under A10 it is logged as an accepted risk with a named approver (A-8 in [12](../12_progress_tracker.md) §8), and the mitigation is a **pin-and-function-compatible fallback design study**, not a drop-in part.

## 7. Developer ecosystem — scored, because it costs weeks

Two parts with identical metrology can differ by a month of engineering time.

| Factor | What to check |
|---|---|
| Evaluation board | Exists, purchasable, **in stock** (A12) — gating |
| Eval GUI / host software | Runs on the team's machines without a licence hunt |
| Register map | Complete, with worked calibration examples |
| Reference firmware | Exists; licence permits commercial use |
| Application notes | Calibration, layout, anti-aliasing, CT interfacing |
| Errata | Published and current. *A part with no errata sheet is not a part with no errata* |
| Support | Reachable FAE or India-based applications support |
| Community | Prior art from other multi-channel branch-monitoring designs |

**Scored objectively:** S8 is set from **measured hours to first credible reading** on the evaluation board, logged per candidate in IMPL-01A. Not from an impression of the documentation.

## 8. Package and manufacturability — where the preferred hypothesis is weakest

**Confirm every package below from the datasheet before scoring** — these are the expected packages, not verified ones.

| Candidate | Expected package | Leads | Inspectable? | Hand-reworkable? |
|---|---|---|---|---|
| ADE7816 | LFCSP-40, exposed pad | No leads (QFN-type) | ❌ Not optically — X-ray or destructive | ⚠ Hot-air only, skilled |
| ADE9000 / ADE9078 | LFCSP-40, exposed pad | No leads | ❌ | ⚠ |
| ATM90E32AS | LQFP-48 | **Leaded** | ✅ Optically | ✅ |

**This is a real trade-off and it cuts against the preferred hypothesis.** QFN-class packages are standard in volume production and entirely normal for an EMS with reflow and AOI. But for a startup at 20–50 boards, leaded packages mean joints an engineer can *see*, probe and fix at 2 a.m. before a demo. A QFN with an exposed thermal pad requires a correct stencil aperture, controlled reflow profile and — for real confidence — X-ray.

**Decision rule:** package does not veto the part. **It sets the assembly requirement**, and the assembly requirement must be confirmed with the chosen EMS **before** Gate 1, not discovered at EVT. If no accessible EMS can build and inspect QFN reliably at prototype volume, that is a genuine finding and the ATM90E32AS's leaded package becomes materially more valuable than its channel arithmetic is bad.

**Yield model** — first-pass yield estimated from joint count, since that is what dominates at prototype volume:

| Option | Metering ICs | Approx. IC joints | Inspection | Expected first-pass yield |
|---|---|---|---|---|
| 2 × ADE7816 | 2 | ~80 + 2 thermal pads | X-ray needed | **92–97%** |
| 3 × ADE9078 | 3 | ~120 + 3 thermal pads | X-ray needed | 90–96% |
| 3 × ATM90E32AS | 3 | ~144 | Optical | **94–98%** |

Estimates, to be replaced by the first EVT lot's measured yield. The point is not the numbers — it is that **a 5% yield difference at ₹14,000 BOM is ₹700/unit of hidden cost**, comparable to the entire AFE line item.

**PCB constraints bounding this choice** (settled in Phase 2, bounded here): the board must fit the approved DB types from the Phase 0 survey (HWR-010) — assume **≤160 × 100 mm and ≤32 mm height** as a working envelope until the survey closes, with 9 CT connectors, the isolated voltage path and the hazardous/SELV separation all inside it. Three 7 × 7 mm ICs plus three voltage front-ends consume noticeably more of that budget than two 6 × 6 mm ICs plus one.

## 9. ⚠ The blocking technical question — potential ECR

**Requirement A7 lists reactive power (VAR) and power factor per channel.** The bundled telemetry frame in [06](../06_cloud_backend_srs.md) §3 carries `reactivePowerVAR` and `powerFactor` for every channel.

**The ADE7816 must be confirmed from its datasheet to provide reactive energy/power per channel.** If it provides only **active and apparent** energy, three options exist, in order of preference:

| Option | Method | Cost | Consequence |
|---|---|---|---|
| **1** | Derive PF = P/S and \|Q\| = √(S²−P²) in firmware | Free | **Loses the sign** — no leading/lagging distinction. Acceptable for V1 display and rules; must be documented as a limitation and the app must not imply sign |
| **2** | Select ADE9000/9078 instead | +₹150–350/unit, 3 devices | Full quadrant information; higher cost and area |
| **3** | Amend PRD-001 to drop signed reactive power from V1 | Free | **Requires an ECR** — a requirement change, not an implementation choice |

**Do not resolve this by quietly implementing option 1.** If the datasheet confirms the gap, raise an ECR per [00](../00_execution_principles.md) §VII naming the affected requirements (PRD-001, CLR-002, the 06 §3 frame schema, MOB-003) and the chosen correction.

## 10. Cost

Indicative only. **Every figure is an assumption until a distributor quote replaces it (R11).**

| Option | Devices | Est. ₹/device @1k | Est. ₹/board | Per current channel | Yield-adjusted |
|---|---|---|---|---|---|
| 2 × ADE7816 | 2 | 300–550 | **600–1,100** | **₹67–122** | +₹350–700 |
| 3 × ADE9078 | 3 | 350–650 | 1,050–1,950 | ₹117–217 | +₹450–900 |
| 3 × ADE9000 | 3 | 500–900 | 1,500–2,700 | ₹167–300 | +₹450–900 |
| 3 × ATM90E32AS | 3 | 250–450 | 750–1,350 | ₹83–150 | +₹250–560 |

*Yield-adjusted = scrap and rework cost per good board at the §8 estimates, on a ₹14,000 BOM.*

**Context:** the 9-CT stack is ₹1,350–2,700 ([10](../10_bom_and_cost.md) §2). The AFE is the second-largest line, and best-to-worst here is roughly **₹900–1,600 per unit** — at 1,000 units, ₹9–16 lakh. That is more than the entire Phase 1 budget, which is why this is worth benchmarking rather than deciding from datasheets.

**Evaluation cost:** ₹1.5 lakh (Phase 1 task 1.1). Test-equipment cost is specified and costed in [IMPL-01A](01A_afe_benchmark_plan.md) §3, because it is shared with CT characterisation and with every later accuracy test — it is bench capex, not a line item of this decision.

## 11. Engineering effort

| Activity | Effort | Note |
|---|---|---|
| §6 supply-chain qualification + §9 datasheet resolution | 4 days | **Do this first.** Zero cost, and it may eliminate a candidate before anything ships |
| A12 eval-board availability check | 0.5 day | Gating. Do it in the first hour |
| EMS consultation on QFN capability (§8) | 1 day | Can run in parallel |
| Eval board procurement | 1–3 weeks lead | Order all surviving candidates simultaneously |
| Bring-up per candidate | 5–8 days | Logged as the S8 score |
| Calibration implementation per candidate | 4–6 days | Per-channel gain and phase |
| Benchmark execution ([IMPL-01A](01A_afe_benchmark_plan.md)) | 8–10 days | |
| Analysis and selection memo | 3 days | |
| **Total** | **5–6 weeks, 1 hardware engineer** | |

Benchmark firmware is throwaway. **Do not build the production metrology driver during evaluation** — it biases the result toward whichever part was implemented first.

## 12. What closes this decision

Selection is made from measured data (R1, R13). The procedure, equipment, wiring, data format and report template are in **[IMPL-01A — Benchmark Plan](01A_afe_benchmark_plan.md)**, so that two engineers running it independently produce comparable results.

**Gate 1 pass criteria:**

- [ ] §6 supply-chain table complete for every benchmarked candidate, lifecycle score ≥7 or a documented migration path
- [ ] §9 resolved, with an ECR raised if a requirement changes
- [ ] §8 assembly capability confirmed with a named EMS
- [ ] Post-calibration error **≤ ±2.5%** at mid-range across all 9 channels (proving ±2.0% is reachable at DVT after production calibration)
- [ ] Low-current point measured and **documented**, whatever it shows — a poor number that is known is acceptable; an unmeasured one is not
- [ ] Device-to-device spread quantified across the shared voltage reference (F4)
- [ ] Full 9-channel read within the aggregation budget with ≥3× headroom
- [ ] Supply current and temperature drift measured against A15/A16
- [ ] §5 scorecard completed with the **pre-registered weights**
- [ ] Selection memo with a named approver, recorded in [14](../14_decision_log.md)

## 13. Output

On completion this artifact records: selected part number, package, distributor, unit cost at 1k and 10k, lead time, written lifecycle statement, accepted-risk record for the absent second source, benchmark report reference, and approver.

**Until then, ADE7816 is a preferred hypothesis awaiting benchmark validation — not the choice.** The channel arithmetic in §3 is a strong prior; it is not evidence about accuracy, supply chain or manufacturability, and §8 already identifies a dimension where it is the weakest of the three. OPEN-1 stays open, [12](../12_progress_tracker.md) shows *Metering AFE selection — Evidence 🟡*, and Phases 2–4 do not start.

---

**Next artifacts:** [IMPL-01A — Benchmark Plan](01A_afe_benchmark_plan.md), then IMPL-02 — CT characterisation ([09](../09_component_selection.md) §3). IMPL-02 runs **in parallel** with the benchmark: the CT and the AFE are jointly responsible for A5/A6, and neither can be judged alone.
