# 09 — Component Selection

**Document ID:** EIP-09 · **Version:** 1.0 · **Status:** Open — decisions close at Phase 1 exit · **Owner:** Hardware
**Supersedes:** component sections of Hardware Engineering, Cost/Manufacturing Analysis

---

## 1. How a component is selected

Every critical part is chosen against: **function fit · India availability and lead time · lifecycle/longevity status · second source · unit cost at 1k and 10k · certification impact · evaluation evidence.** A part with no second source is a risk-register entry, not a decision.

## 2. The metering AFE — the decision that shapes the BOM

> **This corrects a real error in the earlier documentation, which shortlisted only 3–4-channel parts for a 9-channel product.** Nine channels from 3-channel parts means three ICs and three SPI buses.

| Candidate | Channels | Note |
|---|---|---|
| **ADE7816** | **6 current + 1 voltage** | Purpose-built for exactly this branch-monitoring topology. **Must be benchmarked.** Two parts cover 9 channels with one voltage reference shared |
| ADE9000 / ADE9078 | 3-phase class, high performance | Higher cost; more capability than V1 needs |
| ATM90E32AS / E36 | 3 current + 3 voltage | Was the earlier baseline; needs 3 parts for 9 channels |
| STPM33 | 4 × 24-bit ADC | Good ADC; channel count still forces multiples |
| ADE9153A | Single-phase, autocalibration | Attractive autocalibration, wrong channel count for branches; possible service-CT-only role |
| MCU ADC + external AFE | n/a | Only if a benchmark proves it meets HWR-003a; otherwise rejected — do not build your own metrology |

**EVT deliverable (blocking):** a benchmark board comparing at minimum ADE7816 against the incumbent ATM90E32 across accuracy at low current, phase-error correction capability, per-channel calibration support, SPI throughput for 9 channels at the required sample rate, cost at 1k/10k, and India lead time. **Selection is made from measured data, not datasheet reading.**

## 3. Current transformers — the highest-risk part

| Criterion | Requirement |
|---|---|
| Type | Split-core, integrated burden preferred (open-circuit safety, HWR §7) |
| Ratings needed | Service CT sized for 63 A nominal; branch CTs sized per circuit-breaker classes found in the Phase 0 DB survey |
| **Physical fit** | **Must close around conductors inside real Indian distribution boards.** This is R-13, the highest-value unknown in the project |
| Phase error | Characterised at low current; correction capability must exist in the chosen AFE |
| Unit-to-unit spread | Measured across ≥20 samples of each candidate before selection |
| Field replacement | Must support one of the three MET-006 paths — matched pre-characterised part, guided recalibration, or flagged accuracy downgrade |
| Sourcing | ≥2 suppliers evaluated; a single-source CT is an unacceptable V1 position |

**MET-006 sourcing decision, to be closed at Phase 1 exit:** the preferred path is **pre-characterised matched parts sold as a spare** — it keeps the accuracy claim intact and avoids a field recalibration procedure that installers will get wrong. Fallback is guided in-app recalibration. Last resort is an explicit, visible accuracy downgrade on that channel.

## 4. Other critical parts

| Part | Selection rule |
|---|---|
| Wireless MCU module | Pre-certified 2.4 GHz Wi-Fi + BLE module with a **valid Indian ETA** (F7). ESP32-class baseline. Verify the ETA covers the exact part number and antenna variant |
| External flash | **16 MB minimum** (05 §5). Endurance checked against the FWR-010 write pattern |
| Secure element | ATECC608-class or MCU-integrated equivalent; non-exportable key storage (F8, SEC-001). Cost ₹40–90/unit — budgeted, not discovered later |
| Isolated voltage reference | Isolated potential transformer or equivalently qualified isolated path. Primary fuse, secondary protection, phase characterised |
| AC/DC power module | **Certified** isolated module. Do not design your own offline supply for V1 |
| Protection | Line fuse, MOV/surge network sized by the EMC and surge test plan |
| Connectors | Keyed, orientation-controlled CT harness (HWR-002). Keying is a safety and data-integrity feature |
| Enclosure | Flame-rated polymer, DIN or DB-mount, validated against ≥5 real DBs per approved type (HWR-010) |

## 5. Explicitly rejected

Hall-effect current sensing (drift, temperature coefficient) · shunts on customer conductors (requires interrupting the circuit, heat) · Rogowski coils (integration cost, low-current performance) · direct high-impedance divider into a hazardous-domain AFE (F5) · any relay, contactor or triac (F6) · non-pre-certified radio (F7) · flash below 16 MB.

Rationale for each is preserved in [future/Future_Hardware.md](future/Future_Hardware.md).

## 6. Output of this document

At Phase 1 exit this document must contain, for each critical part: manufacturer, part number, second source, unit cost at 1k and 10k, lead time, lifecycle status, evaluation evidence reference, and the named approver. **Until then, every row above is a candidate, not a choice.**
