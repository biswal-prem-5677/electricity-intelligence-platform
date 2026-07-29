# IMPL-01A — Metering AFE Benchmark Plan

**Phase:** 1 — Component Engineering · **Executes:** [IMPL-01](01_energy_meter_ic_evaluation.md) · **Gate:** 🚦 G1 · **Owner:** Hardware
**Status:** Procedure — ready to execute · **Version:** 1.0

**R25 justification:** this is not a planning document. It unblocks Phase 1 task 1.1 by defining the bench, the wiring, the sequence and the data format, so that **two engineers running this independently produce comparable numbers.** Without it, "benchmark the candidates" is an instruction, not a procedure, and the result is not reviewable.

---

## 1. What this produces

One CSV dataset and one filled report template per candidate, sufficient to complete the [IMPL-01 §5](01_energy_meter_ic_evaluation.md#5-scoring--weights-fixed-before-any-measurement) scorecard and close OPEN-1.

**It does not select a part.** It produces the evidence from which a named approver selects a part.

## 2. ⚠ Safety — read before the bench is powered

This procedure involves **live 230 V mains**. It is the most hazardous work in Phase 1 and it happens before any safety review exists for the product.

| Rule | Detail |
|---|---|
| **Isolation transformer, always** | The whole bench sits behind a 1–2 kVA isolation transformer. Without it, every "ground" on the bench is a shock path and an oscilloscope ground clip is a short to earth |
| **RCD/RCBO on the bench feed** | 30 mA. Tested with its own test button at the start of every session |
| **Differential probes only** | **Never** connect a ground-referenced scope probe to a mains node. A differential probe is not optional equipment, it is the equipment |
| **Two people for live work** | One works, one watches and can cut power. No exceptions for "just checking something" |
| **One-hand rule** | Other hand in a pocket when reaching into a live area |
| **Power down to rewire** | Every CT move, every connector change. The turns trick in §5 especially — rewinding a primary loop on a live conductor is how people get hurt |
| **Enclosed load bank** | Resistive loads reach burn temperature. Guarded, on a non-combustible surface, never unattended |
| **CT secondaries never open-circuit** | An unloaded CT on a live conductor can develop dangerous voltage. Burden connected or shorted, always ([04](../04_hardware_srs.md) §4) |
| **Log book** | Date, operator, configuration, what was energised. Also the audit trail for the data |

**If the team has no member experienced with live mains work, hire that experience for the bench setup.** Budget ₹15–30k. This is cheaper than the alternative and is not a place to learn by doing.

## 3. Equipment — the bench, costed

Shared with CT characterisation (IMPL-02) and every later accuracy test, so this is **bench capex, not a cost of this decision.** It also closes OPEN-4.

| # | Item | Spec required | Buy (₹) | Rent (₹) | Verdict |
|---|---|---|---|---|---|
| E1 | **Reference power/energy meter** | **Class 0.2S or 0.5**, ≥4× better than ±2.0%, stated traceable uncertainty | 1.5–4 L | 15–30k/mo | **Rent or lab-hire for Phase 1.** Buy at Tier 4 when DVT needs it continuously |
| E2 | Isolation transformer | 1–2 kVA | 8–20k | — | **Buy.** Safety-critical |
| E3 | RCD/RCBO bench feed + enclosure | 30 mA | 5–12k | — | **Buy** |
| E4 | Variac | 0–270 V, 5–10 A | 5–15k | — | **Buy.** Covers the 180/230/260 V points |
| E5 | Differential probe | ≥700 V, ≥25 MHz | 25–60k | 8–15k/mo | **Buy.** Needed continuously through Phases 3–5 |
| E6 | Oscilloscope | 4 ch, ≥100 MHz | 60k–1.5 L | 10–20k/mo | Buy if absent |
| E7 | True-RMS clamp meter | ±1%, with PF | 5–15k | — | **Buy.** Sanity checks only — **not a reference** |
| E8 | Bench DMM | 5.5–6.5 digit | 30–80k | — | Buy if absent |
| E9 | Resistive load bank | Stepped to ~10 kW, guarded | 10–25k | — | **Build.** Heaters/lamps + contactors |
| E10 | Inductive load for PF 0.5 / 0.8 | Motor + switched capacitors | 15–40k | 10–20k/mo | Build or rent |
| E11 | Temperature chamber | 0 / +25 / +50 °C, for A16 | 1.5–4 L | 5–15k/day | **Rent by the day.** 2 days is enough |
| E12 | Thermocouples + logger | 4–8 ch, K-type | 8–20k | — | Buy |
| E13 | Programmable AC source | 0–300 V, programmable f | 3–8 L | 25–50k/mo | **Optional.** E2+E4 covers Phase 1 |
| E14 | Logging PC + isolated USB/serial | | 5–15k | — | Buy |

**Recommended Phase 1 kit: ₹1.4–2.6 L purchased + E1 and E11 hired.** That fits Tier 2 (₹3–8 L) alongside eval boards and CT samples.

**E1 is the one that cannot be compromised.** A reference no better than the device under test produces a number with no meaning. If the budget forces a choice, hire time at an accredited lab for the traceable points and use a lesser reference for the repeatable ones — and label every dataset with which reference produced it (§7).

## 4. Bench topology

```
  MAINS ─── RCD ─── ISOLATION TXFMR ─── VARIAC ───┬─── E1 REFERENCE METER ───┬─── LOAD BANK
                                                  │        (V and I)          │   (E9 resistive
                                                  │                           │    E10 inductive)
                                                  └───────── PRIMARY ─────────┘
                                                              │
                                        ┌─────────────────────┴──────────────────┐
                                        │   CT under test (same CT, every        │
                                        │   candidate — the AFE is the ONLY      │
                                        │   variable)                            │
                                        └─────────────────────┬──────────────────┘
                                                              │ CT secondary
                          ┌───────────────────────────────────┴────────────────┐
                          │  CANDIDATE EVAL BOARD (ADE7816 / ADE9078 / ATM90E32AS)
                          │  + shared isolated voltage reference (F4)          │
                          └───────────────────────────────────┬────────────────┘
                                                              │ SPI
                                                    E14 LOGGING PC (isolated)
```

**The controlled-variable rule:** the same CT samples, the same voltage-reference path, the same load points, the same operator, in the same session where possible. If the CT changes between candidates, the experiment measures nothing.

**Multi-device candidates:** both/all metering ICs are exercised in every run, with **at least one channel pair split across two devices**, because device-to-device consistency across the shared voltage reference (F4) is the failure mode unique to this topology and the one no datasheet will tell you about.

## 5. Reaching 63 A without a 63 A supply — the turns method

A 63 A resistive load at 230 V is ~14.5 kW. Most benches cannot supply it and most premises cannot either.

**A CT responds to ampere-turns.** Passing the primary conductor through the CT window *N* times makes a current *I* appear as *N × I*:

| Target | Turns | Actual current | Load |
|---|---|---|---|
| 63 A | 10 | 6.3 A | ~1.45 kW |
| 32 A | 8 | 4.0 A | ~0.92 kW |
| 16 A | 8 | 2.0 A | ~0.46 kW |
| 5 A | 5 | 1.0 A | ~230 W |
| 0.5 A | 5 | 0.1 A | ~23 W |

**The reference meter must measure the true single-turn current**, with the multiplication applied in analysis — never assume it.

**⚠ The turns method does not reproduce conductor position, geometry or the magnetic field of a real single heavy conductor.** It is valid for AFE comparison, where all candidates see the identical stimulus. It is **not** valid as final accuracy evidence. **≥3 points must be repeated single-turn at real current** before Gate 5, which is a lab-hire task, not a bench task. Record which method produced each row (§7).

## 6. Procedure

### 6.1 Per candidate — bring-up (S8 is measured here)

1. **Start a stopwatch.** Hours to first credible reading is the S8 developer-ecosystem score. Log honestly, including time lost to documentation gaps
2. Eval board unboxed, powered, host tool connected
3. SPI communication verified — read a known register, confirm the expected value
4. Raw uncalibrated readings from all channels with a known load
5. **Stop the clock.** Record hours, and every obstacle
6. Record measured supply current at the eval board's analogue and digital rails (A15)

### 6.2 Per candidate — calibration

Order matters. Phase calibration at unity PF is meaningless because phase error contributes almost nothing there.

1. **Gain, per channel:** unity PF, ~50% of range, adjust gain register until the channel matches the reference
2. **Phase, per channel:** **PF 0.5 lagging**, same current, adjust the phase-correction register until error is minimised. This is where CT phase error dominates and where parts genuinely differ
3. **Verify:** return to unity PF, confirm gain did not drift
4. **Record every coefficient.** Coefficient spread across channels and across devices is itself a result
5. Save the coefficient set to the dataset directory. **A calibration that cannot be reproduced is not a calibration**

### 6.3 Per candidate — the matrix

Every combination, all channels, **pre- and post-calibration**:

| Dimension | Points | Why |
|---|---|---|
| **Current** | 63 A · 32 A · 16 A · 5 A · 1 A · **0.5 A** | The bottom point is where split-core CTs and cheap AFEs fail. 0.5 A ≈ 0.8% of 63 A |
| **Power factor** | 1.0 · 0.8 lag · 0.5 lag | 0.5 is the discriminator |
| **Voltage** | 230 V nominal; plus 180 V and 260 V at 16 A / PF 1.0 | HWR-001 envelope |
| **Channels** | All 9, incl. ≥1 cross-device pair | F4 consistency |
| **Repeats** | 3 CT samples × 3 runs | Separates unit spread from repeatability |
| **Temperature** | 0 / +25 / +50 °C at 16 A, PF 1.0 and 0.5 | A16 / HWR-012. Chamber day |

**Settling:** ≥60 s at each point before recording; ≥30 min warm-up before the first. Cold readings are a known source of phantom differences between candidates.

### 6.4 Per candidate — timing and power

1. Full 9-channel register read, timed over ≥1,000 iterations: min, mean, max, jitter (A8)
2. Eval-board supply current at idle and during continuous acquisition (A15)
3. Thermocouple on each IC package after 1 h at 63 A equivalent (A16)

## 7. Data format

**One CSV per candidate.** Fixed schema — a schema invented per run is not comparable.

```csv
timestamp_iso,candidate,device_index,channel_id,ct_sample_id,run_index,
method,cal_state,ref_voltage_v,ref_current_a,ref_pf,ref_active_power_w,
dut_voltage_v,dut_current_a,dut_pf,dut_active_power_w,dut_apparent_va,
dut_reactive_var,error_pct,ambient_c,package_c,ref_instrument_id,
ref_uncertainty_pct,operator,notes
```

| Field | Values / notes |
|---|---|
| `candidate` | `ADE7816` · `ADE9078` · `ATM90E32AS` |
| `device_index` | 0-based — which physical IC. **The field that exposes device-to-device spread** |
| `method` | `single_turn` · `turns_N` (N = turns). Per §5, non-negotiable |
| `cal_state` | `precal` · `postcal` |
| `dut_reactive_var` | **Empty if the part does not provide it** — this is the [IMPL-01 §9](01_energy_meter_ic_evaluation.md#9--the-blocking-technical-question--potential-ecr) ECR question answered by data |
| `error_pct` | `(dut_active_power_w − ref_active_power_w) / ref_active_power_w × 100` |
| `ref_instrument_id` | Serial + calibration date. **A row without this is not evidence** |
| `ref_uncertainty_pct` | The reference's own uncertainty at that point |

**Runner requirements** (throwaway code, in whatever language the team is fastest in — it does not ship):

- Reads the candidate over SPI and the reference over its own interface; **never hand-transcribes numbers**
- Emits the schema above, one row per channel per point
- Prompts the operator for load/turns configuration changes and refuses to log until confirmed
- Writes continuously, so an aborted session keeps its data
- Directory: `benchmark/<candidate>/<YYYY-MM-DD>/` — raw CSV, coefficients, log book scan, photographs of the wiring

**Photograph every wiring configuration.** Three weeks later, "was that 8 turns or 10?" is unanswerable from a spreadsheet, and it invalidates the run.

## 8. Acceptance criteria

**Per candidate — proceed to scoring:**

- [ ] Full matrix completed, or every gap explicitly logged with a reason
- [ ] Post-calibration error ≤ **±2.5%** at 16 A, PF 1.0, all 9 channels
- [ ] Post-calibration error at 0.5 A **recorded**, whatever it shows
- [ ] Device-to-device spread quantified (multi-device candidates)
- [ ] 9-channel read time with ≥3× headroom against the aggregation budget
- [ ] Supply current and temperature drift recorded
- [ ] Every row carries `ref_instrument_id` and `method`

**For the benchmark as a whole:**

- [ ] ≥2 candidates completed. **A single-candidate benchmark is a justification, not an evaluation**
- [ ] Identical CTs and voltage reference across candidates
- [ ] Datasets committed to the repo
- [ ] [IMPL-01 §5](01_energy_meter_ic_evaluation.md#5-scoring--weights-fixed-before-any-measurement) scorecard completed with **the pre-registered weights**

## 9. Report template

```markdown
# AFE Benchmark Report — <candidate>
Operator · Dates · Reference instrument + serial + calibration date + uncertainty
Eval board P/N and revision · CT part and sample IDs · Firmware/host tool version

## 1. Bring-up
Hours to first credible reading: ___   (S8 score)
Obstacles:
Documentation gaps:

## 2. Calibration
Method · coefficients per channel · spread across channels · spread across devices
Reproducible? (second calibration from scratch, same result within ___%)

## 3. Accuracy
Error table: current × PF × channel, pre- and post-calibration
Worst-case error and where
0.5 A behaviour:
Device-to-device spread:
Temperature drift 0 / 25 / 50 °C:

## 4. Timing and power
9-channel read: min/mean/max/jitter · headroom ×___
Supply current idle / active · package temperature rise

## 5. Feature verification
Reactive power per channel: YES / NO   ← ECR trigger
Signed/quadrant reactive: YES / NO
Power factor register: YES / NO

## 6. Supply chain (IMPL-01 §6)
Table completed: Y/N · Lifecycle score: __/10 · Written statement: Y/N

## 7. Manufacturability (IMPL-01 §8)
Package confirmed: ______ · EMS capability confirmed: Y/N

## 8. Score
S1…S10 against pre-registered weights · Total __/100

## 9. Recommendation
Recommended / Not recommended, and the single most decisive reason
Known limitations of this benchmark:
```

## 10. What must not happen

- **Do not benchmark one candidate and stop** because it looks acceptable. That is confirmation, not evaluation — and it is the specific failure mode the preferred-hypothesis language in IMPL-01 exists to prevent
- **Do not skip 0.5 A** because it is tedious to set up. It is the point that decides the product
- **Do not change the scoring weights** after seeing results without logging who changed them and why
- **Do not use a clamp meter as the reference.** E7 is for sanity checks
- **Do not report a number without its reference instrument and uncertainty** (R14)
- **Do not present turns-method results as final accuracy evidence** (§5)

---

**On completion:** attach the datasets and reports to [IMPL-01](01_energy_meter_ic_evaluation.md) §13, complete the scorecard, close OPEN-1 with a named approver in [14](../14_decision_log.md), and advance *Metering AFE selection* to **Evidence ✅** in [12](../12_progress_tracker.md).
