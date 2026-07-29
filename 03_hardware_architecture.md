# 03 — Hardware Architecture (FROZEN)

**Document ID:** EIP-03 · **Version:** 1.0 · **Status:** 🔒 **FROZEN at Phase 0 exit** · **Owner:** Hardware
**Supersedes:** Hardware Architecture Decision, Software Architecture Decision (hardware portions), Product Family Strategy (V1 portions)

---

## 1. Freeze statement

This architecture is frozen. It may be changed only by a recorded decision in [14_decision_log.md](14_decision_log.md) with a named approver, a stated schedule impact, and a stated cost impact. "It would be better if…" is not a reason to reopen it. Only a **critical engineering or safety problem** is.

## 2. The frozen architecture

```
  Single-phase service conductor (230 V, 50 Hz)
            │
       ┌────┴──────────────────────────────────┐
       │                                       │
   1 × service CT              8 × branch CTs (one per labelled circuit)
       │                                       │
       └──────────────┬────────────────────────┘
                      │  protected analogue inputs (burden, clamp, anti-alias)
                      ▼
              multi-channel metering AFE  ◄──── isolated voltage-reference path
                      │  SPI                          (single, shared)
                      ▼
                    MCU  ──── secure element (device identity)
                      │
       ┌──────────────┼──────────────┬──────────────┐
       │              │              │              │
   Wi-Fi 2.4 GHz   BLE (commis-   status LED    external flash
   MQTT/TLS        sioning only)                 (7-day journal)
       │
       ▼
     Cloud

  Power: fused AC entry → surge protection → certified isolated AC/DC module → SELV rails

  ══════════ ISOLATION BARRIER ══════════
  Hazardous domain: AC entry, protection, isolated PSU primary, isolated voltage-reference primary
  SELV domain:      metering AFE, MCU, radio, flash, debug, LED, button, every customer-accessible interface
```

## 3. The eight frozen decisions

| # | Decision | Rationale |
|---|---|---|
| **F1** | **Split-core CTs**, one per conductor | Non-invasive retrofit, galvanic separation at the sensor, no shunt heat loss, maps naturally onto circuit labelling. Hall sensors add drift; shunts require interrupting the customer's conductor |
| **F2** | **1 service CT + 8 branch CTs**, single-phase only | A normal single-phase service has one live conductor to measure. *(The earlier "two mains CTs" wording is retired — it was ambiguous and could have led to a CT around live+neutral together.)* |
| **F3** | **A CT surrounds exactly one conductor.** Never live + neutral together | Opposing fields cancel; the reading is invalid. This is an installer training and commissioning-check point |
| **F4** | **One shared, isolated voltage-reference path** | All branches are on the same phase by definition of the V1 site criteria. Isolated potential transformer or equivalently qualified isolated path with primary fuse/protection, secondary protection, anti-aliasing and phase characterisation |
| **F5** | **Hazardous-domain content is minimised.** AFE, MCU, radio, debug, LED, button and all service interfaces are SELV | Makes the isolation boundary reviewable and keeps hazardous potential away from anything a human touches. A direct high-impedance divider into a hazardous-domain AFE is **rejected** and may only be revisited with a written isolation architecture and safety-review approval |
| **F6** | **No load switching. No relay, contactor or triac. Anywhere.** | Removes the highest-consequence failure mode, the largest liability, and a large slice of certification scope. Verified by schematic and BOM review, not by assertion |
| **F7** | **Pre-certified Wi-Fi/BLE module** with a valid Indian ETA | Avoids a separate radio authorisation costing ₹2–6 lakh and months of schedule. Do not lay out your own RF |
| **F8** | **Secure element** (or MCU-integrated equivalent) for non-exportable device identity | SEC-001 requires protected credential material. This is a BOM line, not a firmware trick |

## 4. Accuracy specification — two tiers

> **Decision D-003: ±1% across the full envelope was replaced with a two-tier specification.**

Low-cost split-core CTs show significant phase error below roughly 1% of rated current and meaningful unit-to-unit gap and permeability spread. Emporia publishes ±2% and they are not careless. Committing to ±1% everywhere sets up a crisis at DVT.

| Tier | Specification | Where it may be used |
|---|---|---|
| **Committed (HWR-003a)** | System active-energy error **≤ ±2.0%** across the full approved envelope after calibration | Sales material, datasheet, customer promise |
| **Stretch (HWR-003b)** | System active-energy error **≤ ±1.0%** within a narrower published envelope (nominal voltage, mid-to-high current, PF ≥ 0.8, 20–30 °C) | Only after a DVT report exists, and only with the envelope printed next to the number |

**HWR-007:** every published range or accuracy claim must identify topology, CT part number, calibration method, and the current/PF/temperature envelope with its reference standard. **"99% accurate" is prohibited** — it is not a metrology specification.

## 5. Electrical envelope

| Parameter | V1 value | Status |
|---|---|---|
| Nominal supply | 230 V AC, 50 Hz, single phase | Design input |
| Verification range | 180–260 V AC, 45–65 Hz | To be proven on a traceable bench |
| Service current | **63 A nominal** | A 100 A maximum claim is **deferred** pending CT, thermal and installation evidence — and must not appear in any document until then |
| Branch current | Per selected CT; range fixed at component selection | [09_component_selection.md](09_component_selection.md) |
| Internal sampling | Justified by the chosen AFE; thousands of samples/s, alias control proven at DVT | HWR-008 |
| Operating / storage | 0–60 °C / −20–70 °C planning target, indoor DB installation only | Confirmed at enclosure selection |

## 6. Site eligibility — reject rather than improvise

A site is **rejected** (a safety outcome, not a failed sale) when it has: three-phase or mixed-phase branch topology; insufficient CT clearance around conductors; inaccessible or damaged conductors; ambiguous neutral/phase identification; no space for the approved enclosure; no usable 2.4 GHz Wi-Fi and no acceptable extender position; or any condition the installer judges unsafe.

## 7. Data path architecture — the cost-critical decision

> **Decision D-004: telemetry is bundled and session-gated.** This resolves a ~2,700× cloud-cost difference.

| Tier | Content | Transport | Rate |
|---|---|---|---|
| **Live** | All 9 channels in **one bundled frame** per second | MQTT | **Only while an app live session is open**, with a server-side timeout |
| **Archive** | All 9 channels, 5-minute aggregates, one bundled frame | MQTT | Continuous — ~288 frames/device/day |
| **Journal** | 1-minute aggregates + events, packed binary | Local flash, uploaded on reconnect | 7 days retained |
| **Waveform** | Raw diagnostic capture | HTTPS to object storage | Exceptional; consent- or event-bound only |

**Why this matters, with the arithmetic:**
- Nine separate messages per second per device = 23.3 M messages and ~5 GB per device per month. At 1,000 devices: 23 billion messages and 5 TB/month.
- One bundled 5-minute frame = 8,640 messages and ~15 MB per device per month. At 1,000 devices: **15 GB/month.**

The live path is what customers see and love; it is also what would bankrupt the cloud bill if left always-on. Gate it on an open session.

## 8. What the architecture explicitly does **not** include

Relay or any switching path · three-phase support · neutral/earth CT (V1.1 candidate) · DC/solar measurement · Ethernet or cellular · display · battery backup · local web server · any customer-accessible interface in the hazardous domain.

## 9. Architecture risks carried into Phase 1

| Risk | Where tracked |
|---|---|
| Nine channels of metering AFE at target cost | [09_component_selection.md](09_component_selection.md), R-03 |
| Split-core CT phase error at low current | R-03 |
| Physical CT fit inside Indian distribution boards | R-13 — **the highest-value unknown in the project** |
| Per-CT calibration versus field CT replaceability | R-15 |

All tracked in [13_risk_register.md](13_risk_register.md).
