# IMPL-01 — Energy Metering AFE Evaluation

**Phase:** 1 — Component Engineering · **Closes:** OPEN-1 · **Gate:** 🚦 G1 · **Owner:** Hardware
**Status:** Evaluation specification — no part is selected until §9 evidence exists
**Architecture:** frozen. This artifact selects a part *within* the frozen architecture; it does not revisit it.

---

## 1. The decision and why it is first

The metering AFE is the **first irreversible engineering decision**. It determines:

- how many ICs sit on the board, and therefore board area, cost and SPI topology
- whether per-channel phase-error correction exists at all — which decides whether HWR-003a (±2.0%) is reachable
- what the firmware metrology driver looks like (Phase 5 cannot start without it)
- the power tree's analogue rails (Phase 2 cannot complete without it)

Choose wrong and the correction is a full schematic and PCB respin: **₹4–6 lakh and 10–14 weeks.** Everything downstream in Phases 2–4 is blocked until this closes.

## 2. Requirements the part must satisfy

Derived from frozen documents. No new requirements are introduced here.

| # | Requirement | Source | Type |
|---|---|---|---|
| A1 | **9 current channels** — 1 service + 8 branch | F2 | Hard |
| A2 | **1 voltage channel**, shared across all current channels | F4 | Hard |
| A3 | Single-phase, 230 V / 50 Hz, 63 A nominal service | HWR-001 | Hard |
| A4 | Per-channel **gain and phase** calibration in the part | MET-001, HWR-003a | Hard |
| A5 | System active-energy error ≤ ±2.0% after calibration, full envelope | HWR-003a | Hard |
| A6 | Usable accuracy at **low current** — the branch case, not the service case | R-03 | Hard |
| A7 | Active power, apparent power, **reactive power, power factor**, RMS current, RMS voltage, frequency, per channel | PRD-001, 06 §3 frame | Hard — **see §6** |
| A8 | SPI throughput sufficient for 9 channels at the required aggregation rate | FWR-007, HWR-008 | Hard |
| A9 | 3.3 V SELV operation, no hazardous-domain content | F5 | Hard |
| A10 | India availability, lead time ≤12 weeks, **active lifecycle**, second source or a logged accepted risk | R-09 | Hard |
| A11 | Alias control provable at DVT | HWR-008 | Hard |

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

**Every additional device multiplies four costs:** unit cost, board area, calibration time per unit in production, and firmware complexity in synchronising energy-accumulation windows across devices.

**ATM90E32AS at exactly 9 channels with zero spare is a specific trap** — one damaged channel during assembly scraps the board, and there is no headroom for the V1.1 neutral/earth-imbalance channel that [future/V2.md](../future/V2.md) lists as the leading next candidate.

## 4. Candidates

### 4.1 ADE7816 — Analog Devices · **leading candidate**

Six current channels and one voltage channel in a single package, purpose-built for exactly this topology: one voltage reference, many branch currents.

**For:** two devices cover 9 channels with 3 spare · matches F4 conceptually — the part assumes a shared voltage reference · fewest voltage paths to match · lowest per-channel cost of the multi-channel parts · per-channel calibration registers.

**Against:** **feature set must be confirmed (§6)** · older part, **lifecycle status is a real risk (A10)** · single-source — Analog Devices only, no drop-in second source exists · 6-channel granularity means the second device runs 3 channels idle.

**Verdict:** benchmark first. Selection hinges entirely on §6.

### 4.2 ADE9000 / ADE9078 — Analog Devices

Higher-performance polyphase family with waveform buffers and richer power-quality measurement.

**For:** strongest metrology of the shortlist · genuine power-quality capability supporting M10 / CLR-009 at low incremental effort · waveform buffer helps debug at EVT · well-supported, current parts.

**Against:** three devices for 9 channels · designed for 3-phase, so a large share of silicon goes unused in a single-phase product · highest unit cost · three voltage paths to match.

**Verdict:** the fallback if ADE7816 fails §6. Costs more, but retires accuracy risk.

### 4.3 ATM90E32AS / ATM90E36A — Microchip

The previous baseline in the superseded documentation.

**For:** widely used, good India availability, mature reference designs, competitive cost.

**Against:** 3 or 4 current channels means 3 devices · ATM90E32AS gives **zero spare channels** · 3-phase-oriented, so two-thirds of the voltage channels are wasted · three sets of coefficients to manage.

**Verdict:** benchmark as the incumbent for cost comparison. Not favoured on architecture fit.

### 4.4 STPM34 — STMicroelectronics

**For:** good 24-bit sigma-delta front end.

**Against:** **five devices** for 9 channels. Board area, cost, calibration time and firmware complexity all become unreasonable.

**Verdict:** rejected on channel arithmetic. Not benchmarked.

### 4.5 ADE9153A — Analog Devices

Single-phase part with autocalibration, which is genuinely attractive against MET-006 and production calibration cost.

**Against:** one current channel. Nine devices is absurd.

**Verdict:** rejected for the branch array. **Retain one narrow question:** could a single ADE9153A serve the *service* channel while an ADE7816 pair serves the 8 branches, buying autocalibration where accuracy matters most? This adds a third device, a second calibration model and a second firmware driver — **provisional answer: no.** Record as evaluated and rejected unless the benchmark shows service-channel accuracy is the binding constraint.

### 4.6 MCU ADC + discrete AFE

**Verdict: rejected.** Building metrology from ADCs means owning anti-aliasing, calibration mathematics, temperature stability and phase correction with a 3–5 person team and no metrology heritage. **Do not build your own metrology** ([09](../09_component_selection.md) §5). Reconsider only if every purpose-built part fails, which would itself be an ECR.

## 5. Trade-off summary

| Criterion | Weight | ADE7816 | ADE9000/78 | ATM90E32AS |
|---|---|---|---|---|
| Channel fit (devices for 9 I) | High | ✅ 2 | ⚠ 3 | ⚠ 3, zero spare |
| Feature completeness (A7) | High | ❓ **§6** | ✅ | ✅ |
| Low-current accuracy (A6) | High | ❓ benchmark | ✅ expected best | ❓ benchmark |
| Per-channel cost | High | ✅ | ❌ | ⚠ |
| Lifecycle risk (A10) | High | ❌ **verify** | ✅ | ✅ |
| Second source | Medium | ❌ none | ❌ none | ❌ none |
| Firmware complexity | Medium | ✅ 2 drivers' worth | ⚠ 3 | ⚠ 3 |
| Power-quality support (M10) | Medium | ❓ | ✅ | ⚠ |
| Board area | Medium | ✅ | ❌ | ⚠ |
| Production calibration time | Medium | ✅ | ⚠ | ⚠ |

**No candidate has a second source.** This is a structural feature of the metering-IC market, not an oversight. Under A10 it must be logged as an accepted risk with a named approver, and the mitigation is a **pin-and-function-compatible fallback design study**, not a drop-in part.

## 6. ⚠ The blocking technical question — potential ECR

**Requirement A7 lists reactive power (VAR) and power factor per channel.** The bundled telemetry frame in [06](../06_cloud_backend_srs.md) §3 carries `reactivePowerVAR` and `powerFactor` for every channel.

**The ADE7816 must be confirmed from its datasheet to provide reactive energy/power per channel.** If it provides only **active and apparent** energy, three options exist, in order of preference:

| Option | Method | Cost | Consequence |
|---|---|---|---|
| **1** | Derive PF = P/S and \|Q\| = √(S²−P²) in firmware | Free | **Loses the sign** — no leading/lagging distinction. Acceptable for V1 display and rules; must be documented as a limitation and the app must not imply sign |
| **2** | Select ADE9000/9078 instead | +₹150–350/unit, 3 devices | Full quadrant information; higher cost and area |
| **3** | Amend PRD-001 to drop signed reactive power from V1 | Free | **Requires an ECR** — it is a requirement change, not an implementation choice |

**Do not resolve this by quietly implementing option 1.** If the datasheet confirms the gap, raise an ECR per [00](../00_execution_principles.md) §VII naming the affected requirements (PRD-001, CLR-002, the 06 §3 frame schema, MOB-003) and the chosen correction.

**Second verification item:** confirm ADE7816 lifecycle status with Analog Devices or an authorised distributor **in writing** before committing. An obsolescence notice after tooling is a project-level event, not a sourcing inconvenience.

## 7. Cost

Indicative only. **Every figure is an assumption until a distributor quote replaces it (R11).**

| Option | Devices | Est. ₹/device @1k | Est. ₹/board | Per current channel |
|---|---|---|---|---|
| 2 × ADE7816 | 2 | 300–550 | **600–1,100** | **₹67–122** |
| 3 × ADE9078 | 3 | 350–650 | 1,050–1,950 | ₹117–217 |
| 3 × ADE9000 | 3 | 500–900 | 1,500–2,700 | ₹167–300 |
| 3 × ATM90E32AS | 3 | 250–450 | 750–1,350 | ₹83–150 |

**Context:** the 9-CT stack is ₹1,350–2,700 ([10](../10_bom_and_cost.md) §2). The AFE is the second-largest line and the difference between best and worst option here is roughly **₹900–1,600 per unit** — at 1,000 units, ₹9–16 lakh. That is more than the entire Phase 1 budget, which is why this evaluation is worth doing properly rather than by datasheet reading.

**Evaluation cost:** ₹1.5 lakh (Phase 1 task 1.1) — eval boards, CT samples, load bank access, reference meter hire.

## 8. Engineering effort

| Activity | Effort | Note |
|---|---|---|
| Datasheet review + §6 resolution | 3 days | **Do this first.** It may eliminate a candidate for free |
| Lifecycle + India availability enquiries | 2 days | Written responses only |
| Eval board procurement | 1–3 weeks lead | Order all candidates simultaneously |
| Bring-up per candidate | 5–8 days | SPI, register map, raw readings |
| Calibration implementation per candidate | 4–6 days | Per-channel gain and phase |
| Benchmark execution | 8–10 days | The §9 matrix, all candidates |
| Analysis and selection memo | 3 days | |
| **Total** | **5 weeks, 1 hardware engineer** | Matches Phase 1 task 1.1 |

Benchmark firmware is throwaway. **Do not build the production metrology driver during evaluation** — it biases the result toward whichever part was implemented first.

## 9. Validation method — the benchmark that closes this decision

**Selection is made from measured data, not datasheet reading (R1, R13).**

**Setup:** programmable AC source or stable mains + reference clamp meter or traceable reference meter with stated uncertainty ≥4× better than ±2.0% · resistive and inductive load bank · ≥3 CT samples of the leading candidate CT · same CTs and same voltage reference across all candidates, so the AFE is the only variable.

**Matrix, per candidate:**

| Dimension | Points |
|---|---|
| Current | 6 points across the branch range, **including ≤2% of CT rating** |
| Power factor | 1.0 · 0.8 lagging · 0.5 lagging |
| Voltage | 230 V nominal, plus 180 V and 260 V at one current point |
| Channels | All channels exercised; ≥1 cross-device pair on multi-device options |
| Repeats | 3 CT samples × 3 runs, to expose unit-to-unit and repeatability spread |

**Recorded per candidate:**

1. Active-energy error vs reference at every point, **pre- and post-calibration**
2. Low-current behaviour — the number that most often decides this
3. Phase-error correction range and resolution actually achievable
4. Channel-to-channel consistency within one device
5. **Device-to-device consistency across the shared voltage reference** — the failure mode unique to multi-device options
6. SPI transaction time for a full 9-channel read at the target rate
7. Register-map and driver complexity, as an effort estimate for Phase 5
8. Measured quiescent current and thermal rise

**Pass criteria to reach Gate 1:**

- [ ] Post-calibration error **≤ ±2.5%** at mid-range across all 9 channels (proving ±2.0% is reachable at DVT after production calibration)
- [ ] Low-current point measured and **documented**, whatever it shows — a poor number that is known is acceptable; an unmeasured one is not
- [ ] Device-to-device spread quantified for multi-device options
- [ ] Full 9-channel read completes within the aggregation budget with ≥3× headroom
- [ ] §6 resolved, with an ECR raised if a requirement changes
- [ ] Lifecycle status confirmed **in writing**
- [ ] Selection memo with a named approver, recorded in [14](../14_decision_log.md)

## 10. Output

On completion this artifact is updated with: selected part number, package, distributor, unit cost at 1k and 10k, lead time, written lifecycle statement, accepted-risk record for the absent second source, the benchmark report reference, and the approver's name.

**Until then, ADE7816 is the leading candidate and nothing more.** OPEN-1 stays open, [12](../12_progress_tracker.md) shows *Metering AFE selection — Evidence 🟡*, and Phases 2–4 do not start.

---

**Next artifact:** IMPL-02 — CT characterisation ([09](../09_component_selection.md) §3, Phase 1 task 1.2). It runs **in parallel**, not after: the CT and the AFE are jointly responsible for A5/A6, and neither can be judged alone.
